# Data Filtering

In Permify, you can perform two different types access checks,

- **resource based** authorization checks, in form of `Can user U perform action Y in resource Z ?`
- **subject based (data filtering)** authorization checks , in form of `Which records can user U edit ?`

We cover the API usage for resource-based authorization in [check access control]. In this section, we'll examine Permify's lookup query API to examine access checks over collections of data. An example will be a scenario of CRUD - where a person can do a GET call on a collection that may have thousands of records and this person should only see the records they have "view" access for.

The standard way of doing that is checking resource-based authorization for each of the records; whether user U has view permission. This approach will kill performance and scalability if the data volume isn't small. So the best solution would be asking a question similar to  `Which records can user U view ?` and getting records accordingly.

So we need to filter these data on the Permify side and return authorized records, yet we have another problem right here: What if we have 10,000 authorized records? In that case, we need to paginate or some kind of sort these records in order to perform access checks for a smaller number of records. However, Permify can not know which data to return or paginate.

To overcome this problem, we developed an API endpoint that you can ask `Which records can user U view ?` and get a SQL query without any conditions (filter, pagination or sorting, etc) attached to it. So you can add conditions depending on your needs after getting the query response. If you have a list with pagination, after getting the core SQL query from our API request you can add pagination filters to it to check access control just for a small group of records.

[check access control]: ../../docs/api-overview/check-api.md

**Path:** POST /v1/permissions/lookup-query

| Required | Argument | Type | Default | Description |
|----------|----------|---------|---------|-------------------------------------------------------------------------------------------|
| [x]   | entity_type | object | - | type of records, for example document, post, comment etc.
| [x]   | action | string | - | the action the user wants to perform on the resource |
| [x]   | subject | object | - | the user or user set who wants to take the action. It containes type and id of the subject.  |
| [ ]   | schema_version | string | 8 | Version of the schema |
| [ ]   | depth | integer | 8 | Timeout limit when if recursive database queries got in loop|

#### Request

```json
{
  "entity_type": "document",
  "action": "view",
  "subject": {
    "type": "user",
    "id": "2",
  }
}
```

#### Response

```sql
select * from documents INNER JOIN organizations
ON documents.parent_id = organizations.id 
where owner_id = 2 or organizations.id = 1

```

Generating these SQL is accomplished within looking up Permify Schema and the stored relation tuples. See how this [SQL generated] in more detail. 

[SQL generated]: ../../docs/getting-started/enforcement#subject-specific-data-filtering-check-request

## Need any help ?

Our team is happy to help you get started with Permify. If you'd like to learn more about using Permify in your app or have any questions about this example, [schedule a call with one of our Permify engineer](https://meetings-eu1.hubspot.com/ege-aytin/call-with-an-expert).