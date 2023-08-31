# Bulk Operations

APIs __MAY__ elect to support bulk methods for operating on many resources in a single request. This should be done using [custom http methods](Custom_Methods.md). The API __MUST__ expose endpoints for specific operations.

For any endpoint which implements a bulk operation, there __MUST__ be a limit on the number of resources that the operation can process. The API should be aware of its limits, and cap the number of operations per request accordingly. If a client passes in too many operations, the API should return a 400 code, with an error message explaining why the request was rejected.

## Error States

If all the operations in a bulk request succeed, API should respond with a 2XX code, and if results are returned, it should be in the `results` property of the response body.

If all operations fail, it should return the common 4XX/5XX status code, and a standard error response in the `errors` property of the response body.

If some operations succeeded and some failed, it should return a 207 status code and the results for the successful operations in the `results` property of the response, and the errors in the `errors` property. There __MUST__ be enough information in the error response to know which operations failed.

If the API returns 207 status code, the `errors` property contains one or more errors. Clients could interogate the `errors` property and act accordingly.

## Examples

In an API supporting a `/messages` resource collection, the API can offer custom endpoints to delete or create many messages at the same time.

Batch delete example:

    POST /messages:batch-delete
    ["4e610d92-f155-4da2-84ae-fceb64fcd390", 
     "4091b8b8-81e4-4a11-8ae0-c04723c10db3"]
    200

Batch create example, with one error:

    POST /message:batch-create
    [
     {
      "userId": "5583763a-817e-4c84-8b6a-be1bc0f678b3",
      "content": "This is a message"
     },
     {
      "userId": "603a5ad8-1c13-440e-b9d0-fbbde778ae86",
      "content": "Another message"
     }
    ]
    207
    {
        "results": 
          [
            {
              "messageId": "af958348-d157-4b2d-824f-f9d5bfed93c4",
              "userId": "5583763a-817e-4c84-8b6a-be1bc0f678b3",
              "content": "This is a message"
            }
          ],
        "errors": 
          [
           {
             "title": "Creation failed",
             "detail": "User Id does not exist",
             "id": "603a5ad8-1c13-440e-b9d0-fbbde778ae86"
           }
          ]
    }

Batch patch example:

Batch patch __SHOULD__ follow either [RFC 6902](https://tools.ietf.org/html/rfc6902) or [RFC 7396](https://tools.ietf.org/html/rfc7396) formats, but __SHOULD NOT__ be mixed. The Content-Type header SHOULD reflect the format chosen for the individual patch request.  

Using RFC 6902

    PATCH /messages:batch-patch
    HEADER Content-Type: application/json-patch+json
    {"4e610d92-f155-4da2-84ae-fceb64fcd390":  { "op": "replace", "path": "content", "value": "this is the patched message" }
     "4091b8b8-81e4-4a11-8ae0-c04723c10db3": { "op": "replace", "path": "userId", "value": "this is the patched message user" }}
    200

Using RFC 7396

    PATCH /messages:batch-patch
    HEADER Content-Type: application/merge-patch+json
    {"4e610d92-f155-4da2-84ae-fceb64fcd390": { "content": "this is the patched message" } 
     "4091b8b8-81e4-4a11-8ae0-c04723c10db3": { "userId": "this is the patched message user" }}
    200

Here is an example of the OpenAPI description for an API with batch create and batch delete operations.

    paths:
      '/messages:batch-delete':
        post:
          description: Delete multiple messages
          parameters: 
           - name: idArray
             in: body
             required: true
             description: An array of resource Ids
             schema:
               type: array
               items: 
                 $ref: '#/definitions/GUID'     
          responses:
            200: 
              description: Successfully deleted messages
            400:
              description: Bad message Id(s) or too many Ids
              schema:
                $ref: '#/definitions/ApiError'
      
      '/messages:batch-create':
        post:
          description: Create multiple messages
          parameters: 
           - name: messageData
             in: body
             required: true
             description: An array of message data
             schema:
               type: array
               items: 
                 $ref: '#/definitions/Message'     
          responses:
            200: 
              description: Successfully created messages
              schema:
                type: array
                items:
                  $ref: '#/definitions/NewMessage'
            400:
              description: Bad message data.
              schema:
                $ref: '#/definitions/ApiError'

## Generic batch endpoints

A service __MAY__ implement a generic batch endpoint that allows users to batch together several individual requests in a single call. It is recommended that services implementing generic batch requests follow the [pattern that Google APIs](https://developers.google.com/gmail/api/guides/batch) use for this.

An API supporting generic batching __MUST__ only implement a homogeneous batch request mechanism, *i.e.* only batch requests to its own resources, not other Autodesk services.
