# Response Status Codes

+ Every API **MUST** use the appropriate HTTP Status Codes to communicate the result of a request operation.
+ Every API designer, implementer, and consumer **MUST** understand the semantics of the HTTP Status Code he is using.
+ At a minimum, everyone **MUST** be familiar with the semantics of "Common" HTTP Status Codes.

## Recommended Status Codes

To keep consistency, APIs **SHOULD NOT** use any status codes that are not listed here:

| Code | Test | Description |
|----|-----|----------|
|200|	OK|	The request has succeeded.|
|201|	Created|	The request has been fulfilled and has resulted in one or more new resources being created.|
|202|	Accepted|	The request has been accepted for processing, but the processing has not been completed.|
|204|	No Content|	The server has successfully fulfilled the request, there is no additional content to send in the response payload body.|
|207|	Multi-Status|	A Multi-Status response conveys information about multiple resources in situations where multiple status codes might be appropriate.|
|303|	See Other |	The response to the request can be found under another URI using a GET method |
|304|	Not Modified|	There was no new data to return.|
|400|	Bad Request|	The request was invalid or cannot be otherwise served. An accompanying error message will explain further.|
|401|	Unauthorized|	Missing or incorrect authentication credentials.|
|403|	Forbidden|	The request is understood, but it has been refused or access is not allowed. An accompanying error message will explain why.|
|404|	Not Found|	The URI requested is invalid or the resource requested, such as a user, does not exist. |
|405|	Method Not Allowed|	The request method is not supported for the requested resource. |
|406|	Not Acceptable|	Returned when an invalid format is specified in the request.|
|409|	Conflict|	The request could not be completed due to a conflict with the current state of the target resource.|
|410|	Gone|	This resource is gone. Used to indicate that an API endpoint has been turned off.|
|412|	Precondition Failed|	 This happens with conditional requests on methods other than GET when the condition defined by the If-Unmodified-Since or If-None-Match headers is not fulfilled.|
|415|	Unsupported Media Type|	The server refuses to accept the request because the payload format is in an unsupported format.|
|422| Unprocessable Entity | The server understands the content type of the request entity and the syntax of the request entity is correct, but was unable to process the request contents. An accompanying error message will explain further. |
|429|	Too Many Requests|	Returned when a request cannot be served due to the application’s rate limit having been exhausted for the resource. Response __SHOULD__ include a 'Retry-After' header. |
|500|	Internal Server Error|	Something is broken. This is usually a temporary error, for example in a high load situation or if an endpoint is temporarily having issues.|
|502|	Bad Gateway|	API is down, or being upgraded.|
|503|   Service Unavailable| API service is down or otherwise unavailable. Normally only generated by API gateways, not the service itself.|
|504|  Gateway Timeout| A timeout occured while attempting to reach API service. Normally only generated by API gateways, not the service itself.|

Recommended Reading
+ [How to Think About HTTP Status Codes](https://www.mnot.net/blog/2017/05/11/status_codes)