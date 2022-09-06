---
sidebar_position: 6
---

# API Overview

Permify works as a Rest API. This is the overview section for Permify API. 

## Paths

Some core functionalities:

- [Check Access](#check-access)
- [Create Relational Tuple](#create-relational-tuple)
- [Configure Permify Schema](#configure-permify-schema)
- [Read & Filter Relational Tuples](#read-relational-tuples)

### Check Access 

Returns a decision about whether user can perform an action on a certain resource. For example, can the user do push on
a repository object?

**Path:** POST /v1/permissions/check

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

### Create Relational Tuple

Permify allows to create relational tuples to your writeDB.

**Path:** POST /v1/relationships/write

| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | entity | object | - | Type and id of the entity. Example: "organization:1”|
| [x]   | relation | string | - | Custom relation name. Eg. admin, manager, viewer etc.|
| [x]   | subject | string | - | User or user set who wants to take the action. |

#### Request

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

#### Response

```json
{
    "data": {
        "entity": {
            "type": "organization",
            "id": "1"
        },
        "relation": "admin",
        "subject": {
            "type": "user",
            "id": "1"
        }
    }
}
```

### Read Relational Tuples

Display and filter relational tuples that stored in WriteDB. 

**Path:** POST /v1/relationship/read

| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | entity | string | - | entity type |
| [x]   | id | string | - | | entity type

#### Request

```json
{
  "filter": {
      "entity":  "organization",
      "id":  "1",
  }
}
```

#### Response

```json
[
        {
            "entity": {
                "type": "organization",
                "id": "1"
            },
            "relation": "member",
            "subject": {
                "type": "user",
                "id": "1"
            }
        },
        {
            "entity": {
                "type": "organization",
                "id": "1"
            },
            "relation": "admin",
            "subject": {
                "type": "user",
                "id": "1"
            }
        }
    ]
```



### Configure Permify Schema 

After modeling your authorization using Permify Schema, you need to send Permify Schema file for defining your authorization on Permify API. 

**Path:** POST /v1/schemas/write

| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | schema | file | - | Permify Schema file|

### Delete Tuple

Delete relation tuple.
**Path:** POST /v1/relationships/delete

| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | entity | string | - | |
| [x]   | object_id | string | - | |
| [x]   | relation | string | - | |
| [ ]   | userset_entity | string | - | |
| [x]   | userset_object_id | string | - | |
| [ ]   | userset_relation | string | - | |

#### Request

```json
{
  "entity": "organization",
  "object_id": "1",
  "relation": "admin",
  "userset_entity": "",
  "userset_object_id": "1",
  "userset_relation": ""
}
```

#### Response

```json
{
  "message": "success"
}
```

### Status Ping

**Path:** GET /v1/status/ping

### Status Version

**Path:** GET /v1/status/version


You can find more on [API docs](https://github.com/Permify/permify/tree/master/docs)


