---
sidebar_position: 7
---

# API Overview

You can run Permify with docker and it works as a Rest API. This is the overview section for Permify API. 

## Paths

Some core paths:

- [Check Access](#check-access)
- [Create Relational Tuple](#create-relational-tuple)
- [Configure Permify Schema](#configure-permify-schema)

### Check Access 

Returns a decision about whether user can perform an action on a certain resource. For example, can the user do push on
a repository object?

**Path:** POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | user | string | - | the user or user set who wants to take the action. Examples: “1”, “organization:1#owners”
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | object | string | - | name and id of the resource. Example: “organization:1” |
| [ ]   | depth | integer | 8 | |

#### Request

```json
{
  "user": "1",
  "action": "push",
  "object": "repository:1"
}
```

#### Response

```json
{
  "can": true,
  "debug": "user 1 is a owner of organization 1"
}
```

### Create Relational Tuple

Permify allows to create relational tuples to your writeDB.

**Path:** POST /v1/relationships/write

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

### Configure Permify Schema 

After modeling your authorization using Permify Schema, you need to send Permify Schema file for defining your authorization on Permify API.

**Path:** POST /v1/schemas/replace

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

### Request

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

### Response

```json
{
  "message": "success"
}
```

### Status Ping

Delete relation tuple.
**Path:** GET /v1/status/ping

### Status Version

Delete relation tuple.
**Path:** GET /v1/status/version


You can find more on [API docs](https://github.com/Permify/permify/tree/master/docs)


