---
sidebar_position: 3
---

# Access Control Check

In Permify, you can check authorization with single [check] request. Basic authorization checks take the form of

***Can the subject U perform action X on a resource Y ?***

Permify designed to answer these questions efficiently and with minimal complexity while providing low latency with it's parallel graph engine. 

[check]:  https://app.swaggerhub.com/apis-docs/permify/permify-api/v0.0.0-alpha3#/Permission/permissions.check

## Check Request

Let's follow an simplified github access control decision for examining the check request.

***Can the user X push on a repository Y ?***

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
    "type": "repository", 
    "id": "12"
  },
  "action": "push",
  "subject": {
    "type":"user",
    "id": "1"
  }
}
```

#### Response

```json
{
  "can": false, // check decision
  "decisions": { // decision logs
    "organization:1#member": {
      "prefix": "not",
      "can": false,
      "err": null
    },
    "repository:1#owner": {
      "prefix": "",
      "can": true,
      "err": null
    }
  }
}
```

## How Access Decisions Are Evaluated?

Access decisions are evaluated by stored [relational tuples] and your authorization model, [Permify Schema]. 

In high level, access of an subject related with the relationships created between the subject and the resource. You can define this relationships in Permify Schema then create and store them as relational tuples, which is basically your authorization data. 

So the workflow of check request is, 
1. Lookup to schema for finding the given action's relations.
2. Walk over a graph of each relation (paralel check) to find whether given subject - user - is related with the action. 

Let's continue our github example with a schema to understand this workflow,

[relational tuples]: /docs/relational-tuples
[Permify Schema]:  /docs/getting-started/modeling

```perm
entity user {} 

entity organization { 

    relation owner @user  
    relation member @user   

} 

entity repository {

    relation owner @user
    relation org @organization

    action push = owner or org.owner or org.member
   
} 
```

Answering access checks is accomplished within Permify using a basic graph walking mechanism. 

Below is our push actions graph of relations that defined by above schema and relationships. So this is the graph that Permify walkthough and check whether the user can push.

[relation tuples]: /docs/relational-tuples

![graph-of-relations](https://user-images.githubusercontent.com/34595361/181000466-d2f28fc7-3c41-49b3-8731-3c4b34643075.png)

The graph shows a set of paths from the resource (somerepository), through the push action, to the relations defining our rules (owner) and finally to the subjects (user, organization).

We can see that owner, organization owner or organization member can push to a repo. As an example if our check requests asks ***Can Asher push on a somerepository ?*** 

Further walking outward to the relation owner, we can find **Asher**. Since being owner is enough for pushing the answer to our question is “yes”.



