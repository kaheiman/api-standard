
# Standard Methods

This chapter defines the concept of standard methods, which are List, Get, Create, Update, and Delete. Standard methods reduce complexity and increase consistency.

The following table describes how to map standard methods to HTTP methods:

| Standard Method | HTTP Mapping | HTTP Request Body | HTTP Repsonse Body|
|-------------|-----------|---------------|--------------|
|List|	GET \<collection URL\>|	N/A|	Resource\* list|
|Get|	GET \<resource URL\>|	N/A|	Resource\* |
|Create|	POST \<collection URL\>|	Resource|	Resource\* |
|Update|	PUT or PATCH \<resource URL\>|	Resource|	Resource\* |
|Delete|	DELETE \<resource URL\>|	N/A|	Empty\*\*|

*The resource returned from List, Get, Create, and Update methods **MAY** contain partial data if the methods support response field masks, which specify a subset of fields to be returned. In some cases, the API platform natively supports field masks for all methods.

**The response returned from a Delete method that doesn't immediately remove the resource (such as updating a flag or creating a long-running delete operation) **SHOULD** contain either the long-running operation or the modified resource.

A standard method **MAY** also return a long-running operation for requests that do not complete within the time-span of the single API call.

The following sections describe each of the standard methods in detail.



## List
The List method takes a collection resource and zero or more parameters as input and returns a list of resources that match the input.

List is commonly used to search for resources. List is suited to data from a single collection that is bounded in size and not cached. For broader cases, the [custom method](Custom_Methods.md) Search **SHOULD** be used.

A batch get (such as a method that takes multiple resource IDs and returns an object for each of those IDs) **SHOULD** be implemented as a custom BatchGet method, rather than a List. However, if you have an already-existing List method that provides the same functionality, you may reuse the List method for this purpose instead. If you are using a custom BatchGet method, it **SHOULD** be mapped to HTTP GET.

Applicable common patterns: [pagination](Pagination.md), [sorting](Sorting.md), [filtering](Filtering.md) and [fetching sub-resources](SubResources.md).



### HTTP mapping:

+ The List method **MUST** use an HTTP GET verb.
+ There is no request body.
+ The response body **SHOULD** contain a list of resources along with optional metadata.
##### Example

> GET /banners  

    200 OK
    [
        {
            "id": "b87c4886-6e28-4dc0-9c45-7f5d00029537",
            "name": "First Banner",
            "createdAt": "2017-11-27 11:53:54",
            "updatedAt": "2017-11-27 14:53:54"
        },
        {  
            "id": "b87c4886-6e28-4dc0-9c45-7f5d00029537",
            "name": "Second banner",
            "createdAt": "2017-11-28 10:55:54",
            "updatedAt": "2017-11-28 11:12:54"
        }
    ]

##### Swagger
   
    paths:
      '/banners':
        get:
          responses:
            200:
              description: Success
              schema:
                type: array
                items:
                  $ref: '#/definitions/Banner'

## Get
The Get method takes a resource name, zero or more parameters, and returns the specified resource.

HTTP mapping:

+ The Get method MUST use an HTTP GET verb.
+ There is no request body.
+ The returned resource SHALL map to the entire response body.
Example:

##### Example

> GET /banners/b87c4886-6e28-4dc0-9c45-7f5d00029537  

    200 OK
    {
        "id": "b87c4886-6e28-4dc0-9c45-7f5d00029537",
        "name": "First Banner",
        "createdAt": "2017-11-27 11:53:54",
        "updatedAt": "2017-11-27 14:53:54"
    }

##### Swagger
    paths:
      '/banners/{id}':
        get:
          parameters:
            - name: id
              required: true
              description: The banner UUID identifier
              type: string
              in: path
          responses:
            200:
              description: Success
              schema:
                $ref: '#/definitions/Banner'

## Create

The Create method takes a parent resource name, a resource, and zero or more parameters. It creates a new resource under the specified parent and returns the newly created resource.

If an API supports creating resources, it SHOULD have a Create method for each type of resource that can be created.

### HTTP mapping:

+ The Create method **MUST** use an HTTP POST verb.
+ The request message field containing the resource **SHOULD** map to the request body.
+ The returned resource **SHALL** map to the entire response body.
##### Example

>     POST /banners
>     {  
>         "name": "First Banner"  
>     }  

    201 Created
    {
        "id": "b87c4886-6e28-4dc0-9c45-7f5d00029537",
        "name": "First Banner",
        "createdAt": "2017-11-27 11:53:54",
        "updatedAt": "2017-11-27 14:53:54"
    }

