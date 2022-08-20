---
sidebar_position: 5
---

# Relational Tuples

## What is Relational Tuples ?

Relation tuples are the underlying data form that represents object-to-object and object-to-subject relations.Each relational tuple represents an action that a specific user or user set can do on a resource.

The simplest form of relational tuple structured as: `entity # relation @ user`. Here are some relational tuples with semantics,

![relational-tuples](https://user-images.githubusercontent.com/34595361/183959294-149fcbb9-7f10-4c1e-8d66-20a839893909.png)

### How these Relational Tuples Used ?

In Permify, these relational tuples represents your authorization data. 

Permify stores your relational tuples (authorization data) in a database you prefer. We called that database as Write Database, shortly [WriteDB]. You can configure it with using our [YAML config file]. Stored relational tuples are queried on access control check requests to decide whether user action is authorized. 

[YAML config file]: /docs/getting-started/sync-data
[WriteDB]: #write-database

## Creating Relational Tuples 

Relational tuples can be created with an simple API call in runtime, since relations and authorization data's are live instances. Each relational tuple should be created according to its authorization model, [Permify Schema]. 

[Permify Schema]: docs/getting-started/modeling

For example, lets say you have a document management system with the following Permify Schema.

```perm
entity user {} 

entity organization {

    relation member @user

} 

entity document {
    
    relation  owner  @user   
    relation  org    @organization      

    action view   = owner or org.member
    action edit   = owner 
    action delete = owner
} 
```

 Acorrding to schema above; when user creates a document in a organization, more specifically let say, when user:1 in organization:2 create a document:4 we need to create 2 relational tuples to be stored in [WriteDB]. Respectively,

- `document:4#owner@user:1`

[WriteDB]: #write-database

## API endpoint 

You can create relational tuples by using `/v1/relationships/write` endpoint. 

Send a request to POST - `/v1/relationships/write`

**Request**

```json
{
    "entity": {
        "type": "document",
        "id": "4" 
    },
    "relation": "owner",
    "subject": {
        "type": "user",
        "id": "1", 
        "relation": ""
    }
}
```

## Write Database 

Think WriteDB as source of truth for your authorization system. We took that approach because a unified authorization system offers important advantages over maintaining separate access control mechanisms for individual applications.

But how authorization data stored in WriteDB ? Let's take a look at a snap shot of demo table on sample Write Database.

![demo-table](https://user-images.githubusercontent.com/34595361/180988784-a9424088-2d4f-4cee-8db4-96adde40d27d.png)

Each row represents object-user or object-object relations, which we call relational tuples. Each row (tuple) behave as ACL and takes the form of “user U has relation R to object O”

→ Considering table above, semantics of second row (id:8) is **user 1 is owner of repository 1**

Alternatively user U can behave as "set of users".
More spesifically, “set of users S has relation R to object O”, where S is itself specified in terms of another object-relation pair. 

 → First row in our table (id:7), we can see that **organization 1 (set of users in organization) is parent of repository 1**

## Graph Of Relations

The relation tuples of the ACL used by Permify can be represented as a graph of relations. This graph will help you
understand the performance of check engine and the algorithms it uses.

![graph-of-relations](https://user-images.githubusercontent.com/34595361/181000466-d2f28fc7-3c41-49b3-8731-3c4b34643075.png)

With these relational tuples store in WriteDB, an example authorization checks take the form of “does user U have relation R to object O?” and are evaluated by a those relational tuples and Permify Schema.

:::info
Permify's data model is inspired by Google’s Consistent, Global Authorization System, [Google Zanzibar White Paper](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41f08f03da59f5518802898f68730e247e23c331.pdf)
:::

