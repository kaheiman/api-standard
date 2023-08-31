# Error Responses

An error response is intended for use with the HTTP status codes 4xx and 5xx. Error response **MUST NOT** be used with 2xx status code responses.

At the minimum, any error response **MUST** have the 'title' and 'detail' fields.

### Example

    {
      "title": "Authentication required",
      "detail": "Missing authentication credentials for the Greeting resource."
    }

## Optional Fields

It **SHOULD** have the 'type' field with the identifier of the error.

    {
      "type": "https://developer.api.autodesk.com/problems/scv/unauthorized",
      "title": "Authentication required",
      "detail": "Missing authentication credentials for the Greeting resource."
    }
> **NOTE:** The type field is an identifier, and as such it **MAY** be used to denote additional error codes.

## Additional Fields

If needed, the error response **MAY** include additional fields.

## Validation Errors

When necessary, an error response **MAY** include additional error details about the problems that have occurred.

These additional errors **MUST** be under the *errors* key and **MUST** follow the error response structure with additional *field* key which determines the source of the error.

### Example

Request:

    POST /my-resource HTTP/1.1  
    Content-Type: application/json
     
    {  
      "age": -32,  
      "color": "cyan"  
    }

Response:

    HTTP/1.1 400 Bad Request
    Content-Type: application/json
    Content-Language: en
     
    {
      "type": "https://example.net/validation_error",
      "title": "Your request parameters didn't validate.",
 
      "errors": [
        {
          "type": "https://example.net/invalid_params",
          "field": "age",
          "title": "Invalid Parameter",
          "detail": "age must be a positive integer"
        },
        {
          "type": "https://example.net/invalid_params",
          "field": "color",
          "title": "Invalid Parameter",
          "detail": "color must be 'green', 'red' or 'blue'"
        }
      ]
    }

The problem type is a URI reference that identifies the  problem.  When dereferenced, it __SHOULD__ provide human-readable documentation for the problem. It is a unique string that identifies the type of problem, and can be used for other purposes, such as a key for a localized string explaining the problem type.

## No Stack Traces or Server Logs

*Error details are not a debugging tool for the underlying implementation; rather, they are a way to expose greater detail about the HTTP interface itself.*

An error response **MUST NOT** contain a program stack trace or server log for debugging purposes. Instead, provide a logref field with reference to the particular server log.

These recommendations are based on the [Problem Details RFC](https://tools.ietf.org/html/rfc7807)
