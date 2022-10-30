---
sidebar_position: 1
---

# What is Permify?

[Permify](https://github.com/Permify/permify) is an **open-source authorization service** for creating and maintaining fine-grained authorizations in your applications.

With Permify you can easily structure your authorization model, store authorization data in your own servers securely, and interact with Permify API to handle all authorization questions from any of your applications.

Permify's data model is inspired by Google’s consistent, global authorization system, [Google Zanzibar Paper](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41f08f03da59f5518802898f68730e247e23c331.pdf).

## Core Features of Permify

🔐 Store your permissions **in house** with high availability.

🔮 Easily model and refactor your authorization with **Permify's DSL, Permify Schema**.

📝 **Audit & Reason** your access control hassle-free with various methods.

✅ High performence on authorization questions with **parallel graph engine**.

🩺 Analyze **performance and behavior** of your authorization with tracing tools [jaeger], [signoz] or [zipkin].

[jaeger]: https://www.jaegertracing.io/
[signoz]: https://signoz.io/
[zipkin]: https://zipkin.io/

## Getting Started

In Permify, authorization divided into 3 core aspects; **modeling**, **storing authorization data** and **access checks**.  

- See how to [Model your Authorization] using Permify Schema.
- Learn how Permify [Store Authorization Data] as relations.
- Perform an [Access Checks] anywhere in your stack.

[Model your Authorization]: /docs/getting-started/modeling
[Store Authorization Data]: /docs/getting-started/sync-data
[Access Checks]: /docs/getting-started/enforcement

This document explains how Permify handles these aspects to provide a robust and scalable unified authorization system for your applications. For the ones that want trying out and examine it instantly, 

<div class="getting-started-grid">
    <a href="https://play.permify.co/">
        <div class="btn-thumb">
            <div class="thumbnail">
                <img src="https://uploads-ssl.webflow.com/61bb34defcff34f786b458ce/6332bb38106ffd85102bb3bc_Screen%20Shot%202022-09-27%20at%2011.58.27.png"/>
            </div>
           Use our Playground to test your authorization in a browser.
        </div>
    </a>
    <a href="https://docs.permify.co/docs/Installation">
        <div class="btn-thumb">
            <div class="thumbnail">
                 <img src="https://uploads-ssl.webflow.com/61bb34defcff34f786b458ce/6332bb38106ffd85102bb3bc_Screen%20Shot%202022-09-27%20at%2011.58.27.png"/>
            </div>
            Set Up Permify service in your enviroment 
        </div>
    </a>
    <a href="https://www.permify.co/post/google-zanzibar-in-a-nutshell">
        <div class="btn-thumb">
            <div class="thumbnail">
                <img src="https://uploads-ssl.webflow.com/61bb34defcff34f786b458ce/634520d7859cd419ec89f9ef_Google%20Zanzibar%20in%20a%20Nutshell-1.png"/>
            </div>
            Examine Google Zanzibar In A Nutshell
        </div>
    </a>
</div>


## Community & Support

We love to talk about authorization also we would love to hear from you :heart:

You can get immidiate help on our [Discord](https://discord.gg/MJbUjwskdH) channel. This can be any kind of questions related to Permify, authorization, or even from authentication or identity access control. We'd love to discuss anything related with access control space.

For feature requests, bugs or any improvements you can always open an [issue] on Github. If you like Permify, please consider giving us a :star:️ on [Github](https://github.com/Permify/permify)

[issue]: https://github.com/Permify/permify/issues

<h3 align="left">Let's get connected</h3>

<p align="left">
<a href="https://discord.gg/MJbUjwskdH">
 <img height="70px" width="70px" alt="permify | Discord" src="https://user-images.githubusercontent.com/39353278/187209316-3d01a799-c51b-4eaa-8f52-168047078a14.png" />
</a>
<a href="https://twitter.com/GetPermify">
  <img height="70px" width="70px" alt="permify | Twitter" src="https://user-images.githubusercontent.com/39353278/187209323-23f14261-d406-420d-80eb-1aa707a71043.png"/>
</a>
<a href="https://www.linkedin.com/company/permifyco">
  <img height="70px" width="70px" alt="permify | Linkedin" src="https://user-images.githubusercontent.com/39353278/187209321-03293a24-6f63-4321-b362-b0fc89fdd879.png" />
</a>
</p>

## Need any help on Authorization ?

Our team is happy to help you anything about authorization. Moreover, if you'd like to learn more about using Permify in your app or have any questions, [schedule a call with one of our founders](https://calendly.com/ege-permify/30min).