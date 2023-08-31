# Partial Fields

The API **MAY** support fetching a subset of the fields in objects. The client specifies with fields he wants to get back in query parameters. For example when a client does a __GET__ on a resource, with the query string `projects/{projectId}?fields=id,name` The response will include the `id`, and `name` fields of the resource.

This also works for collection resources with include sub-resource. See [sub-resources](SubResources.md) for details.

APIs which support partial fields **MUST** use the `?fields=` query string to support getting partial resources.

## Errors

If the `fields` parameter specifies a field that doesn't exist, or does not have a string or numeric value, then the request will return a `400 Bad Request` response, and a body with an error message conforming to our [error response standard](Error_Responses.md).

## Example

    GET /employees?fields=foo

Response:

    400
    {
      "title": "The specified field field does not exist",
      "detail": "Employee resources do not have a field called 'foo.'"
    }
