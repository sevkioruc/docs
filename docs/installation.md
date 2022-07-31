---
sidebar_position: 4
---

# Installation Guide

This guide shows how Permify works and how to fully implement it. Since this is a beta version of Permify, installation and implementation alternatives can be differ in further. 

## Quick Recap

To give a quick recap for Permify, it's an open-source authorization service that you can run with docker and works on a Rest API.

Permify converts, coordinates, and sync your authorization data as relation tuples into your preferred database. And you can check authorization with a single request based on those tuples.

## Installation

Permify (Beta version) only support Docker for installation right now. 

### Container (Docker)

You can use Permify on your server by running Permify on docker. There are 2 alternatives for that:
 
- [With Using Terminal](#with-using-terminal)
- [With Using Docker Desktop](#with-using-docker-desktop) 

#### With Using Terminal

**1.** Open your terminal.

**2.** Run following line.

```shell
docker run -d -p 3476:3476 --name permify-container -v {YOUR-CONFIG-PATH}:/config permify/permify:0.0.1
```

Above config path - *{YOUR-CONFIG-PATH}* - addresses ***"config.yaml"*** file, where you configure databases to store and coordinate your authorization data. We provide a YAML file to define database that will store your authorization data. 

Check out [Synchronize Authorization Data] section to learn how to create this config YAML file and get more details  about how Permify centralize your authorization data.

[Synchronize Authorization Data]:  /docs/getting-started/sync-data

**3.** Test your connection.
    - Create an HTTP GET request ~ localhost:3476/v1/status/ping 

***Note:*** localhost:3476 is an example base path, you can run Permify API anywhere you want.

#### With Using Docker Desktop

Setup docker desktop, and run service with the following steps;

1. Open your docker account.
2. Open terminal and run following line

```shell
docker pull permify/permify:0.0.1
```

3. Open images, and find Permify.
4. Run Permify with the following credentials (optional settings)
    - **Container Name:** authorization-container
      
      *Ports:*
    - **Local Host:** 3476
      
      *Volumes:*
    - **Host Path:** choose the config file's (which addresses *"config.yaml"*) folder.
    - **Container Path:** /config
5. Test your connection.
    - Create an HTTP GET request ~ localhost:3476/v1/status/ping

## Building Permify Schema (Modeling)

After installation, you need to model your authorization with Permify Schema and condigure it to Permify API by sending schema file. 

You can define your entities, relations between them and access control decisions of each actions with using Permify Schema.

### Creating your model

Firsly create a file with extension ***".perm"***. This created file is our Permify Schema file.

To give an example for this guide, we'll be using following user-organization case. 

```perm
entity user {} 

entity organization {

    relation admin @user    
    relation member @user     
    
    action add_member = admin or member
    action delete_member = admin 
    action delete = admin

} 
```

We have 2 entities these are **"user"** and **"organization"**.

Organization entity has its own relations (admin and member) which related with user entity.And have access control decision points with rules:  

*Everybody can add member to a organization but only members can remove them, Additionally deleting access granted to users with admin role in organization.*

***Note:*** defining user entity is mandatory when creating Permify Schema*

### Configuring Schema on Permify 

After you finished modeling you need to send Permify Schema file to API endpoint **/schemas/replace"** for configuration of your authorization model on Permify API.

#### Path : **"/schemas/replace" POST**
| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | schema | file | - | Permify Schema file|

You can find mode information on [Modeling Authorization with Permify] section.

[Modeling Authorization with Permify]: /docs/getting-started/modeling

## Storing Relational Tuples

After Permify API is running on your server and you completed modeling of your authorization via Permify Schema. Its time to add authorizations data to see Permify in action. 

As we mentioned Permify stores your authorization data in a database you prefer. We called that database as WriteDB, and you can define it with using our YAML config file.

### Example config.yaml file

```yaml
app:
  name: ‘permify’
  version: ‘0.0.1’
http:
  port: ‘3476’
logger:
  log_level: ‘debug’
  rollbar_env: ‘permify’
database:
  write:
    connection: postgres
    pool_max: 2
    url: ‘postgres://user:password@host:5432/database_name’
```

This configuration file's path is used on docker to address database that authorization data unifies (writeDB).

There are 2 approaches manage (add, delete, update) authorization data in Permify;

 - [Creating Custom Relational Tuples](https://github.com/Permify/permify/blob/master/assets/content/SYNC.md#creating-custom-relational-tuples) 
 - [With Change Data Capture](https://github.com/Permify/permify/blob/master/assets/content/SYNC.md#with-change-data-capture)

You can find more detailed explanation and implementation for these from [Move & Synchronize Authorization Data] section. For this guide we'll go with creating relational tuples with Permify API.

[Move & Synchronize Authorization Data]: /docs/getting-started/sync-data

You can create custom relational tuples with using "/v1/relationships/write" endpoint. For our guide let's grant one of the team members (Ashley) an admin role. 

### Grant Administration Role to Ashley 

Request

```json
{
  "entity": "organization",
  "object_id": "1", //Organization identifier
  "relation": "admin",
  "userset_entity": "",
  "userset_object_id": "1", //Ashley's identifier
  "userset_relation": ""
}
```

Response

```json
{
  "message": "success"
}
```

Created relational tuple: ***organization:1#admin@1***

Definition: user 1 (Ashley) has admin role on organization 1.

## Checking Permissions

# Access Control Check

You can check authorization with
single API call. This check request returns a decision about whether user can perform an action on a certain resource.

Access decisions generated according to relational tuples, which stored in your database (writeDB) and [Permify Schema] action conditions.

[Permify Schema]: /docs/getting-started/modeling

## Example Check

Lets examine a example access control decision on our organization example: 

***Can the user X delete member on organization Y ?***

### Path: 

POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | user | string | - | the user or user set who wants to take the action.
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | object | string | - | name and id of the resource. Example: repository:1” |
| [ ]   | depth | integer | 8 | |

### Request

```json
{
  "user": "1",
  "action": "delete_member",
  "object": "organization:1"
}
```

### Response

```json
{
  "can": true,
  "debug": "user 1 can delete_member on organization 1"
}
```