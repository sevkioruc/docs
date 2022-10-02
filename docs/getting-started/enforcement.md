---
sidebar_position: 4
---

# Access Control Check

In Permify, you can check authorization with single [check] request. Basic authorization checks take the form of ***Can the subject U perform action X on a resource Y ?***

Permify designed to answer authorization questions efficiently and with minimal complexity while providing low latency with:
- Using its parallel graph engine. 
- Storing the relationships between resources and subjects beforehand in Permify data store: [writeDB], rather than providing these relationships at “check” time.
- Using in memory cache to store authorization schema.

[check]:  https://app.swaggerhub.com/apis-docs/permify/permify-api/v0.0.0-alpha4#/Permission/permissions.check
[writeDB]: ../getting-started/sync-data.md

## Check Request

Let's follow an simplified access control decision for examining the check request.

***Can the user 3 edit document 12 ?***

#### Path: 

POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity | object | - | id and type of the entity. Example: repository:1”.
| [x]   | action | string | - | the action the subject wants to perform on the resource |
| [x]   | subject | object | - | The user or user set that wants to take the action  |
| [ ]   | depth | integer | 20 | Timeout limit when if recursive database queries got in loop|
| [ ]   | schema_version | string | - | Version of the schema|

#### Request

```json
{
  "entity": {
    "type": "document", 
    "id": "12"
  },
  "action": "edit",
  "subject": {
    "type":"user",
    "id": "3"
  }
}
```

#### Response

```json
{
  "can": false, // check decision
  "decisions": { // decision logs
    "organization:1#admin": {
      "prefix": "not",
      "can": false,
      "err": null
    },
    "document:1#owner": {
      "prefix": "",
      "can": false,
      "err": null
    }
  }
}
```

## How Access Decisions Are Evaluated?

Access decisions are evaluated by stored [relational tuples] and your authorization model, [Permify Schema]. 

In high level, access of an subject related with the relationships created between the subject and the resource. You can define this relationships in Permify Schema then create and store them as relational tuples, which is basically your authorization data. 

Permify Engine to compute access decision in 2 steps, 
1. Looking up authorization model for finding the given action's ( **edit**, **push**, **delete** etc.) relations.
2. Walk over a graph of each relation to find whether given subject ( user or user set ) is related with the action. 

Let's turn back to above authorization question ( ***"Can the user 3 edit document 12 ?"*** ) to better understand how decision evaluation works. 

[relational tuples]: /docs/relational-tuples
[Permify Schema]:  /docs/getting-started/modeling

When Permify Engine recieves this question it ireclty looks up to authorization model to find document `‍edit` action. Let's say we have a model as follows

```perm
entity user {}
        
entity organization {

    // organizational roles
    relation admin @user
    relation member @user
}

entity document {

    // represents repositories parent organization
    relation parent @organization
    
    // represents owner of this repository
    relation owner  @user
    
    // permissions
    action edit   = parent.admin or owner
    action delete = owner
} 
```

Which has a directed graph as follows:

![relational-tuples](https://user-images.githubusercontent.com/34595361/193418063-af33fe81-95ed-4615-9d86-b50d4094ad8e.png)

As we can see above: only users with an admin role in an organization, which `document:12` belongs, and owners of the `document:12` can edit. Permify runs two concurent queries for **parent.admin** and **owner**:

**Q1:** Get the owners of the `document:12`.

**Q2:** Get admins of the organization where `document:12` belongs to.

Since edit action consist **or** between owner and parent.admin, if Permify Engine found user:3 in results of one of these queries then it terminates the other ongoing queries and returns authorized true to the client.

Rather than **or**, if we had an **and** relation then Permify Engine waits the results of these queries to returning a decision. 



