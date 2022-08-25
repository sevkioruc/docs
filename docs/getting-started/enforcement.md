---
sidebar_position: 3
---

# Access Control Check

You can check authorization with
single API call. This check request returns a decision about whether user can perform an action on a certain resource.

Access decisions generated according to relational tuples, which stored in your database (writeDB) and [Permify Schema] action conditions.

[Permify Schema]:  /docs/getting-started/modeling

## Graph Of Relations

The [relation tuples] of the ACL used by Permify can be represented as a graph of relations. This graph will help you
understand the performance of check engine and the algorithms it uses.

[relation tuples](/docs/relational-tuples.md)

![graph-of-relations](https://user-images.githubusercontent.com/34595361/181000466-d2f28fc7-3c41-49b3-8731-3c4b34643075.png)

With these relational tuples store in WriteDB, an example authorization checks take the form of “does user U have relation R to object O?” and are evaluated by a those relational tuples and Permify Schema.

## Example Access Check

Lets examine a example access control decision on github: 

***Can the user X push on a repository Y ?***

### Path: 

POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity | string | - | name and id of the entity. Example: repository:1”.
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | subject | string | - | the user or user set who wants to take the action  |
| [ ]   | depth | integer | 8 | Timeout limit when if recursive database queries got in loop|

#### Request

```json
{
  "entity": {
    "type": "repository",
    "id": "1"
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
  "can": false, // main decision
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


