
# Where does Permify fit into your enviroment?

Permify is a simply GRPC service (with an HTTP option) that responsible for managing and authorization in your enviroment, across your applications. 

Permify stores access control relations on a database you choose and performs authorization checks based on the stored relations. We called that database Write Database - **WriteDB** - and it behaves as a centralized data source for your authorization system. 

You can model your authorization with Permify's DSL - Permify Schema and your applications can talk to Permify API over REST API or GRPC Service to perform access control checks, read or query authorization-related data, or make changes to data stored in WriteDb. 

![relational-tuples](https://user-images.githubusercontent.com/34595361/186108668-4c6cb98c-e777-472b-bf05-d8760add82d2.png)

### Permify with Authentication 

Authentication involves verifying that the person actually is who they purport to be, while authorization refers to what a person or service is allowed to do once inside the system.

To clear out, Permify doesn't handle authentication or user management. Permify behave as you have a different place to handle authentication and store relevant data. Authentication or user management solutions (AWS Cognito, Auth0, etc) only can feed Permify with user informations (attributes, identities, etc) to provide more consistent authorization acrros your stack. 

### Permify with Identity Providers

Identity providers help you store and control your users’ and employees’ identities in a single place. 

Let’s say you build a project management application. And a client wants to connect this application via SSO. You need to connect your app to Okta. And your client can control who can access the application, and which group of authorization types they can have. But as a maker of this project management app. You need to build the permissions and then map to Okta. 

What we do is, help you build these permissions and eventually map anywhere you want.
