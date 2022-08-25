---
sidebar_position: 2
---

# Centralizing Authorization Data

Permify unifies your authorization data in a database you prefer. We named that database as Write Database, shortly **WriteDB**.

Permify API provides various functionalities - checking access, reasoning permissions etc - to maintain separate access control mechanisms for individual applications/services. 
WriteDB stands as a source of truth for these authorization functionalities.

## Configuration

You can configure WriteDB on Permify Container configuration YAML file.  

**Example config file:**

```yaml
app: 
  name: ‘permify’ 
http:
  port: 3476
logger:
  log_level: ‘debug’
database:
  write:
    connection: 'mongo'
    database: 'github-sample'
    uri: 'mongodb://172.17.0.2:27017'
    pool_max: 2
tracer:
  exporter: 'zipkin'
  endpoint: 'http://172.17.0.4:9411/api/v2/spans'
  disabled: false

```
### **app**
* **name:** Name of your application.

### **http**
* **port:** port of the writeDB run on.

### **logger**
* **log_level:** There are 4 different types of log levels; `'debug'`, `'error'`, `'warn'`, `'info'`. Permify uses [zerolog] for logging.

[zerolog]: https://github.com/rs/zerolog

### **database**
* **write:** Points out where your want to store your authorization data (relation tuples, audits, decision logs, authorization model )
    * **connection:** Data source, Permify supports **MongoDb** (`'mongo'`) and **PostgreSQL**(`'postgres'`) right now. Contact with us for your preferred database.
    * **database:** Custom database name.
    * **uri:** Uri of your datasource.
    * **pool_max:** Max connection pool size.

### **tracer** (optional)
* **exporter:** Permify integrated with [jaeger] , [signoz] and [zipkin] tacing tools. See our [change log] about tracing performance of your authorization.
* **endpoint:** export url for tracing data.
* **disabled:** switch option for tracing.

[jaeger]: https://www.jaegertracing.io/
[zipkin]: https://zipkin.io/
[signoz]: https://signoz.io/
[change log]: https://www.permify.co/change-log/integration-with-tracing-tools-jaeger-signoz-and-zipkin

:::info
Since Permify is an open 
source solution, it's better to host databases in your servers for **low latency** and **high availability**.
:::

## How to move authorization related data to WriteDB ?

In Permify, authorization data stored as [Relation Tuples] into your preferred database. So these these relational tuples represents your authorization data.

[Relation Tuples]: /docs/relational-tuples

Relational tuples can be created with an simple API call in runtime, since relations and authorization data's are live instances. Each relational tuple should be created according to its authorization model, [Permify Schema]. 

[Permify Schema]: docs/getting-started/modeling

![tuple-creation](https://user-images.githubusercontent.com/34595361/186637488-30838a3b-849a-4859-ae4f-d664137bb6ba.png)

### Creating Relational Tuples

You can create relational tuples with using **"/v1/relationships/write"** endpoint.

**Path:** POST /v1/relationships/write

| Argument | Type |  Description |
|-------------------|---------|-------------|
| entity | string | Type and id of the entity. Example: repository:1”|
| relation | string |  Custom relation name. Eg. admin, manager, viewer etc. |
| subject | string |  User or user set who wants to take the action. |

### Examples 

#### **Organization Admin**

Request

```json
{
    "entity": {
        "type": "organization",
        "id": "1"
    },
    "relation": "admin",
    "subject": {
        "type": "user",
        "id": "1",
        "relation": ""
    }
}
```

**Created relational tuple:** organization:1#admin@1

**Definition:** User 1 has admin role on organization 1.

#### **Organization Members are Viewer of Repo** 

Request

```json
{
    "entity": {
        "type": "repository",
        "id": "1"
    },
    "relation": "viewer",
    "subject": {
        "type": "organization",
        "id": "2",
        "relation": "member"
    }
}
```

**Created relational tuple:** repository:1#admin@organization:2#member

**Definition:** Members of organization 2 are viewers of repository 1.

#### **Parent Organization**

Request

```json
{
    "entity": {
        "type": "repository",
        "id": "1"
    },
    "relation": "parent",
    "subject": {
        "type": "organization",
        "id": "1",
        "relation": "..."
    }
}
```

**Relational Tuple:** repository:1#parent@organization:1#…

**Definition:** Organization 1 is parent of repository 1.

:::info
Note: `relation: “...”` used when subject type is different from **user** entity. **#…** represents a relation that does not affect the semantics of the tuple.
:::
