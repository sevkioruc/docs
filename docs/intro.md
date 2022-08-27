---
sidebar_position: 1
---

# What is Permify?

[Permify](https://github.com/Permify/permify) is an **open-source authorization service** for creating and maintaining fine-grained authorizations. You can run Permify container image and it works as a Rest API. 

Permify converts & syncs authorization data as relational tuples into a database you point at with a YAML config file. And after completing modeling of your authorization with Permify's DSL - Permify Schema, you can perform access checks with a single API call. Access decisions made according to stored relational tuples.

![relational-tuples](https://user-images.githubusercontent.com/34595361/186108668-4c6cb98c-e777-472b-bf05-d8760add82d2.png)

## Getting Started
Permify consists of 3 core parts; modeling authorization, synchronizing authorization data and access checks. 

- [Modeling Authorization]
- [Centralize & Store Authorization Data]
- [Access Checks]

[Modeling Authorization]: /docs/getting-started/modeling
[Centralize & Store Authorization Data]: /docs/getting-started/sync-data
[Access Checks]: /docs/getting-started/enforcement

Permify centralize your authorization data with high availability, low latency and with ensuring data consistency. These core parts of Permify aims to ease,

- Creating fine grained authorizations with combining multiple access control approaches.
- Maintaining separate access control mechanisms for individual applications.

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
