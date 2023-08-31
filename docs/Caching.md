# Caching

Services __SHOULD__ properly support caching, when appropriate. 

No single caching policy works well in all situations. Depending on traffic, type of data served and application requirements you should define a caching policy that works best for you. The links at the bottom of this page provide more information on defining a caching policy appropriate for you service.

+ For GET requests, server responses __SHOULD__ include an `ETag` header, and __SHOULD__ also include a `Last-Modified` header. If only one header is supported, it __SHOULD__ be `ETag`.
+ When resource responses include an `ETag` header, servers __MUST__ support `If-None-Match: etag` headers for GET requests, and __SHOULD__ support `If-Match: etag` headers for state-changing requests, such as PUT, POST, DELETE and PATCH.
+ When resource responses include an `Last-Modified` header, servers __MUST__ support `If-Modified-Since: date` headers for GET requests, and __SHOULD__ support `If-Unmodified-Since: date` headers for state-changing requests, such as PUT, POST, DELETE and PATCH.
+ Servers __SHOULD__ include "Cache-Control" headers in responses to GET requests and explicitly decide what the caching policy for their resources should be​.
+ For other, non-GET, requests, the server __MAY__ include ETag or last modified dates, depending on the use-case. 

`Etag` and `Last-Modified` headers and conditional requests can be used to avoid concurrent updates to a model. For more details, please see [Avoiding mid-air collisions](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag#Examples).

## Further Reading

+ [HTTP/1.1: Conditional Requests, RFC 7232](https://tools.ietf.org/html/rfc7232)
+ [Google caching guidelines](https://developers.google.com/web/fundamentals/performance/optimizing-content-efficiency/http-caching)
+ [Adidas API Guidelines on Caching](https://adidas.gitbook.io/api-guidelines/rest-api-guidelines/quality/execution/caching#etag)
+ [Boost Your REST API with HTTP Caching](https://www.kennethlange.com/boost-your-rest-api-with-http-caching/)
