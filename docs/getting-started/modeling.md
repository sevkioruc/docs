---
sidebar_position: 1
---
# Modeling Authorization 

## Permify Schema

Permify has its own language that you can model your authorization logic with it, we called it Permify Schema. The language allows to define arbitrary relations between users and objects, such as owner, editor, commenter or roles like admin, manager etc. You can define your entities, relations between them and access control decisions with using Permify Schema. 

It includes set-algebraic operators such as inter- section and union for specifying potentially complex access control policies in terms of those user-object relations.

Here’s a simple breakdown of our schema.

![permify-schema](https://user-images.githubusercontent.com/34595361/183866396-9d2850fc-043f-4254-aa4c-ee2c4172afb8.png)

Permify Schema can be created on our [playground](https://play.permify.co/) as well as in any IDE or text editor. We also have a [VS Code extension](https://marketplace.visualstudio.com/items?itemName=Permify.perm) to ease modeling Permify Schema with code snippets and syntax highlights. Note that on VS code the file with extension is ***".perm"***.

## Developing a Schema

Lets create a example auhtorization model using Permify Schema. We'll follow a simplified version of github access control system. To see completed model you can jump directly to [Github Example](#github-example). 

:::info
You can start developing Permify Schema on [VSCode]. You can install the extension by searching for **Perm** in the extensions marketplace.

[VSCode]: https://marketplace.visualstudio.com/items?itemName=Permify.perm
:::

### Entities

The very first step to build Permify Schema is creating your Entities. Entity is an object that defines your resources that held role in your permission system.

Think of entities as tables in your relationship database. We are strongly advice to name entities same as your database table name that its corresponds. In that way you can easily model and rason your authorization as well as eliminating the error posibility.

You can create entities using **entity** keyword. Since we're following example of simplified github access control, lets create some of our entities as follows.

```perm
entity user {}

entity organization {}

entity repository {} 
```

Entities has 2 different attributes. These are;

- **relations**
- **actions**

### Relations

Relations represent relationships between entities. It's probably the most critical part of the schema because Permify mostly based on relations between resources and their permissions. Keyword ***relation*** need to used to create a entity relation with name and type attributes.

**Relation Attributes:**

- **name:** relation name.
- **type:** relation type, basically the entity it’s related to (e.g. user, organization, document, etc.)

An example relation takes form of,

```
relation [name] @[type] 
```

→ Organizations and users can have multiple repositories, so each repository is related with an organization and user. We can define repository relations as below.

```perm
entity repository {

    relation    owner @user         
    relation    org   @organization   

}
```
**Defining Multiple Relation Types**

You can define multiple types to a relation with just adding types one after another. 

```perm
    relation admin @user @repository#owner
```

When we look at the admin relation, it indicates that the admin can be an `user` as well as this user can be a repository owner. 

***Quick note here:*** with using # you can reach entities relation. When we look at the `@org#owner` specifies that if the user has a relation with the repository, this relation can only be the `owner`. We called that feature locking, because it basically locks the relation type.

Defining multiple relation types totally optional. The goal behind it to improve validation and reasonability. And for complex models, it allows you to model your entitlement in a more structured way. You can find more about usage of multiple relation type and locking feature from ...

### Actions

Actions describe what relations, or relation’s relation can do. Think of actions as permissions of the entity it belongs. So actions defines who can perform a specific action on a resource in which circumstances. So, the basic form of authorization check in Permify is ***Can the user U perform action X on a resource Y ?***.

Permify Schema supports ``and``, ``or``, ``and not`` and ``or not`` operators to define actions. Keyword ***action*** need to used with these operators to form an action.

Lets get back to our github example and create some actions on repository entity,

```perm
entity repository {

    relation  owner @user         
    relation  org   @organization      
    
    ..
    ..

    action push = owner

}
```

→ ``action push = owner`` indicates only the repository owner can push to
repository.

```
entity repository {

    relation  owner @user         
    relation  org   @organization 

    ..
    ..

    action read = (owner or org.member) and org.admin

}
```

→ For more fine grained permission let's examine the ``read`` action rules; user that is ``organization admin`` and either ``owner`` of the repository, or ``organization member`` of the organization which repository belongs to can read.

## Github Example 

Here is full implementation of simple Github access control example with using Permify Schema.

```perm
entity user {} 

entity organization {

    relation admin @user @repository#owner
    relation member @user   

    action create_repository = admin or member
    action delete = admin

} 

entity repository {

    relation org    @organization    
    relation owner  @user  

    action push   = owner 
    action read   = (owner or org.member) and org.admin
    action delete = org.admin or owner

} 
```

See more schema examples from the [Example Use Cases](/docs/example-use-cases/simple-rbac) section with their detailed examination.
