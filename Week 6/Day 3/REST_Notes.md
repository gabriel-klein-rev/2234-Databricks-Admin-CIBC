# REST
## REST Introduction

- What is REST?
    - REpresentations State Transfer
    - Coined by Roy Fielding, in 2000
    - Representational State Transfer (REST) is a software architecture that imposes conditions on how an API should work. REST was initially created as a guideline to manage communication on a complex network like the internet. [Amazon](https://aws.amazon.com/what-is/restful-api/)
    - REST emphasizes scalability of component interactions, generality of interfaces, independent deployment of components, and intermediary components to reduce interaction latency, enforce security, and encapsulate legacy systems. [Roy Fieldings Dissertation](https://www.ics.uci.edu/~fielding/pubs/dissertation/abstract.htm)  

- Principles of REST [knowledgehut.com](https://www.knowledgehut.com/blog/programming/rest-api)
    - Client-Server Decoupling: In a REST API design, client and server programs must be independent. The client software should only know the URI of the requested resource; it should have no additional interaction with the server application.
    - Uniform Interface: All API queries for the same resource should look the same regardless of where they come from. The REST API should ensure that similar data, such as a user's name or email address, is assigned to just one uniform resource identifier (URI).
    - Statelessness: REST APIs are stateless, meaning each request must contain all the information needed to process it.
    - Layered System Architecture: REST API requests and responses are routed through many tiers. REST APIs must be designed so neither the client nor the server can tell whether they communicate with the final application or an intermediary.
    - Cacheable: Wherever feasible, resources should be cacheable on the client or server side. Server responses must additionally indicate if caching is authorized for the offered assistance. The objective is to boost client-side speed while enhancing server-side scalability.
    - Code on Demand: REST APIs typically provide static resources, but in rare cases, responses may include executable code (such as Java applets). In these cases, perform the code when necessary.

- [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)
    How "RESTful" is an application?
    - Level 0: Implement HTTP
    - Level 1: Leverage Resources (make resources the target of specific requests, to interact with the resource more directly)
    - Level 2: HTTP Verbs (use the safest HTTP method for a given action. Use GET rather than POST when possible, etc.)
    - Level 3: Hypermedia Controls / HATEOAS (Hypermedia As The Engine Of Application State) (resonses to a HTTP request can contain URIs to other resources or possible next steps)

---

## HTTP

- What is the purpose of HTTP?
    - Communication with vs without protocols  

- HTTP Messaging Cycle  

    | build request -> | transmit -> | recieve request -> | compute -> | build response -> | transmit -> | recieve response |
    | ---------------- | ----------- | ------------------ | ---------- | ----------------- | ----------- | ---------------- |
    | client | ISP | server | server | server | ISP | client |  

- HTTP Methods
    - GET
    - HEAD
    - PUT
    - POST
    - DELETE
    - OPTIONS
    - PATCH
    - TRACE

- Safe and Idempotency
    - Safe: Method can be run with no effect on the data.
    - Idempotency: Method can be run multiple times, and after the first time the effect on the data will not change.  
        (ie. a record can only be deleted once, no matter how many times the request is sent)

        | Method | Idempotent | Safe |
        | ------ | ---------- | ---- |
        | GET | YES | YES |
        | HEAD | YES | YES |
        | OPTIONS | YES | YES |
        | TRACE | YES | YES |
        | DELETE | YES | NO |
        | PUT | YES | NO |
        | POST | NO | NO |
        | PATCH | NO | NO |  

- HTTP Message Structure
    - Head Structure
        - Request-line  
        {METHOD} {URI} {VERSION}  
        GET https://www.google.com HTTP/1.1  

        - Headers (General | Request | Entity)  
        Host: www.tutorialspoint.com  
        User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)  
        Accept-Language: en-us  
        Accept-Encoding: gzip, deflate  
        Connection: Keep-Alive  

            Headers can be paired with the request-line to be more specific about the request being made.  
            Note Host header and URI in request line: 

            POST /cgi-bin/process.cgi HTTP/1.1  
            User-Agent: Mozilla/4.0 (compatible; MSIE5.01; Windows NT)  
            Host: www.tutorialspoint.com  
            Content-Type: application/x-www-form-urlencoded  
            Content-Length: 106  
            Accept-Language: en-us  
            Accept-Encoding: gzip, deflate  
            Connection: Keep-Alive  

        - An empty line indicating the end of the header fields
    - Optionally a message-body (format and length *metadata* declared and described in the headers)  

- HTTP Response Codes
    - 100s Informative
    - 200s Confirmation
    - 300s Redirection
    - 400s Client Error
    - 500s Server Error

---

## Authorization and Authentication

- Define and contrast authentication and authorization.
    - Authentication and credentials should be introduced as a way to verify a users identity.
    - Authorization should be introduced as a level of action allowed to a verified user.
- Athentication standards
    - OIDC - OpenID Connect (OIDC) is an authentication protocol that allows users to access third-party websites and apps without having to reshare their sign-in information. OIDC is built on top of the OAuth 2.0 framework and uses JSON web tokens (JWTs). OIDC allows users to use single sign-on (SSO) to access relying party sites using OpenID Providers (OPs), such as an email provider or social network, to authenticate their identities.
    - SAML - Security Assertion Markup Language (SAML) is an open standard for sharing security information about identity, authentication, and authorization across different systems. SAML is implemented with the Extensible Markup Language (XML) standard for sharing data. SAML provides single sign-on across multiple domains, allowing users to authenticate only once. Users gain access to multiple resources on different systems by supplying proof that the authenticating system successfully authenticated them.
    - OAuth 2.0 (oh-auth) - an open standard that allows users to grant websites or applications access to their information on other websites without entering passwords. For example, you can tell Facebook that it's OK for ESPN.com to access your profile or post updates to your timeline without having to give ESPN your Facebook password. OAuth 2.0 is now the de facto industry standard for online authorization. OAuth 2.0 is called an authorization “framework” rather than a “protocol” since the core spec leaves room for various implementations to do things differently depending on their use cases.
- Authentication services - "Identity management platforms" (like Auth0, Okata, or Amazon Cognito) implement industry authentication standards like OIDC, SAML, and OAuth 2.0 to provide developers implementation of those standards and features like SSO which may be dificult to implement when built into an application. 
- Authorization - Which type of user is allowed to take a specific action? Can an "employee" user approve or reject an expense report, or is that action reserved for a "manager" type user? How is the users permissions level verified? Does it happen with each request, or is every request after authentication assumed to be from the same user?
- Zero Trust Architecture (ZTA) - "never trust, always verify" - ZTA is an enterprise cybersecurity architecture that is based on zero trust principles. It is designed to prevent data breaches and limit internal lateral movement. ZTA enforces access policies based on context, including the user's role and location, their device, and the data they are requesting. It works by assuming that every connection and endpoint is considered a threat. ZTA aims to protect organizations from advanced threats and data breaches while assisting in compliance with FISMA, HIPAA, GDPR, CCPA, and other core data privacy or security laws.