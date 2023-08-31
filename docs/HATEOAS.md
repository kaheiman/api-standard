# Hypermedia links - HATEOAS

Services **MAY** use hypermedia as a way to express relationships and convey available state transitions for a resource at runtime. Hypermedia is part of level 3 of the [Richardson Maturity model](https://martinfowler.com/articles/richardsonMaturityModel.html) and is commonly called "hypermedia as the engine of application state" ([HATEOAS](https://en.wikipedia.org/wiki/HATEOAS)).

## Hyperemedia Format

Services using hypermedia **MUST** use [RESTful JSON](http://restfuljson.org) as the format for hypermedia. RESTful JSON has two rules for using links within API responses.

1. JSON objects MAY include a `url` property to indicate a link to itself
1. JSON objects MAY append `Url` to property names to indicate related links (e.g. `ordersUrl`)

RESTful JSON also supports a `profileUrl` property which corresponds to [RFC 6906](https://tools.ietf.org/html/rfc6906) that MAY be used to link to documentation about the resource. The `application/json` media type MUST be used as the `Content-Type` with RESTful JSON responses.

## Web Linking

Hyperlinks found in API responses MUST be documented when used, specifically stating what resource and operation to which the link points. JSON properties that are links SHOULD define the `format` in the JSON Schema property as `url` in the OpenAPI document. Services using hypermedia **SHOULD** include a `url` link on every response. For more information about how to use URLs in the response, please see the [links page](Links.md).

## On Using Hyperlinks

This section is meant to be informative and not normative. Hyperlinks are useful in an API to reduce coupling and allow for evolvability. There are some pragmatic ways to use hyperlinks in services.

- Instead of using a UUID to define a resource, an API could use hyperlinks with a `url` link.
- Instead of nesting resource names in the URI (e.g. `/customers/3/orders`) an API could use a link to the related resource (e.g. use an `ordersUrl` property with a URL).
- Instead of rebuilding logic in the client as with pagination, an API could include links that walk the client through a workflow.
- Instead of asking clients to modify a `status` property to change states, an API could provide links for available transitions.

Hypermedia can reduce coupling between clients and servers and can increase expressibility for services. It does come with tradeoffs, such as larger response sizes and increased number of requests in some cases. To make the best use of hypermedia and minimize the impact of these tradeoffs, good design is important along with a solid caching strategy.

## Example

This example shows a RESTful JSON representation of a customer.

```json
{
    "url": "...",
    "name": "Jane Doe",
    "email": "jdoe@exampl.com",
    "ordersUrl": "...",
    "nextUrl": "..." ,
    "prevUrl": "..."
}
```

This response gives the URL of the object with the `url` property, the orders owned by the customer for `ordersUrl`, and links to the next and previous customers with `nextUrl` and `prevUrl` respectively. The design of the links and their relationships with other resources is dependent on the design of the API in which they are used.

## More Information and Examples

- [Understanding HATEOAS - Spring](https://spring.io/understanding/HATEOAS)
- [What is Hypermedia?](https://smartbear.com/learn/api-design/what-is-hypermedia/)
- [GitHub API and Hypermedia](https://developer.github.com/v3/#hypermedia)
- [PayPal API and Hypermedia](https://github.com/paypal/api-standards/blob/master/api-style-guide.md#hypermedia)
- [AWS API Gateway API, Hypermedia, and HAL](https://docs.aws.amazon.com/apigateway/api-reference/)
