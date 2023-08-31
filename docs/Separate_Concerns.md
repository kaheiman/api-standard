# Separate Concerns

Every API using HTTP/S API **MUST** precisely follow the separation of concerns principle of an HTTP message:

1. A resource identifier–URI **MUST** be used to indicate **identity** only (except of version prefix)
1. HTTP request method **MUST** be used to communicate the **action semantics** (intent and safety)
1. HTTP response status code **MUST** be used to communicate the **information about the result** of the attempt to understand and satisfy the request
1. HTTP message body **MUST** be used to transfer the **message content**
1. HTTP message headers **MUST** be used to transfer the **metadata** about the message and its content
1. URI query parameter **SHOULD NOT** be used to transfer metadata

## Example 1
The rule

> A resource identifier–URI **MUST** be used to indicate identity only

implies there **MUST NOT** be any information about the representation media type, version of the resource or anything else in the URI.

For example, URI */greeting.json* is **illegal** as it is not used for identification of a resource only but they convey the information about representation format or version.

URIs are not meant to carry any other information but the identifier of the resource.

## Example 2
The rule

> HTTP message body **MUST** be used to transfer the message content

Implies an HTTP GET request **MUST NOT** use HTTP message body to identify the resource. For example a request:

    GET /greeting HTTP/1.1
    Content-Type: application/json
    ...
  
    {
        "filter": "string"
        "depth": 3
    }

is **not acceptable** (ignoring the fact that HTTP GET method shouldn't have the body). To express identity use URI and query parameters instead e.g: */greeting?filter=string&depth=3*.



>*Keep things simple while designing by separating the concerns between the different parts of the request and response cycle. Keeping simple rules here allows for greater focus on larger and harder problems.*  
>
>*Requests and responses will be made to address a particular resource or collection. Use the path to indicate identity, the body to transfer the contents and headers to communicate metadata. Query params may be used as a means to pass header information also in edge cases, but headers are preferred as they are more flexible and can convey more diverse information.* 
>  
>– [Heroku HTTP API Design Guide](https://geemus.gitbooks.io/http-api-design/content/en/foundations/separate-concerns.html)

