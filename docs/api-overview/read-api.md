# Read Relational Tuples

Read API allows for directly querying the stored graph data to display and filter stored relational tuples.

**Path:** POST /v1/relationship/read

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity | object | - | contains entity type and id of the entity. Example: repository:1”.
| [x]   | relation | string | - | relation of the given entity |
| [ ]   | subject | object | - | the user or user set. It containes type and id of the subject.  ||


#### Request

```json
{
  "filter": {
    "entity": {
        "type": "organization",
        "id": "1"
    },
    "relation": "member",
    "subject": {
        "type":"",
        "id": ""
    }
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
        "relation": "member",
        "subject": {
            "type": "user",
            "id": "2"
        }
    }
]
```

## Need any help ?

Our team is happy to help you get started with Permify. If you'd like to learn more about using Permify in your app or have any questions about this example, [schedule a call with one of our Permify engineer](https://meetings-eu1.hubspot.com/ege-aytin/call-with-an-expert).