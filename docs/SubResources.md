# Getting Sub-resources

APIs __MAY__ provide a way to fetch sub-resources in a single request when fetching the main resource. They will be able, for example, to fetch all projects when they fetch the account.

The URL might look like:

    /issues/{issueId}?include=comments

This request would get the issue, and any comments that are children of the issue.

This can also be combined with the `fields` query string, to only fetch certain fields of the sub-resources.

    /issues/{issueId}/?include=comments&fields=comments.id,comments.text

This will get the issue, and the `id` and `text` fields of the comments owned by the issue.

> DISCLAIMERS:
>  1. Adding includes to a request can affect performance by potentially requiring n+1 queries
>  2. Having sub resources can impact client side caching since clients may cache the whole response and then requery when just the sub resource is needed

## Sub-resource Pagination

If an included sub-resource is a collection resource, the API __MUST__ keep the size of the included resources to a fixed limit. The exact number of resource in that limit is up to the service, but it must keep payload sizes reasonable. Sub-resource collections of varying length __MUST__ follow the pagination scheme defined [here](Pagination.md).  Sub-resource collections of fixed length may exclude paging if the length is less than the fixed limit.

For more complex scenarios for querying large amounts of data an API __MAY__ implement support for the [GraphQL](https://graphql.org/) query language.

### Example 1 - Included resource, with a small size

    GET /issues?include=comments&limit=2&offset=0

    {
      "pagination": {
        "limit": 2,
        "offset": 0,
        "totalResults": 4,
        "nextUrl": "https://developer.api.autodesk.com/data/v1/issues?include=commentslimit=2&offset=2"
      },
      "results":
      [
        {
          "id": "issueId1",
          "userId": "user3Id",
          "text": "This is an issue with two comments & one attachment",
          "comments": {
            "pagination": {
              "limit": 10,
              "offset": 0,
              "totalResults": 2
            },
            "results": [
              {
                "id": "commentId1",
                "text": "One comment.",
                "user": "userId2"
              },
              {
                "id": "commentId2",
                "text": "I like unicorns.",
                "user": "userId1"
              }
            ]
          }
        }
      ]
    }

### Example 2 - Two included resources, some with large size

In this next example, we get a set of issues, and include comments and attachments on the issues.  Note that the limit in this call applies to the top-level collection, not the sub-resource collection, which has a limit provided by the service. The limit __MAY__ be computed dynamically, based on the number of top-level resources, and the size of each of the included sub-resources.

    GET /issues?include=comments,attachments&limit=2&offset=0

    {
      "pagination": {
        "limit": 2,
        "offset": 0,
        "totalResults": 201,
        "nextUrl": "https://developer.api.autodesk.com/hq/v1/issues?include=comments,attachments&limit=2&offset=2"
      },
      "results": [
        {
          "id": "issueId1",
          "userId": "user3Id",
          "text": "This is an issue with two comments & one attachment",
          "comments": {
            "pagination": {
              "limit": 10,
              "offset": 0,
              "totalResults": 2
            },
            "results": [
              {
                "id": "commentId1",
                "text": "One comment.",
                "user": "userId2"
              },
              {
                "id": "commentId2",
                "text": "I like unicorns.",
                "user": "userId1"
              }
            ]
          }
          "attachments": {
            "pagination": {
              "limit": 10,
              "offset": 0,
              "totalResults": 1
            },
            "results": [
              {
                "id": "attachmentId1",
                "userId": "userId1",
                "attachmentUrl": "https://upload.wikimedia.org/wikipedia/commons/7/70/Oftheunicorn.jpg"
              }
            ]
          }
        },
        {
          "id": "issueId2",
          "name": "This is another issue with too many comments & attachments",
          "comments": {
            "pagination": {
              "limit": 10,
              "offset": 0,
              "totalResults": 12,
              "nextUrl": "https://developer.api.autodesk.com/hq/v1/issues/issue2Id/comments?limit=10&offset=10"
            },
            "results": [
              {
                "id": "commentId1",
                "text": "First comment.",
                "user": "userId12"
              },
              ...,
              {
                "id": "commentId10",
                "text": "Tenth comment.",
                "user": "userId10"
              }
            ]
          },
          "attachments": {
            "pagination": {
              "limit": 10,
              "offset": 0,
              "totalResults": 22,
              "nextUrl": "https://developer.api.autodesk.com/hq/v1/issues/issue2Id/attachments?limit=10&offset=10"
            },
            "results": [
              {
                "id": "attachmentId1",
                "userId": "userId1",
                "attachmentUrl": "https://upload.wikimedia.org/wikipedia/commons/7/70/Oftheunicorn.jpg"
              },
              ...,
              {
                "id": "attachmentId10",
                "userId": "userId10",
                "attachmentUrl": "https://upload.wikimedia.org/wikipedia/commons/6/6f/Topaz-153562.jpg"
              }
            ]
          }
        }
      ]
    }


# Swagger sample for sub-resource

    paths:
      /issues:
        get:
          summary: Get a list of issues
          parameters:
          - name: include
            in: query
            description: The sub-resources to include in the response. A comma-separated list
                of one or both of 'comments' and 'attachments'
            required: false
            type: string
          responses:
            200:
              description: Success.
              schema:
                $ref: '#/definitions/IssuesResponse'
            400:
              description: Failure.
              schema:
                $ref: '#/definitions/ApiError'

    definitions:
      Issue:
        type: object
        required:
          - id
          - userId
          - text
        properties:
          id:
            description: The GUID identifier for the issue
            type: string
          userId:
            description: The user id of the creator of this issue.
            type: string
          text:
            description: The content of the issue.
            type: string
          comments:
            $ref: '#/definitions/CommentsResponse'
          attachments:
            $ref: '#/definitions/AttachmentsResponse'

      Comment:
        type: object
        required:
          - id
          - userId
          - text
        properties:
          id:
            description: The GUID identifier for the comment
            type: string
          userId:
            description: The user id of the creator of this comment.
            type: string
          text:
            description: The content of the comment.
            type: string

      Attachment:
        type: object
        required:
          - id
          - userId
          - attachmentUrl
        properties:
          id:
            description: The GUID identifier for the attachment.
            type: string
          userId:
            description: The user id of the creator of this attachment.
            type: string
          attachmentUrl:
            description: The URL of the attached resource.

      Pagination:
        type: object
        required:
          - limit
          - offset
          - totalResults
        properties:
          limit:
            type: integer
          offset:
              type: integer
          totalResults:
            type: integer
          next:
            type: string

      AttachmentsResponse:
        type: object
        required:
          - results
        properties:
          results:
            type: array
            items:
              $ref: '#/definitions/Attachment'
          pagination:
              $ref: '#/definitions/Pagination'

      CommentsResponse:
        type: object
        required:
          - results
        properties:
          results:
            type: array
            items:
              $ref: '#/definitions/Comment'
          pagination:
              $ref: '#/definitions/Pagination'

      IssuesResponse:
        type: object
        required:
          - results
        properties:
          results:
            type: array
            items:
              $ref: '#/definitions/Issue'
          pagination:
              $ref: '#/definitions/Pagination'

      ApiError:
        type: object
        required:
          - type
          - title
        properties:
          title:
            type: string
          detail:
            type: string
          type:
            type: string
