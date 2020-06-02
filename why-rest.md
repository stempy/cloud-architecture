# Why Rest?

16/5/2020

Over the years, server client interaction have evolved from various approaches, such as RPC (Remote Procedure Call), SOAP Web Services, and for the past decade or so (since around 2010) Web API's. We used to send and receive binary packets, XML, and other custom formats for internal and external data exchange, than along came JSON and Web Api's which dramatically simplified the data structure and transport mechanisms, using the HTTP protocol for both public web applications, along with internal software approaches has enabled us to quickly build api driven applications, especially as the front-end development has become mini-applications in themselves. 

WebApi's have been the defacto standard for some time now, however many developers still don't use standardized approaches to creating webapi's....some still apply RPC approaches in an environment suited for REST, take for instance the following endpoints for a simple crud application.

RPC style routes - notice how the action names represent the action, this style was used for many years

```c#
POST /api/somecontroller/create
GET /api/somecontroller/getAll
GET /api/somecontroller/getById/{id}
POST /api/somecontroller/updateById/{id}
POST or GET  /api/somecontroller/deleteById/{id}
```

These methods look fine, however there is an obvious problem with them. 
1. there is no real standard naming for them
2. front end consumers need to be shown the exact endpoints for *every* CRUD action, as there are 5 different endpoints
3. even if they were applied consistently across an application, it's much easier to forget, or have subtly different naming conventions across different api's
4. when it comes to adding new actions on top of the basic crud approaches, the new names have the same issue. Someone has to name them for every action.

Now consider a REST-like API version:

```c#
POST /api/somecontroller/
GET /api/somecontroller/
GET /api/somecontroller/{id}
PUT /api/somecontroller/{id}
DELETE /api/somecontroller/{id}
```

In this style there is only 2 endpoint formats `/api/somecontroller` and `/api/somecontroller/{id}`
1. `/api/somecontroller` - when we want to obtain a list of items, OR we are creating a new item (resource).
2. `/api/somecontroller/{id}` - when we know the items `{id}` we can get it, update, or delete it.

With rest, we use the HTTP VERBS as they were meant to be used, and 2 different endpoints relating to a resource. Now this same approach can be nested, for filtering, or for more fine-grained resource structures.

What's the big deal?, they both acheive the same thing right?...... one word... _consistency_.  With each new RPC action introduced, a CRUD action name could be anything, with REST, you add new resource endpoints that can be nested in a consistent fashion. Once a frontend developer knows the resource endpoint, than they know how to perform general CRUD actions.

On very small projects with one or two api's it may not be too noticeable, but when you build multitudes of api's, and work with various front-end developers having consistent approaches and standards makes a dramatic difference.

Ok, now check the [Rest API Basics](rest-api-basics.md)