# Sorting

Services **MAY** provide sorting support when querying the value of a collection resource. If the API supports sorting, it **MUST** provide options to sort on multiple field values, in ascending or descending order. The `sort` query parameter specifies a comma separated list of fields use to sort items. The second field is used to sort entries where the first field is equal. Extra spaces around the sorting field name are ignored.

Optionally, the field names **MUST** be follow by `asc` or `desc` to specify ascending or descending sort order. Without a direction specifier the items must be sorted in ascending order.

## Example

    GET /employees?sort=name asc

Will return a list of employees, sorted by name in ascending order.

    GET /employees?sort=name desc,hireDate asc

Will return a list of employees, sorted by name in descending order, and secondarily by the hire date in ascending order.

## Errors

If the `sort` parameter specifies a field that doesn't exist, or does not have a string or numeric value, then the request will return a `400 Bad Request` response, and a body with an error message conforming to our [error response standard](Error_Responses.md).

## Example

    GET /employees?sort=foo

Response:

    400
    {
      "title": "The sorting field does not exist",
      "detail": "Employee resources do not have a field called 'foo.'"
    }

