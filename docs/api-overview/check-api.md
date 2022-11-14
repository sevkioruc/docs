# Check Access Control

In Permify, you can perform two different types access checks,

- **resource based** authorization checks, in form of `Can user U perform action Y in resource Z ?`
- **subject based (data filtering)** authorization checks , in form of `Which records can user U edit ?`

In this section we'll investigate proior check request of Permify: **resource based** authorization checks. You can find subject based access checks in [data filtering] section.

[data filtering]: ./data-filtering

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

Our team is happy to help you get started with Permify. If you'd like to learn more about using Permify in your app or have any questions about this example, [schedule a call with one of our Permify engineer](https://meetings-eu1.hubspot.com/ege-aytin/call-with-an-expert).