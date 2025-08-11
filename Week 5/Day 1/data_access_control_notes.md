# Week 5 Monday - 8/11/2025

## 1. Data Access Control in Unity Catalog
**Summary**  
Unity Catalog centralizes governance for all data assets in Databricks across workspaces. It provides fine-grained access control to tables, views, volumes, and functions, supporting data security and compliance. Access control is implemented through **Identity and Access Management (IAM)** concepts such as users, groups, and service principals.

**Key Points**
- Works at **metastore** level, governing all catalogs, schemas, and objects within.
- Uses **ANSI SQL GRANT/REVOKE** syntax for permissions.
- Supports **row- and column-level security**.
- Access is enforced consistently across all compute platforms (SQL, Python, R, Scala).

**Implementation Steps**
1. **Enable Unity Catalog** in the Databricks account console and assign a metastore to workspaces.
2. **Create a Catalog** → Create Schemas → Create Tables or other objects.
3. Assign permissions:
   ```sql
   GRANT SELECT ON TABLE my_catalog.my_schema.my_table TO `analyst_group`;
   ```
4. Use **Data Explorer** in UI or SQL to review and modify permissions.
5. For fine-grained controls, configure **row filters** and **column masks**.

---

## 2. Importing from Identity Providers
**Summary**  
Identity Providers (IdPs) like Microsoft **Entra ID** (formerly Azure AD), Okta, or AWS IAM Identity Center act as the source of truth for authentication. Databricks can sync users, groups, and service principals from these providers.

**Key Points**
- Single Sign-On (SSO) integration enables centralized authentication.
- **SCIM (System for Cross-domain Identity Management)** automates provisioning/de-provisioning.
- Reduces manual account management and enforces corporate security policies.

**Implementation Steps**
1. In **Account Console** → **Identity Federation** → Choose IdP integration.
2. Configure **SCIM provisioning** in the IdP (e.g., Entra ID Enterprise App → Provisioning → Endpoint + Token from Databricks).
3. Test **user sync** to confirm roles and groups are imported.
4. Enable **SSO** in IdP with Databricks application.
5. Verify that imported identities appear in the Databricks **Admin Console**.

---

## 3. User–Group Relationship Management
**Summary**  
Groups simplify permission management by assigning roles to multiple users at once. Best practice is to assign access at the group level, not individual users.

**Key Points**
- Groups can be synced from IdP or created manually in Databricks.
- Nested groups may be supported depending on IdP integration.
- Service principals can also be assigned to groups.

**Implementation Steps**
1. Navigate to **Admin Console → Groups**.
2. **Create group** manually or verify synced group from IdP.
3. Add users/service principals to the group.
4. Assign permissions to the group at Unity Catalog, workspace, or cluster level.
5. Review membership regularly and automate via SCIM if possible.

---

## 4. Admin Roles
**Summary**  
Admin roles define elevated privileges for managing Databricks resources.

**Common Roles**
- **Account Admin** – Full control over all workspaces, billing, identity federation, and Unity Catalog metastores.
- **Workspace Admin** – Manages a specific workspace (clusters, permissions, workspace objects).
- **Metastore Admin** – Manages catalogs, schemas, and data permissions in Unity Catalog.
- **Cluster/SQL Admins** – Manage compute resources and SQL warehouses.

**Implementation Steps**
1. In **Account Console** → **Users** → Assign **Account Admin** role if needed.
2. In **Workspace Admin Console** → Manage **Workspace Admins**.
3. For Unity Catalog:
   ```sql
   GRANT CREATE CATALOG ON METASTORE TO `data_stewards`;
   ```
4. Document role assignments to ensure least-privilege principle.

---

## 5. Workspace APIs
**Summary**  
Workspace APIs allow automation of administrative tasks such as cluster creation, job scheduling, permissions management, and object imports.

**Key Points**
- REST API endpoints are organized into categories: Clusters, Jobs, Workspace, Unity Catalog, SCIM.
- Common uses:  
  - Create/update clusters programmatically  
  - Import/export notebooks  
  - Manage permissions at scale

**Implementation Steps**
1. **Generate a personal access token** in **User Settings** or use OAuth for service principals.
2. Use the [Databricks REST API docs](https://docs.databricks.com/api) to find the needed endpoint.
3. Example: List clusters  
   ```bash
   curl -X GET https://<workspace-url>/api/2.0/clusters/list      -H "Authorization: Bearer <TOKEN>"
   ```
4. Integrate API calls into automation scripts (Python, Bash, Terraform).

---

## 6. Workspace Backup and Recovery
**Summary**  
Backup and recovery protect against accidental deletions, corruption, or disaster scenarios. While Databricks manages infrastructure resilience, customers are responsible for backing up notebooks, configurations, and Unity Catalog data.

**Key Points**
- Use the **Databricks CLI** or APIs to export notebooks and configs regularly.
- Unity Catalog metadata is backed up automatically, but data in external storage must follow your organization’s backup policies.
- Version control with Git is highly recommended.

**Implementation Steps**
1. **Export notebooks**:
   ```bash
   databricks workspace export_dir /Shared /local/backup
   ```
2. **Export job definitions** via API or CLI.
3. For data:
   - If using Delta Lake on cloud storage, ensure **version history** (time travel) is enabled.
   - Backup cloud storage data to another bucket/container.
4. Document a **recovery procedure**:
   - Restore notebooks from local/Git backup.
   - Restore tables using Delta Lake time travel (`VERSION AS OF`).
   - Recreate clusters/jobs from stored configurations.


---

## Resources

- https://docs.databricks.com/aws/en/data-governance/unity-catalog/access-control
- https://docs.databricks.com/aws/en/data-governance/unity-catalog/manage-privileges/