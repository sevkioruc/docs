---
sidebar_position: 2
---

# Why Permify?

You can use Permify any stage of your development for your authorization needs but Permify works best:

- If you need to refactor your authorization.
- If you’re managing authorization for growing micro-service infrastructure.
- If your authorization logic is cluttering your code base.
- If your data model is getting too complicated to handle your authorization within the service.
- If your authorization is growing too complex to handle within code or API gateway.

## Features

- Sync & coordinate your authorization data hassle-free.
- Get Boolean - Yes/No decision returns.
- Store your authorization data in-house with high availability & low latency.
- Easily model, debug & refactor your authorization logic.
- Enforce authorizations with a single request anywhere you call it.
- Low latency with parallel graph engine for enforcement check.

## Example

Permify helps you move & sync authorization data from your ListenDB to WriteDB with a single config file based on your
authorization model that you provide us in a YAML schema.
After configuration, you can check authorization with a simple call.
**Request**

```json
{
  "user": "1",
  "action": "push",
  "object": "repository:1"
}
```

**Response**

```json
{
  "can": false, // main decision
  "decisions": { // decision logs
    "repository:1#parent.admin": {
      "can": false,
      "err": null
    },
    "repository:1#parent.member": {
      "can": false,
      "err": null
    }
  }
}
```