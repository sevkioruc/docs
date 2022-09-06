---
sidebar_position: 1
---
# Modeling Authorization 

## Permify Schema

Permify has its own language that you can model your authorization logic with it, we called it Permify Schema. You can 
define your entities, relations between them and access control decisions with using Permify Schema. 

Here’s a simple breakdown of our schema.

![permify-schema](https://user-images.githubusercontent.com/34595361/183866396-9d2850fc-043f-4254-aa4c-ee2c4172afb8.png)

This schema represents your authorization model. You can create Permify Schema file with **.perm** file extension. And you can configure it on Permify API via sending schema as a file. This configuration covered more detailed in [Installation Guide] section. 

[Installation Guide]:  /docs/Installation#configuring-schema-on-permify

For now, lets model your authorization using Permify Schema. We follow a simple example of github access control system. To see completed model you can jump directly to [Github Example](#github-example). 

### Entities

The very first step to build Permify Schema is creating your Entities.

Entities represent your main tables. The table name and the entity name here must be the same. 
You can create entitis using **entity** keyword.

```perm
entity user {}

entity organization {}

entity repository {} 
```

→ For our github case, we create user, organization and repository entities like above. In that case, name of the user entity represents user table in your database.

Entities has 2 different attributes. These are;

- **relations**
- **actions**

### Relations

Relations represent relationships between entities. Attribute ***relation*** need to used in with several attributes to create a entity relation.

**Relation Attributes:**

- **name:** custom relation name.
- **entity:** the entity it’s related with (e.g. user, organization, repo…)
- **table (optional):** the name of the pivot table. (Only for many-to-many relationships.)
- **rel:(optional):** type of relationship (many-to-many, belongs-to or custom)
- **cols:(optional):** the columns you have created in your database.

An example relation takes form of,

```
relation [name] @[entity] 
```

→ For better understanding, let's go back to our github example. Organizations and users can have multiple repositories,
so each repository is related with an organization and user. We can define repository relations as below.

```perm
entity repository {

    relation    owner @user         
    relation    org   @organization   

}
```

### Actions

Actions describe what relations, or relation’s relation can do, think of actions as entity permissions. Actions
defines who can perform a specific action in which circumstances.

Permify Schema supports ``and``, ``or``, ``and not`` and ``or`` not operators to define actions. Keyword ***action*** need to 
used with these operators to form an action.

Lets get back to our github example and create some actions,

```perm
entity repository {

    relation    owner @user         
    relation    org   @organization      
    
    ..
    ..

    action push   = owner

}
```

→ For example, only the repository owner can push to
repository.

```
entity repository {

    relation    owner @user         
    relation    org   @organization 

    ..
    ..

    action read   = (owner or org.member) and org.admin

}
```

→ For more fine grained permission, "user with a admin role and 'either owner of the repository, or member of the organization which repository belongs to'"
can read.

## Github Example 

Here is full implemetation of Github example with using Permify Schema.

```perm
entity user {} 

entity organization {

    relation admin @user     
    relation member @user   

    action create_repository = admin or member
    action delete = admin

} 

entity repository {

    relation    owner @user        
    relation    org   @organization    

    action push   = owner 
    action read   = (owner or org.member) and org.admin
    action delete = org.admin or owner

} 
```
