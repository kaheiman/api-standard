# Resource Names

In resource-oriented APIs, *resources* are named entities, and *resource names* are their identifiers. Each resource **MUST** have its own unique resource name. The resource name is made up of the ID of the resource itself, the IDs of any parent resources, and its API service name. We'll look at resource IDs and how a resource name is constructed below.

A *collection* is a special kind of resource that contains a list of sub-resources of identical type. For example, a directory is a collection of file resources. The resource ID for a collection is called collection ID.

The resource name is organized hierarchically using collection IDs and resource IDs, separated by forward slashes. If a resource contains a sub-resource, the sub-resource's name is formed by specifying the parent resource name followed by the sub-resource's ID - again, separated by forward slashes.

## Example 1: A storage service has a collection of buckets, where each bucket has a collection of objects

| API Service Name| Collection ID | Resource ID| Collection ID | Resource ID |
|-------------|-----------|---------|-----------|----------|
|storage.googleapis.com | `/buckets` | `/{bucket-id}` | `/objects` | `/{object-id}` |

## Example 2: An email service has a collection of users

Each user has a settings sub-resource, and the settings sub-resource has a number of other sub-resources, including customFrom:

| API Service Name| Collection ID | Resource ID| Collection ID | Resource ID |
|-------------|-----------|---------|-----------|----------|
| mail.googleapis.com | `/users` | `/name@example.com` | `/settings` | `/customFrom` |

An API producer can choose any acceptable value for resource and collection IDs as long as they are unique within the resource hierarchy. You can find more guidelines for choosing appropriate resource and collection IDs below.

By splitting the resource name, such as `name.split("/")[n]`, one can obtain the individual collection IDs and resource IDs, assuming none of the segments contains any forward slash.

## Choosing Resource Name

Resource names are subject to the [Naming Conventions](Naming_Conventions.md)

## API Prefix

All Autodesk APIs on the __<https://developer.api.autodesk.com>__ domain __MUST__ choose an appropriate prefix for resource names. APIs hosted on other domains __SHOULD__ use a prefix, but it may be incorporated in the domain name. The prefix form depends on what type of API it is. The prefix is typically the path in front of the specific resource name, and includes information such as the category and name of the service. Abbreviations __SHOULD NOT__ be used in API prefixes.

The API prefix __MUST__ have a version number in it. The [API Versioning](API_Versioning.md) page has more details.

### Prefix placement

The prefix is usually placed in the resource path in front of the specific resource name but it can be part of the API service domain name. Services defining their own domain can put part of the prefix in the domain name:

    https://developer.api.autodesk.com/sales/pricing/v1/items
    https://sales.api.autodesk.com/pricing/v1/items
    https://enterprise.api.autodesk.com/order/v3/orders

### Core APIs

Core APIs, consist of core Forge services, such as authentication and Forge Data Management, should follow this pattern:

    /core/<category>/<service>/<version>/<resource path>

Examples:

    /core/data/oss/v2/buckets
    /core/data/asset-graph/v1/collections

### Customer Data APIs

Customer Data APIs consist of APIs for particular products, like BIM360. These APIs should use the following template:

    <industry>/<service>/<version>/<resource path>

Examples:

    /construction/issues/v2/projects/{projectId}/issues/{issueId}/attachments
    /production/components/v3/entities

### Enterprise/Business APIs

Enterprise/Business APIs consists of B2B or B2C APIs for various domains like subscriptions, orders, channel partners, etc. These should use the following template:

    <domain>/<version>/<resource path>

Examples:

    order/v3/order-details/{id}
    sales/v2/offerings/{id}
    account/v2/accounts/{csn}

### Generic Pattern

APIs not covered by the sections above should still use an API prefix. This is the suggested form, but others could be considered, as appropriate.

    <category>/<service>/<version>/<resource path>

Example:

    /sales/pricing/v1/items
