---
sidebar_position: 5
---

# Storing Relational Tuples

## What is Relational Tuples

Permify stores your authorization data in a database you prefer. We called that database as WriteDB, and you can define it with using our YAML config file.

Relation tuples are the underlying data form that represents object-to-object and object-to-subject relations. 

The simplest form of relational tuple structured as:

***entity # relation @ user***

with an example data : ***repository:1#owner@user:asher***

corresponding semantics: ***User 'Asher' is an owner of repository:1***

Here’s a simple breakdown of two relational tuples.

![relational-tuples](https://user-images.githubusercontent.com/34595361/183959294-149fcbb9-7f10-4c1e-8d66-20a839893909.png)

## Write Database 

Think WriteDB as source of truth for your authorization system. We took that approach because a unified authorization system offers important advantages over maintaining separate access control mechanisms for individual applications.

But how authorization data stored in WriteDB ? Let's take a look at a snap shot of demo table on sample Write Database.

![demo-table](https://user-images.githubusercontent.com/34595361/180988784-a9424088-2d4f-4cee-8db4-96adde40d27d.png)

Each row represents object-user or object-object relations, which we call relational tuples. Each row (tuple) behave as ACL and takes the form of “user U has relation R to object O”

→ Considering table above, semantics of second row (id:8) is **user 1 is owner of repository 1**

Alternatively user U can behave as "set of users".
More spesifically, “set of users S has relation R to object O”, where S is itself specified in terms of another object-relation pair. 

 → First row in our table (id:7), we can see that **organization 1 (set of users in organization) is parent of repository 1**


## Creating Relational Tuples 

Permify has its own language that you can model your authorization logic with it, we call it Permify Schema. You can define your entities, relations between them and access control decisions of each actions with using Permify Schema. 

For more details about Permify Schema check out [Modeling Authorization with Permify].

[Modeling Authorization with Permify]: /docs/getting-started/modeling

If we look back to our example above, repository entity and its relations look similar to this on Permify Schema:

```perm
entity repository {

    relation    owner @user       
    relation    org   @organization   

    action push   = owner
    action read   = (owner or org.member) and org.admin
    action delete = org.admin or owner

} 
```

According to these rules and relations we convert your application data into authorization data as tuples and stored respectively like database table in above.

## Graph Of Relations

The relation tuples of the ACL used by Permify can be represented as a graph of relations. This graph will help you
understand the performance of check engine and the algorithms it uses.

![graph-of-relations](https://user-images.githubusercontent.com/34595361/181000466-d2f28fc7-3c41-49b3-8731-3c4b34643075.png)

With these relational tuples store in WriteDB, an example authorization checks take the form of “does user U have relation R to object O?” and are evaluated by a those relational tuples and Permify Schema.

:::info
Permify's data model is inspired by Google’s Consistent, Global Authorization System, [Google Zanzibar White Paper](https://storage.googleapis.com/pub-tools-public-publication-data/pdf/41f08f03da59f5518802898f68730e247e23c331.pdf)
:::

