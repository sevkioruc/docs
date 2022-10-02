# Write Relationships

In Permify, relations between your entities, objects and users stored as [relational tuples]. Since relations and authorization data's are live instances these relational tuples should be created with an simple API call in runtime. 

When using Permify, the application client should update writeDB about the changes happening in entities or resources that are related to the authorization structure. For example, somebody when somebody created a wallet or user X joins a group that has edit access on some documents: the application side needs to write relational tuples to keep [writeDB] up-to-date. Besides, each relational tuple should be created according to its authorization model, Permify Schema.

[relational tuples]: ../relational-tuples.md
[writeDB]: ../getting-started/sync-data.md

So if user:42 has been granted an admin role in organization:1, relational tuple `organization:1#admin@user:42` must be created by using **/v1/relationships/write** endpoint.

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

## Need any help ?

Our team is happy to help you get started with Permify. If you'd like to learn more about using Permify in your app or have any questions about this example, [schedule a call with one of our Permify engineer](https://calendly.com/ege-permify/30min).