---
sidebar_position: 1
---

# What is Permify?

[Permify](https://github.com/Permify/permify) is an **open-source authorization service** for creating and maintaining fine-grained authorizations accross your individual applications and services.

Permify converts authorization data as relational tuples into a database you point at. We called that database Write Database (WriteDB) and it behaves as a centralized data source for your authorization system. You can model your authorization with Permify's DSL - Permify Schema and perform access checks with a single API call anywhere on your stack. Access decisions made according to stored relational tuples.

![relational-tuples](https://user-images.githubusercontent.com/34595361/186108668-4c6cb98c-e777-472b-bf05-d8760add82d2.png)

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

## Getting Started
Permify consists of 3 core parts; modeling authorization, centralizing authorization data and access checks. 

- [Modeling Authorization]
- [Centralize Authorization Data]
- [Access Checks]

[Modeling Authorization]: /docs/getting-started/modeling
[Centralize Authorization Data]: /docs/getting-started/sync-data
[Access Checks]: /docs/getting-started/enforcement

Permify centralize your authorization data with high availability, low latency and with ensuring data consistency. These core parts of Permify aims to ease,

- Creating fine grained authorizations with combining multiple access control approaches.
- Maintaining separate access control mechanisms for individual applications and services.

Permify's data model is inspired by Google’s consistent, global authorization system, [Google Zanzibar Paper](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41f08f03da59f5518802898f68730e247e23c331.pdf).

## Community

Join the conversation at our [Discord channel](https://discord.gg/MJbUjwskdH). We love to talk about authorization and access control - we would love to hear from you :heart:

If you like Permify, please consider giving us a :star:️ on [github](https://github.com/Permify/permify)

<h2 align="left">Let's get connected</h2>

<p align="left">
<a href="https://discord.gg/MJbUjwskdH">
 <img alt="guilyx’s Discord" width="50px" src="https://user-images.githubusercontent.com/34595361/178992169-fba31a7a-fa80-42ba-9d7f-46c9c0b5a9f8.png" />
</a>
<a href="https://twitter.com/GetPermify">
  <img alt="guilyx | Twitter" width="50px" src="https://user-images.githubusercontent.com/43545812/144034996-602b144a-16e1-41cc-99e7-c6040b20dcaf.png"/>
</a>
<a href="https://www.linkedin.com/company/permifyco">
  <img alt="guilyx's LinkdeIN" width="50px" src="https://user-images.githubusercontent.com/43545812/144035037-0f415fc7-9f96-4517-a370-ccc6e78a714b.png" />
</a>
</p>
