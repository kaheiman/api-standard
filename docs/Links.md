# Representing links in a response body

Many APIs return links to resources, such as [pagination](Pagination.md), [sub-resource overflows](Sub_Resources.md) and, in general, [HATEOAS APIs](HATEOAS.md). They are references to REST endpoints that should be accessed via the __GET__ method, unless documented otherwise.

All links  __MUST__ follow the format specified in  [RFC3986](https://tools.ietf.org/html/rfc3986).

When a response body contains a link to another resource, it __SHOULD__ return a [URL](https://tools.ietf.org/html/rfc3986#section-1.1.3), with the protocol, host, and resource path including the version, and optionally query parameters.

If it is not practical to support a full URL link, an API __MAY__ return an [absolute-path reference](https://tools.ietf.org/html/rfc3986#section-4.2). These references begin with the `/` character, and include the service name and version. Absolute-path references use the same protocol and domain that the original call used.

In some cases, links __MAY__ return a [relative-path reference](https://tools.ietf.org/html/rfc3986#section-4.2). Relative-path references __SHOULD NOT__ be used for references to resource in a API, but can be used for other purposes, such as links to static resources.

APIs returning links __MUST__ document what resources are being linked to, so that the client can know how to parse the response body, and what authentication the call requires. Additionally, the documentation __MUST__ specify whether the API returns a absolute reference, or an absolute-path reference.

Services returning links **MUST** use [RESTful JSON](http://restfuljson.org) as the format for links.

> JSON objects __MUST__ append `Url` to property names to indicate related links (e.g. `ordersUrl`)

## URL

A URL includes the protocol, host, resource path and optional query parameters and optional fragment.

## Example of returning a URL

The following call receives a response with a URL link in the "nextUrl" field. This is the preferred form for a link.

    GET /v1/folder/contents?limit=20&offset=0

    {
      "pagination": {
        "limit": 20,
        "offset": 0,
        "totalResults": 201,
        "nextUrl": "https://developer.api.autodesk.com/data/v1/folder/contents?limit=20&offset=20"
      },
      "results": [
        ...
      ]
    }

## Absolute-path reference

An absolute-path reference excludes the protocol and domain, but includes the resource path, starting with the `/` character, and optional query parameters.

## Example of returning an absolute-path reference

The following call receives a response with an absolute-path reference link in the "nextUrl" field.

    GET /v1/folder/contents?limit=20&offset=0

    {
      "pagination": {
        "limit": 20,
        "offset": 0,
        "totalResults": 201,
        "nextUrl": "/data/v1/folder/contents?limit=20&offset=20"
      },
      "results": [
        ...
      ]
    }
