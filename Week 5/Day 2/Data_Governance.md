# Week 5 Tuesday - 8/12/2025

## 1. Managing Principals in Unity Catalog
**Summary**  
In Unity Catalog (UC), a *principal* is an entity that can be granted privileges: a **user**, **group**, or **service principal**. Managing principals involves creating, updating, and removing them, usually via identity federation from an IdP like Microsoft Entra ID, Okta, or AWS IAM Identity Center.

**Key Points**
- **Users** – human identities authenticated via SSO.
- **Groups** – collections of principals; best practice for permission assignments.
- **Service principals** – non-human identities for automation, apps, or pipelines.
- Prefer IdP sync via **SCIM** for consistent lifecycle management.
- Principals are **account-level** in UC, so they work across all workspaces attached to the same metastore.

**Implementation Steps**
1. **Enable identity federation** in Account Console.
2. **Connect IdP** and configure SCIM provisioning for automatic sync.
3. Manage principals in UI (**Account Console → Identity and access**) or via SCIM API endpoints.
4. Assign privileges to **groups**, not individual users, for consistency and easier management.
5. Remove unused principals via IdP to automatically deprovision in UC.

---

## 2. Data Governance
**Summary**  
Data governance ensures data security, compliance, and proper usage. In UC, this includes **access control**, **auditing**, **lineage**, and **policy enforcement**.

**Best Practices**
- Model **catalogs** and **schemas** logically by domain.
- Grant to **groups** instead of individuals.
- Implement **least privilege** and use **future grants** for scalability.
- Apply **row filters** and **column masks** for sensitive fields.
- Use **lineage tracking** and **audit logs** for transparency.
- Keep permissions and policies version-controlled.

**Implementation Steps**
1. Define data domains in catalogs and schemas.
2. Attach workspaces to the UC metastore.
3. Grant baseline access (catalog `USAGE`, schema `USAGE` + `SELECT` as needed).
4. Enable and monitor lineage in Catalog Explorer.
5. Regularly review grants via `SHOW GRANTS` and system audit tables.

---

## 3. Row Level Filters
**Summary**  
Row-level security (RLS) restricts which rows a user can see based on attributes like group membership or column values. In UC, this is implemented with **row filters**—functions that return a BOOLEAN to determine visibility.

**Implementation Example**
```sql
-- Create security schema
CREATE SCHEMA IF NOT EXISTS main.sec;

-- Function to allow analysts only USA rows
CREATE OR REPLACE FUNCTION main.sec.only_usa(country STRING)
RETURN (NOT is_account_group_member('analysts') OR country = 'USA');

-- Attach to table
ALTER TABLE main.sales.orders
SET ROW FILTER main.sec.only_usa ON (country);
```

**Notes**
- Use `is_account_group_member()` for account-level groups.
- Row filters apply automatically to all queries on the table.
- Check filters with:
```sql
DESCRIBE EXTENDED main.sales.orders;
```

---

## 4. Column Masks
**Summary**  
Column masks hide or transform values based on the user querying them. They are useful for protecting sensitive columns (PII, financial data).

**Implementation Example**
```sql
-- Mask function: show real value only to data stewards
CREATE OR REPLACE FUNCTION main.sec.mask_ssn(ssn STRING)
RETURN CASE WHEN is_account_group_member('data_stewards') THEN ssn ELSE sha2(ssn, 256) END;

-- Apply mask to column
ALTER TABLE main.hr.employees
ALTER COLUMN ssn SET MASK main.sec.mask_ssn;
```

**Notes**
- Masks are evaluated before returning results to the user.
- Can return NULL, hashed, or bucketed values depending on your policy.
- Mask definitions are visible in `DESCRIBE EXTENDED`.

---

## 5. Secret Management
**Summary**  
Secrets store sensitive values (e.g., API keys, passwords) securely. In Databricks, secrets are stored in **secret scopes** and accessed without exposing the values in code.

**Key Points**
- Secret scopes can be **Databricks-backed** or backed by cloud services (Azure Key Vault, AWS Secrets Manager).
- Access to secrets is controlled via scope-level ACLs.

**Implementation Steps**
1. Create a secret scope:
```bash
databricks secrets create-scope --scope my-scope
```
2. Add a secret:
```bash
databricks secrets put --scope my-scope --key db-password
```
3. Access in a notebook:
```python
dbutils.secrets.get(scope="my-scope", key="db-password")
```
4. Manage permissions:
```bash
databricks secrets put-acl --scope my-scope --principal analysts --permission READ
```

---

## 6. Model Governance
**Summary**  
Model governance ensures ML models are managed, versioned, secured, and compliant. In Databricks, use the **Unity Catalog–integrated MLflow Model Registry**.

**Key Points**
- Models are versioned and can be in stages: `Staging`, `Production`, `Archived`.
- Access control via Unity Catalog privileges.
- Lineage shows datasets and code used to train models.
- Approval workflows can be enforced before production promotion.

**Implementation Steps**
1. Enable UC-backed Model Registry.
2. Register a model:
```python
mlflow.register_model("runs:/<run-id>/model", "main.ml_models.customer_churn")
```
3. Assign permissions:
```sql
GRANT EXECUTE ON MODEL main.ml_models.customer_churn TO `ml_engineers`;
```
4. Use Catalog Explorer for lineage and audit logs.
5. Define a promotion process with required reviews.
