# Filtering

A search (filter) operation on a collection resource __SHOULD__ be defined as safe, idempotent and cacheable, therefore using the GET HTTP request method.

Every search parameter __SHOULD__ be provided in the form of a query parameter. In the case of search parameters being mutually exclusive or requiring the presence of another parameter, the explanation **MUST** be part of operation's description.

Filters are specified in the query string using the name of a field, and a value or range of values to test against.  The syntax in the query string is:

    ?filter[<fieldName>]=<valueToFilterOn>

Filters **MAY** supply a range of values rather than a specific value, using the `..` notation as shown in the examples below. This can be used for numeric ranges and date ranges.

## Examples

    GET /books?filter[title]=Great Expectations

Will return all books with the title `Great Expectations`.

    GET /books?filter[price]=10..20

Will return all books with a price in the range of 10 to 20, inclusive.

    GET /books?filter[price]=..50

Returns all the books with a price up to 50 dollars.

    GET /books?filter[price]=10..&filter[title]=The Bible

Will return all books with a price over 10, and the title `The Bible`.

### Example Swagger

A collection of orders can be filtered by either article id it includes or by article manufacturer id.

The two parameters are mutually exclusive and cannot be used together. The Swagger API description for such a design should look as follows:

    paths:
      /books:
        get:
          description: Get the collection of books.
          operationId: getBooks
          parameters:
          - in: query
            name: filter[title]
            type: string
            description: Filter books by title.
          - in: query
            name: filter[price]
            type: number
            description: Filter books by price, in dollars. This may be a number or a
              range. Ranges can be **lowerValue..upperValue**, **lowerValue..** or **..upperValue**.
              The range tests are always inclusive of their endpoints.
          responses:
            200:
              description: An array of book objects.
              schema:
                type: array
                items:
                  $ref: "#/definitions/Book"
            400:
              $ref: "#/responses/ApiError"

## Alternative Design

When it would be beneficial (e.g. one of the filter queries is more common than another one) a separate resource for the particular query __SHOULD__ be provided. In such a case, the pivotal search parameter __MAY__ be in the form of a path variable.

### Example

Building on top of the example mentioned above, we will provide the filtering of orders by article id as a separate resource.

    paths:
      /articles/{article_id}/orders:
        get:
          summary: Retrieve the collection of Orders that contain given article.

## Errors

If the `filter` parameter specifies a field that doesn't exist, or does not have a string or numeric value, then the request will return a `400 Bad Request` response, and a body with an error message conforming to our [error response standard](Error_Responses.md).

## Example

    GET /employees?filter[foo]=bar

Response:

    400
    {
      "title": "The filtered field does not exist",
      "detail": "Employee resources do not have a field called 'foo.'"
    }

## More complex filters

An API __MAY__ implement a more complex filtering system with general predicates. APIs that implement more comprehensive filtering, the __SHOULD__ expose it at a `:search` method, using the [custom method](Custom_Methods.md) conventions. In that case, the search mechanism exposed can be tailored to the type of data being searched.

[The standard defined by Microsoft](https://github.com/Microsoft/api-guidelines/blob/vNext/Guidelines.md#97-filtering) __MAY__ be used as the model for these more complex filters expressions, but they should be exposed as a custom method, not using a `filter` or `$filter` query string parameter.

## Custom method Filter

The search parameters for an endpoint may exceed the URL length limit when representing it as query parameters.

To circumvent this limitation one can use the custom method Filter: **POST** with a `:filter` suffix on its url path.

All the search parameters will be under the body's field `filter`

Please note the body response has a slightly different structure to accomodate [pagination](Pagination.md#Pagination-via-POST) in a POST.

Example:

    paths:
      /books:filter:
        post:
          description: Get the filtered collection of books.
          operationId: getBooks
          parameters:
          - in: body
            name: filter
            type: object
            description: Filter books by criterias.
            properties:
              price:
                type: number
                description: Filter books by price, in dollars.
          - in: body
            name: cursorState
            type: string
            description: An opaque token representing the progress of the pagination. All other params such as ``filter`` and ``limit`` are ignored if this is supplied.
          - in: body
            name: limit
            type: number
            description: Max of books to return in a response
          responses:
            200:
              description: Filtered results.
              schema:
                type: object
                properties:
                  results:
                    type: array
                    items:
                      $ref: "#/definitions/Book"
                  pagination:
                    type: object
                    description: Pagination related information
                    properties:
                      nextPost:
                        type: object
                        description: All the info to POST to get the next set of results.
                        properties:
                          url:
                            type: string
                            description: the URL to POST to get the next set of results.
                          body:
                            type: object
                            description: |
                              The body to POST to get the set of results.
                              If cursor-based it will contain the fields: ``cursorToken``
                              If offset-based it will contains the fields: ``offset`` and ``limit``
            400:
              $ref: "#/responses/ApiError"
