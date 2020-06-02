# Rest API Basics

From here on, "items" refers to "resource". A resource in rest terminology is a term to define an entity of some sort, it could be a database entry, cache, or even a process, it's often used for crud like data querying and updates. 

Create api endpoints like so

```c#
GET /api/somecontroller                 // get a list of items
POST /api/somecontroller                // create a new item, receive a 201 Created

GET /api/somecontroller/{id}            // get an existing item by id 200 Ok
DELETE /api/somecontroller/{id}         // delete existing item by id, 200 Ok
PUT /api/somecontroller/{id}            // update existing item by id, 200 Ok or 204
PATCH /api/somecontroller/{id}          // update specific properties within an existing item by id, 200Ok or 204
```

- DO use verbs and `{id}` when dealing with a specific item.
- DO return 201 Created, and set RESPONSE "location" header for POSTing a new item
- DO nest further REST like endpoints for filtering, grouping if needed
- DONT expect an api to find a resources primary id through something sent up


## Get list of items
```c#
GET /api/somecontroller
```
Here you will retrieve all, or some records for this resource, in order to filter, you could nest the endpoint to provide a new resource endpoint

## Create new item
```c#
POST /api/somecontroller
```
Create a new item, and if successful
1. Return 201 Created (new resource)
2. in response location header, set the url to fetch the resource, effectively the GET url `/api/somecontroller/{id}` with new id obtained.

## Crud
Once the client knows the items `{id}` property, it's than very simple and consistent to apply any further operations on that item.