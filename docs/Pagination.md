# Pagination

Access to lists of data items __MAY__ support pagination for best client-side batch processing and iteration experience. This holds true for all lists that are (potentially) larger than just a few hundred entries.

There are two pagination techniques:

- Offset/Limit-based pagination: numeric offset identifies the first entry in the page
- Cursor-based — aka key-based — pagination: a unique key element identifies the first entry in the page.

## Offset/Limit-based Pagination (Preferred Option)

An offset-based pagination __MUST__ accept the following query parameters:

- offset: Offset from the start of the collection to the first entry in the page. Zero based.
- limit: Determines the maximum number of objects that __MAY__ be returned. A query __MAY__ return fewer than the value of limit due to filtering or other reasons.

The response of the offset based pagination MUST contain a pagination object with the following fields:

- offset: The offset value supplied in the request.
- limit: The limit value used by the server in the response. Note that this will be different from request in case of server overrides.
- totalResults: Optional. The total number of results that match the query irrespective of limit.
- nextUrl: [Link](Links.md) that will return the next page of data. If not included, this is the last page of data. It is allowed that a page may be empty but contain a next paging link. Consumers should stop paging only when the next link no longer appears.
- previousUrl: Optional. [Link](Links.md) that will return the previous page of data. If the previous link is supported, then the absence indicates the first page of results.
- nextOffset: Optional. The offset parameter that must be used to request the next page. This field is intended for clients that programmatically create the request URL instead of using the nextUrl field.
- previousOffset: Optional. The offset parameter that must be used to request the previous page. This field is intended for clients that programmatically create the request URL instead of using the nextUrl field.
Example

A response of offset-based paginated collection:

    {
      "pagination": {
        "limit": 20,
        "offset": 0,
        "totalResults": 201,
        "nextUrl": "https://developer.api.autodesk.com/data/v1/folder/contents?limit=20&offset=20"
      },
      "results": [
        {
          ...
        },
        {
          ...
        },
        ...
      ]
    }

## Cursor-based pagination

A cursor-based pagination accepts the following query parameters:

- limit: Determines the maximum number of objects that MAY be returned. A query MAY return fewer than the value of limit due to filtering or other reasons.

The response of the cursor-based pagination MUST contain a pagination object with the following fields:

- limit: The limit value supplied in the request.
- totalResults: Optional. The total number of results that match the query irrespective of limit.
- nextUrl: [Link](Links.md) that will return the next page of data. If not included, this is the last page of data. It is allowed that a page may be empty but contain a next paging link. Consumers should stop paging only when the next link no longer appears. The next URL is supposed to be opaque and the client should navigate to the URL to fetch next set of results. The format of the URL would be implementation dependent and hence Clients shouldn't attempt to change or update the URL in any manner. Similarly, depending on the implementation, the next URL may expire and hence storing or bookmarking is not recommended.
- previousUrl: Optional. [Link](Links.md) that will return the previous page of data. If the previous link is being returned then absence indicates this being the first page. All caveats as stated for opaqueness of next link apply to previous as well.
- nextCursorState: Optional. The cursorState parameter that must be used to request the next page. This field is intended for clients that programmatically create the request URL instead of using the nextUrl field.
- previousCursorState: Optional. The cursorState parameter that must be used to request the previous page. This field is intended for clients that programmatically create the request URL instead of using the nextUrl field.
- 
Example: Request would be /folder/contents?limit=20

An example of opaque URL could be /folder/contents?cursorState=AMMAEACAtgALYWRzay53aXBkZXYNZnMuZmlsZS5hZGRlZAZmb2xk
Check out the next link on one of the Forge APIs for a real example

Example

A response of cursor-based paginated collection:

    {
      "pagination": {
        "limit": 20,
        "nextUrl": "https://developer.api.autodesk.com/data/v1/folder/contents?cursorState=AMMAEACAtgALYWRzay53aXBkZXYNZnMuZmlsZS5hZGRlZAZmb2xk"
      },
      "results": [
        {
          ...
        },
        {
          ...
        },
        ...
      ]
    }

> Note: Changing non-paginated collection to be paginated (or the opposite) doesn't follow the rules for extending and will result a new API version
> For cases that the client requests more records then the server can provide, the server **MUST** fallback to the max limit and not throw an error response (since the max limit might change, and we don't want to break the API consumers).

## Pagination via POST

Pagination can happen for POST endpoint such as the custom method [Filter](Filtering.md#Custom-method-Filter).

Instead of `nextUrl` in the pagination object, a `nextPost` object is returned containing the `url` and `body` to POST for the next page of result

`nextPost` and `prevPost` will have the following:

- url: The url to POST along with the body. May be the same URL as the original request.
- body: An object representing the body to be POST for the next set of results.

If `nextPost` is null then the endpoint has returned all results.

The below example is a POST cursor-based pagination therefore `nextPost.body` has a `cursorState`.

Example

    {
      "pagination": {
        "limit": 20,
        "nextPost": {
          "url": "https://developer.api.autodesk.com/construction/photos/v1/project/8e52b200-f856-4f17-a57c-c03ba2f6673a/photos:filter",
          "body": {
            "cursorState": "AMMAEACAtgALYWRzay53aXBkZXYNZnMuZmlsZS5hZGRlZAZmb2xk"
          }
        }
      },
      "results": [
        {
          ...
        },
        {
          ...
        },
        ...
      ]
    }

If the POST pagination is offset-based it will contain `offset` and `limit` instead

Example

    {
      "pagination": {
        "limit": 20,
        "nextPost": {
          "url": "https://developer.api.autodesk.com/construction/photos/v1/project/8e52b200-f856-4f17-a57c-c03ba2f6673a/photos:filter",
          "body": {
            "offset": 160,
            "limit": 20,
          }
        }
      },
      ..
    }

## Compound Collection Operations

Filtering, Sorting and Pagination operations MAY all be performed against a given collection. When these operations are performed together, the evaluation order MUST be:

1. Filtering. This includes all range expressions performed as an AND operation.
2. Sorting. The potentially filtered list is sorted according to the sort criteria.
3. Pagination. The materialized paginated view is presented over the filtered, sorted list. This applies to both server-driven pagination and client-driven pagination.
