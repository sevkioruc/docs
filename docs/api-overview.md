---
sidebar_position: 4
---

# API Overview

In Permify, authorization structured around 3 core parts; *modeling authorization*, *managing authorization data* and *enforcement*.Therefore, Permify API has sections that represent the functionalities of these core parts.

- **Permission Section**: Consist enforcement requests and options.
- **Relationship Section**: Authorization data operations such as creating, deleting and reading relational tuples.
- **Schema Section**: Modeling and Permify Schema related functionalities including configuration and auditing.
- **Server Section**: Basic server informations.

Permify primarily exposes its APIs via [REST](https://restfulapi.net/), see [API Documentation](https://app.swaggerhub.com/apis-docs/permify/permify-api) for all endpoints and details.

## Core Paths

- [Check Access](#check-access)
- [Create Relational Tuple](#create-relational-tuple)
- [Configure Permify Schema](#configure-permify-schema)

### Check Access 

Returns a decision about whether user can perform an action on a certain resource. For example, can the user do push on
a repository object?

**Path:** POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity | object | - | name and id of the entity. Example: repository:1”.
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | subject | object | - | the user or user set who wants to take the action  |
| [ ]   | schema_version | string | 8 | Version of the schema |
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

Permify allows to create relational tuples to your preferred database, [writeDB](/docs/getting-started/sync-data.md).

**Path:** POST /v1/relationships/write

| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | entity | object | - | Type and id of the entity. Example: "organization:1”|
| [x]   | relation | string | - | Custom relation name. Eg. admin, manager, viewer etc.|
| [x]   | subject | string | - | User or user set who wants to take the action. |
| [ ]   | schema_version | string | 8 | Version of the schema |

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


### Configure Permify Schema 

After modeling your authorization using Permify Schema, you need to send Permify Schema file for defining your authorization on Permify API. 

**Path:** POST /v1/schemas/write

| Required | Argument | Type | Default | Description |
|----------|-------------------|--------|---------|-------------|
| [x]   | schema | file | - | Permify Schema file|
