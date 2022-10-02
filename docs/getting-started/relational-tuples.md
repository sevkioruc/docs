---
sidebar_position: 2
---

# Access Control as Relations

In Permify, relationship between your entities, objects, and users builds up a collection of access control lists (ACLs), which are then used in to access decision points. 

As an example; to decide whether a user could view a protected resource, Permify looks up the relations between that specific user and the protected resource. These relation types could be ownership, parent-child relation, or even a role such as an administrator or manager.

## What is Relational Tuples ?

These ACLs called relational tuples: the underlying data form that represents object-to-object and object-to-subject relations. Each relational tuple represents an action that a specific user or user set can do on a resource and takes form of “user U has relation R to object O”, where user U could be a simple user or a user set such as team X members.

In Permify, the simplest form of relational tuple structured as: `entity # relation @ user`. Here are some relational tuples with semantics,

![relational-tuples](https://user-images.githubusercontent.com/34595361/183959294-149fcbb9-7f10-4c1e-8d66-20a839893909.png)

## Where Relational Tuples Used ?

In Permify, these relational tuples represents your authorization data. 

Permify stores your relational tuples (authorization data) in a database you prefer. We called that database as Write Database, shortly [WriteDB]. You can configure it with using our [YAML config file]. Stored relational tuples are queried on access control check requests to decide whether user action is authorized. 

![tuple-creation](https://user-images.githubusercontent.com/34595361/186637488-30838a3b-849a-4859-ae4f-d664137bb6ba.png)

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

 According to the schema above; when a user creates a document in an organization, more specifically let's say, when user:1 in organization:2 create a document:4 we need to create the following relational tuple,

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

### More Examples 

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

## Write Database 

Think WriteDB as source of truth for your authorization system. We took that approach because a unified authorization system offers important advantages over maintaining separate access control mechanisms for individual applications.

But how authorization data stored in WriteDB ? Let's take a look at a snap shot of demo table on sample Write Database.

![demo-table](https://user-images.githubusercontent.com/34595361/180988784-a9424088-2d4f-4cee-8db4-96adde40d27d.png)

Each row represents object-user or object-object relations, which we call relational tuples. Each row (tuple) behave as ACL and takes the form of “user U has relation R to object O”

→ Considering table above, semantics of second row (id:8) is **user 1 is owner of repository 1**

Alternatively user U can behave as "set of users".
More spesifically, “set of users S has relation R to object O”, where S is itself specified in terms of another object-relation pair. 

 → First row in our table (id:7), we can see that **organization 1 (set of users in organization) is parent of repository 1**

:::info
These relational tuples data form is inspired by Google’s Consistent, Global Authorization System, [Google Zanzibar White Paper](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41f08f03da59f5518802898f68730e247e23c331.pdf)
:::