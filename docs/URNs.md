# URNs

A service **SHOULD** support [URN](https://en.wikipedia.org/wiki/Uniform_Resource_Name) names for its resources, in addition to the usual [resource names](Resource_Names.md).

URNs **MUST** use meaningful namespace identifiers and **MUST** be independently resolvable. URNs **MUST** be persistent and stable under operations such as moving locations. Reference: [UDP URNs](https://wiki.autodesk.com/display/CPG/URN+Reference)

## REST API Support

Any service that supports URNs **MUST** provide a way for clients to retrieve the URN for a resource, and **SHOULD** provide a way to resolve a URN to a resource path.

How the URN is provided is up to the service. It may include the URN by default in the response body when creating a resource, or getting the data for the resource.  

For example a 
> GET /customers/1234 

Response:

     {
       name: "Some Customer",
       other_properties...
       urn: "urn:adsk.someservice:us.prd:customer:1234"
    }

Or the API can provide a specific resource path the get the URN
> GET /customers/1234/urn

Response:

    {
       urn: "urn:adsk.someservice:us.prd:customer:1234"
    }

The service **SHOULD** also provide an API to resolve a URN into a resource path.

> GET /entities?urn=urn:adsk.someservice:us.prd:customer:1234

Response:

    {
      path: "customers/1235"
    }

Services **MAY** provide a way to specify resource with a URN in a request body. This example shows assigning a policy to a resource.

> POST /assignments

      {
         policy: "urn:adsk:...:policy:78787239",
         resources: ["urn:adsk.oss:us.prd:os.object:508eab49-34e7-4390-986d-a44beb0e6afe.dwfx"]
      }

** URN Format

All Autodesk URNs **SHOULD** use the following naming convention:

    urn:adsk.[service name]:[environment]:[resource type]:[service-specific unique string]

- The *service name* is a short string describing the service, such as `udp`, `oss` or `hfdm`.
- *environment* - `Region.Server`
  - Region - `us` | `emea` or other valid region identifier.
  - Server - `prd` | `stg` | `dev` | `stable` or any other. It can be any set of characters representing a separate cluster.
- *Resource type* - A short string describing the service-specific resource type.
  - Examples include `space` and `asset` for UDP. `fs.folder` and `fs.file` for WIP data.
- *Service-specific unique string* - This is any unique string that the service can use to resolve the asset. Typically it will be a combination of one or more UUID strings.
    - For UDP it will look like `ace_ce_identifier_guid.space_branch_GUID.resource_GUID`
    - For an OSS object it will look like `bucketId.objectId`
  
## Examples

| Object | Example | 
|-------------|-----------|
|UDP Space | `urn:adsk.udp:emea.stg:space:656f4ea3-a6fb-4071-9fb9-404ad9e7ca9c.8904a87c-f783-4b42-928e-bda49e01d6f0` |
|UDP Asset | `urn:adsk.udp:us.dev:asset:656f4ea3-a6fb-4071-9fb9-404ad9e7ca9c.8904a87c-f783-4b42-928e-bda49e01d6f0.29660168-9d24-487c-82be-272c92c69b68` |
| HFDM repository | `urn:adsk.hfdm:us.prd:repository:656f4ea3-a6fb-4071-9fb9-404ad9e7ca9c` |
| HFDM commit | `urn:adsk.hfdm:emea.stg:commit:5148aec1-9ae9-4864-b18e-9a37859beaf4.dfa8a18-56b5-4d13-a204-9f713a6bb398` |
| OSS object | `urn:adsk.oss:us.prd:fs.file:508eab49-34e7-4390-986d-a44beb0e6afe.dwfx` |

## Legacy Services

Some older Autodesk services, such as OSS and WIP, will continue to support older style URNs that do not follow these conventions. All new services **MUST** support the new format, if they support URNs.