##### Swagger
    paths:
      '/banners':
        post:
          parameters:
            - name: data
              in: body
              schema:
                $ref: '#/definitions/Banner'
          responses:
            201:
              description: Success
              schema:
                $ref: '#/definitions/Banner'

## Update
The Update method takes a request message containing a resource and zero or more parameters. It updates the specified resource and returns the updated resource.

### HTTP mapping:

+ The standard Update method **SHOULD** support partial resource update and use HTTP verb PATCH.
+ The PATCH methods __SHOULD__ support the [JSON Merge Patch (RFC 7396)](https://tools.ietf.org/html/rfc7396) format.
+ An Update method that requires more advanced patching semantics, such as appending to an array field, **MAY** use the [JSON Patch (RFC 6902)](https://tools.ietf.org/html/rfc6902) standard. 
+ For more complex, or non-standard patching behavior, a custom method __MAY__ be used.
+ Supporting a full update with the http verb PUT is highly discouraged because it has backward compatibility issues when adding new resource fields. 
+ The request message field containing the resource **MUST** map to the request body.
+ All remaining request message fields **MUST** map to the URL query parameters.
+ The response message **MUST** be the updated resource itself.

When using the PATCH verb, the documentation __MUST__ specify what protocol will be used to specify the updated data. The format __MUST__ be one of the following:

1. `JSON Merge Patch` - Most APIs should use this simple format for
   patching. The API method __MUST__ support both `application/json`
   and `application/merge-patch+json` content types. The first is to
   provide a consistent API with all the other methods, and the second
   is to ensure the API conforms with RFC 7396.
2. `JSON Patch` - If there is a valid use-case for it, APIs may use the JSON Patch format. The content type for the method __MUST__ be `application/json-patch+json`. 

##### Example - JSON Merge Patch

>     PATCH /banners/b87c4886-6e28-4dc0-9c45-7f5d00029537  
>     Content-Type: application/json
>     {  
>         "name": "Second Banner"  
>     }  

    200 OK
    {
        "id": "b87c4886-6e28-4dc0-9c45-7f5d00029537",
        "name": "Second Banner",
        "createdAt": "2017-11-27 11:53:54",
        "updatedAt": "2017-11-27 20:20:05"
    }

## Delete
The Delete method takes a resource name and zero or more parameters, and deletes or schedules for deletion the specified resource. The Delete method **SHOULD** return '204 No Content' response.

An API should not rely on any information returned by a Delete method, as it cannot be invoked repeatedly.

### HTTP mapping:

+ The Delete method **MUST** use an HTTP DELETE verb.
+ There is no request body.
+ If the Delete method immediately removes the resource, it **SHOULD** return a '204 No Content' response.
+ If the Delete method initiates a long-running operation, it **SHOULD** return the long-running operation.
+ If the Delete method only marks the resource as being deleted, it **SHOULD** return the updated resource.  

Calls to the Delete method **SHOULD** be idempotent in effect but do not need to yield the same response. Any number of Delete requests **SHOULD** result in a resource being (eventually) deleted, but only the first request **SHOULD** result in a success code. Subsequent requests **SHOULD** result in a '404 Not Found'.

##### Example
> DELETE /banners/b87c4886-6e28-4dc0-9c45-7f5d00029537  

    204 No Content


##### Swagger
    paths:
      '/banners/{id}':
        delete:
          parameters:
            - name: id
              required: true
              description: The banner UUID identifier
              type: string
              in: path
          responses:
            204:
              description: Success

## Partial Fields 

The API **MAY** support fetching a subset of the fields of objects. For details, see [partial fields](Partial_Fields.md). 

## Fetching sub-resources

An API **MAY** fetch sub-resources on demand, so they will be able, for example, to fetch all projects when they fetch an account. 

The URL might look like `accounts/{accountId}?include=projects` This request would get the account, and any projects resources that are children of the account.

This can also be combined with the fields query string, to just fetch certain fields of the sub-resources. `accounts/{accountId}/?fields=projects.id,projects.name&include=projects` This will get the account, and the id and name fields of the projects owned by the account.

This is not strictly a REST API, but it offers a convenient and efficient way to fetch related data.

[More Details](SubResources.md), including pagination of sub-resources.

## Errors

If the `include` parameter specifies a field that doesn't exist, or does not have a string or numeric value, then the request will return a `400 Bad Request` response, and a body with an error message conforming to our [error response standard](Error_Responses.md).

## Example

    GET /employees?include=foo

Response:

    400
    {
      "title": "The specified included field does not exist",
      "detail": "Employee resources do not have a field called 'foo.'"
    }
