---
sidebar_position: 3
title: "Set Up In Your Enviroment"
---

# Set Up Permify

Permify is an open-source authorization service that you can run in your enviroment and works as a Rest API. This guide shows how to set up Permify in your servers and use it accross your applications. Set up and implementation consists of 4 steps,

1. [Set Up Permify Service](#run-permify-api)
2. [Model your Authorization with Permify's DSL, Permify Schema](#model-your-authorization-with-permify-schema)
3. [Migrate and Store Authorization Data as Relational Tuples](#store-authorization-data-as-relational-tuples)
4. [Perform Access Check](#perform-access-check)

:::info
Want to walk through this example 1x1 rather than docs ? [schedule a call with one of our Permify engineers](https://calendly.com/ege-permify/30min).
:::

## Set Up Permify Service

You can run Permify API on your server by pulling Permify container image. 

<details>
<summary>With Using Terminal</summary>
<p>

**1.** Open your terminal.

**2.** Run following line.

```shell
docker run -d -p 3476:3476 --name permify-container -v {YOUR-CONFIG-PATH}:/config ghcr.io/permify/permify
```

This will start an HTTP server with the configuration options. Port 3476 is used to serve the REST API.

Above config path - {YOUR-CONFIG-PATH} - addresses "config.yaml" file, where you can configure database to store and coordinate your authorization data. 

Permify stores your authorization data in a database you prefer as relation tuples. We called that database **‘writeDB’**, and you can define it using our YAML file.

*** Example config.yaml file *** 

```yaml
app:
  name: 'permify-demo'

http:
  port: 3476

logger:
  log_level: 'debug'
  rollbar_env: 'permify'

database:
  write:
    connection: 'postgres'
    database: 'db_name'
    uri: 'postgres://user:password@host:1241'
    pool_max: 20
```

#### **database:**
* **write:** Points out where your want to store your authorization data (relation tuples, audits, decision logs, authorization model )
    * **connection:** Data source. Permify supports **MongoDb** (`'mongo'`) and **PostgreSQL**(`'postgres'`) for now. Contact with us for your preferred database.
    * **database:** Custom database name.
    * **uri:** Uri of your data source.
    * **pool_max:** Max connection pool size.


Check out [Centralize Authorization Data] section to learn how to organize this config YAML file and get more details  about how authorization data stored.

[Centralize Authorization Data]:  /docs/getting-started/sync-data

**3.** Test your connection.
    - Create an HTTP GET request ~ localhost:3476/v1/status/ping

</p>
</details>

<details><summary>With Using Docker Desktop</summary>
<p>

Run Permify service with the following steps;

**1.** Open terminal and run following line

```shell
docker pull ghcr.io/permify/permify
```
**2.** Open your [docker desktop](https://docs.docker.com/get-docker/).

**3.** Open images, and find Permify.

![found-image](https://user-images.githubusercontent.com/34595361/193456544-c58e53a4-a257-4787-8ef9-2470a5c21fe5.png)

**4.** Hit the Run button and choose optional setting to run Permify container.

![found-image](https://user-images.githubusercontent.com/34595361/193477621-4477ebdb-4a19-4d6d-b5ee-159aba1740d5.png)

Above **Host path** addresses the folder of **config.yaml** file, where you configure databases to store and coordinate your authorization data. 

Permify stores your authorization data in a database you prefer as relation tuples. We called that database **‘writeDB’**, and you can define it using our YAML file.

*** Sample config.yaml file *** 

```yaml
app:
  name: 'permify-demo'

http:
  port: 3476

logger:
  log_level: 'debug'
  rollbar_env: 'permify'

database:
  write:
    connection: 'postgres'
    database: 'db_name'
    uri: 'postgres://user:password@host:1241'
    pool_max: 20
```

#### **database:**
* **write:** Points out where your want to store your authorization data (relation tuples, audits, decision logs, authorization model )
    * **connection:** Data source. Permify supports **MongoDb** (`'mongo'`) and **PostgreSQL**(`'postgres'`) for now. Contact with us for your preferred database.
    * **database:** Custom database name.
    * **uri:** Uri of your data source.
    * **pool_max:** Max connection pool size.

Check out [Centralize Authorization Data] section to learn how to organize this config YAML file and get more details  about how authorization data stored.

[Centralize Authorization Data]:  /docs/getting-started/sync-data

**5.** Test your connection.
    - Create an HTTP GET request ~ localhost:3476/v1/status/ping


</p>
</details> 


## Model your Authorization with Permify Schema

After installation completed and Permify API running, you need to model your authorization with Permify Schema and condigure it to Permify API by sending schema file. 

You can define your entities, relations between them and access control decisions of each actions with using Permify Schema.

### Creating your authorization model

Firsly create a file with extension ***".perm"***. This will be our Permify Schema file. Let's create our authorization model. We'll be using following user-organization authorization case for this guide. 

```perm
entity user {} 

entity organization {

    relation admin @user    
    relation member @user     
    
    action view_files = admin or member
    action edit_files = admin

} 
```

We have 2 entities these are **"user"** and **"organization"**. Entities represents your main tables. We strongly advise naming entities the same as your original database entities. 

Lets roll back our example, 

- The `user` entity represents users. This entity is empty because it's only responsible for referencing users.

- The `organization` entity has its own relations (`admin` and `member`) which related with user entity. This entity also  has 2 actions, respectively:
  - Organization member and admin can view files.
  - Only admins can edit files.

:::info
For implementation sake we'll not dive more deep about modeling but you can find more information about modeling on [Modeling Authorization with Permify] section. Also can check out [example use cases] to better understand some basic use cases modeled with Permify Schema. 

[Modeling Authorization with Permify]: /docs/getting-started/modeling
[example use cases]: /docs/example-use-cases/simple-rbac
:::

### Configuring Permify Schema on API 

After modeling completed, you need to send Permify Schema file (.perm extension file) to API endpoint **/v1/schemas/write"** for configuration of your authorization model on Permify API.

#### Path : ** POST "/schemas/write"**
| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | schema | file | - | Permify Schema file|

**Example Request on Postman:**

![permify-schema](https://user-images.githubusercontent.com/34595361/185436333-77c2c9b7-5537-495c-a4de-e3ef7371d24d.png)

## Store Authorization Data as Relational Tuples

After you completed configuration of your authorization model via Permify Schema. Its time to add authorizations data to see Permify in action. 

As we mentioned Permify stores your authorization data in a database you prefer. We called that database as WriteDB, and you can define it with using our YAML config file.

### Example config.yaml file

```yaml
app:
  name: 'permify'

http:
  port: 3476

logger:
  log_level: 'debug'
  rollbar_env: 'permify'

database:
  write:
    connection: 'postgres'
    database: 'db_name'
    uri: 'postgres://user:password@host:1241'
    pool_max: 20
```

This configuration file's path is used on docker to address database that authorization data unifies (writeDB).

### Create Relational Tuples

You can create relational tuples as authorization rules at this writeDB by using `/v1/relationships/write` endpoint.

For our guide let's grant one of the team members (Ashley) an admin role. 

**Request:** POST - `/v1/relationships/write` 

```json
{
    "entity": {
        "type": "organization",
        "id": "1" //Organization identifier
    },
    "relation": "admin",
    "subject": {
        "type": "user",
        "id": "1", //Ashley's identifier
        "relation": ""
    }
}
```

**Created relational tuple:** organization:1#admin@1

**Semantics:** User 1 (Ashley) has admin role on organization 1.

:::info
You can find more detailed explanation from [Move & Synchronize Authorization Data] section.

[Move & Synchronize Authorization Data]: /docs/getting-started/sync-data
:::

### Performing Access Control Check

You can check authorization with
single API call. This check request returns a decision about whether user can perform an action on a certain resource.

Access decisions generated according to relational tuples, which stored in your database (writeDB) and [Permify Schema] action conditions.

[Permify Schema]: /docs/getting-started/modeling

## Perform Access Check

Finally we're ready to control authorization. Lets perform an example access check via [check] API. 

[check]: ./api-overview/check-api.md

***Can the user 45 view files on organization 1 ?***

### Path: 

POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity | object | - | name and id of the entity. Example: organization:1”.
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | subject | object | - | the user or user set who wants to take the action  |
| [ ]   | schema_version | string | - | get results according to given schema version|
| [ ]   | depth | integer | 8 | |

### Request

```json
{
    "entity": {
        "type": "organization",
        "id": "1"
    },
    "action": "view_files",
    "subject": {
        "type":"user",
        "id": "45"
    }
}
```

### Response

```json
{
    "can": true,
    "remaining_depth": 5,
    "decisions": {
        "organization:1#admin": {
            "can": true,
            "err": null
        }
    }
}
```

See [Access Control Check] section for learn how access checks works and access decisions evaluated in Permify

[Access Control Check]: ./getting-started/enforcement.md

## Need any help ?

Our team is happy to help you get started with Permify. If you struggle with installation or have any questions, [schedule a call with one of our Permify engineers](https://calendly.com/ege-permify/30min). Alternatively you can join our [discord community](https://discord.com/invite/MJbUjwskdH) to discuss.