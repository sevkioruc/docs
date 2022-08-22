---
sidebar_position: 2
---

# Why Permify

## Permify works best:

- If you already have an identity/auth solution and want to plug in fine-grained authorization on top of that.
- If you want to create a unified access control mechanism for individual applications.
- If you’re managing authorization for growing micro-service infrastructure.
- If your authorization logic is cluttering your code base.
- If your data model is getting too complicated to handle your authorization within the service.
- If your authorization is growing too complex to handle within code or API gateway.

## Features

🔐 Convert & store authorization data **in house** with high availability.

🔮 Easily model and refactor your authorization with **Permify's DSL, Permify Schema**.

📝 **Audit & Reason** your access control hassle-free with user interface.

✅ Low latency with **parallel graph engine** on access checks.

🩺 Analyze **performance and behavior** of your authorization with tracing tools [jaeger], [signoz] or [zipkin].

[jaeger]: https://www.jaegertracing.io/
[signoz]: https://signoz.io/
[zipkin]: https://zipkin.io/

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