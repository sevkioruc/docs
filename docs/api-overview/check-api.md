# Check Access Control

In Permify, you can check authorization with single [check] request. This method returns a decision about whether user can perform an action on a certain resource. For example, ***Can the user 1 push to respository 1 ?***

[check]:  https://app.swaggerhub.com/apis-docs/permify/permify-api/v0.0.0-alpha4#/Permission/permissions.check

**Path:** POST /v1/permissions/check

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity | object | - | contains entity type and id of the entity. Example: repository:1”.
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | subject | object | - | the user or user set who wants to take the action. It containes type and id of the subject.  |
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

Answering access checks is accomplished within Permify using a basic graph walking mechanism. See how [access decisions evaluated] in Permify. 

[access decisions evaluated]: ../../docs/getting-started/enforcement#how-access-decisions-are-evaluated

## Need any help ?

Our team is happy to help you get started with Permify. If you'd like to learn more about using Permify in your app or have any questions about this example, [schedule a call with one of our Permify engineer](https://calendly.com/ege-permify/30min).