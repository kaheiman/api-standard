# API Versioning

> The fundamental principle is that you can’t break existing clients, because you don’t know what they implement, and you don’t control them.
>
> In doing so, you need to turn a backwards-incompatible change into a compatible one.
>
> – [Mark Nottingham](https://www.mnot.net/blog/2011/10/25/web_api_versioning_smackdown)

Any modification to an existing API **MUST** maintain backward compatibility. Any change that does not maintain backward compatibility must use a new major version for that API, while maintaining support for the old version for a given period of time.

## Parameters vs. Responses

The requirement that existing client code must not break implies different rules for parameters and fields in responses. Often the rules for parameters are the converse of the rules for response fields. _e.g._ You cannot remove optional parameters, but you can remove optional fields in a response. In the following rules _parameters_ refer to query parameters, headers and fields in request bodies for POST, PUT and PATCH.

## Backward-compatible (non-breaking) changes

You **MAY** make any of the following changes, and the API will remain backward-compatible.

1. Add new resources.
1. Add HTTP methods to existing resources.
1. Add optional parameters.
1. Change parameters from required to optional.
1. Change response fields from optional to required.
1. Add new fields to a response.
1. Add values to existing enums used _only_ as parameters.
1. Remove values from an existing enums used _only_ as fields in a response.

## Non-backward-compatible (breaking) changes

To preserve backward compatibility, you **MUST NOT** make any of the following changes:

1. Remove existing resources.
1. Remove HTTP methods handled by a existing resources.
1. Add required parameters
1. Make optional parameters required.
1. Remove parameters.
1. Make required response fields optional.
1. Remove required response fields.
1. Make a non-nullable response fields nullable.
1. Remove a value from existing enums used in parameters.
1. Add a new value to existing enums used in a responses.
1. Change the type of a field or parameter. _e.g._ Changing a field for a single user to contain a list of users.
1. Rename a field. (see section below on renaming fields in a compatible way)
1. Add a new response code to an existing resource.
1. Change processing rules. The semantics of the API cannot change for existing clients.

Any changes not explicitly listed in the sections above need to be evaluated for backward-compatibility.

## Changing fields names

It is not recommended to change field names because it can lead to confusion. However, if you do need to do that, and still maintain backward compatibility, you can follow these rules, and support both the old and new names. The old name should be marked as deprecated in the documentation.

1. The old name **MUST** still be supported in requests and responses.
1. The new name **MUST** must be supported in both requests and responses.
1. The old name **MUST NOT** be re-used for a different purpose.

## URI-based versioning

The major version of a resource must include a version identifier in the path.

Resource URL before a breaking change:

    https://developer.api.autodesk.com/data/v1/banners

Resource URL after a breaking change:

    https://developer.api.autodesk.com/data/v2/banners

The resource version path should only include the major version number, like `/v1` or `/v3`. If your system uses semantic versioning, then the minor and patch version numbers __MUST NOT__ be included in the version path.

The old version __MUST__ remain available for a specified period of time. The exact period depends on business and technical decisions.

All new resources __MUST__ use a `/v1` (or the most recent [Global Version](API_Versioning.md#global-vs-per-resource-versioning))  version identifier in its path, following the service name, and preceding all other parts of the path.

## Backward-incompatible changes

A change to a resource identifier, resource metadata, resource actions, resource relations or representation format that can't follow the rules above **MUST** result in a **new resource version**. Existing resource versions **MUST** be preserved.

### Example

Currently, an optional URI Query Parameter on an existing resource `/v1/greeting?first=John&last=Appleseed` needs to be made required. Since this change violates the 3rd rule of extending and could break existing clients a new version of the resource is created with different URI `/v2/greeting?first=John&last=Appleseed`.

## Global vs Per-resource versioning

There are two styles of URI versioning for an API: global or per-resource.

+ Global URI Versioning - Every resource is made available under the new version, even if the method didn't change.
+ Per-Resource Versioning - Only the URI of resources that changed get a new version in the URI.

An API __MUST__ be versioned either globally or per resource when making a major version change to any of its resources. Global versioning is preferred, unless implementation conditions prevent it. An API __MUST NOT__ use other methods of versioning, such as query string parameters, custom headers or content negotiation.

## API Versions for Changelogs

When an API or service is updated, typically the changes are are published publicly to let people know what changed. Changelogs should identify versions using dates

A service __MAY__ choose to use semantic version numbers internally, but the changes should be documented to clients publicly using dates, not the semantic version number.

## Recommended Reading

+ [Google's protocol buffer versioning guidelines](https://cloud.google.com/apis/design/compatibility)
+ [API Versioning Has No "Right Way"](https://blog.apisyouwonthate.com/api-versioning-has-no-right-way-f3c75457c0b7)
