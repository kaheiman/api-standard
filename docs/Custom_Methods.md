# Custom Methods

This chapter will discuss how to use custom methods for API designs.

Custom methods refer to API methods besides the 5 standard methods. They **SHOULD** only be used for functionality that cannot be easily expressed via standard methods. In general, API designers **SHOULD** choose standard methods over custom methods whenever feasible. Standard Methods have simpler and well-defined semantics that most developers are familiar with, so they are easier to use and less error-prone. Another advantage of standard methods is the API platform has better understanding and support for standard methods, such as billing, error handling, logging, monitoring.

A custom method can be associated with a resource, a collection, or a service. It **MAY** take an arbitrary request and return an arbitrary response, and also supports streaming request and response.

Custom method names **MUST** follow method [naming conventions](Naming_Conventions.md).

## HTTP mapping

For custom methods, they **SHOULD** use the following generic HTTP mapping:

    https://service.name/v1/<some-resource-name>:custom-verb

The reason to use ':' instead of '/' to separate the custom verb from the resource name is to support arbitrary paths. For example, undelete a file can map to POST /files/a/long/file/{name}:undelete

The following guidelines SHALL be applied when choosing the HTTP mapping:

+ Custom methods **SHOULD** use HTTP POST verb since it has the most flexible semantics, except for methods serving as an alternative get or list which **MAY** use GET when possible. (See the third bullet for specifics.)
+ Custom methods **SHOULD NOT** use HTTP PATCH, but **MAY** use other HTTP verbs. In such cases, the methods **MUST** follow the standard HTTP semantics for that verb.
+ Notably, custom methods using HTTP GET **MUST** be idempotent and have no side effects. For example, custom methods that implement special views on the resource **SHOULD** use HTTP GET.

### Example

    paths:
      '/banners:search':
        get:
          parameters:
            - name: term
              required: true
              description: The banner UUID identifier
              type: string
              in: query
          responses:
            200:
              description: Success
              schema:
                type: array
                schema:
                   $ref: '#/definitions/Banner'

## Use Cases

Some additional scenarios where custom methods may be the right choice:

+ **Reboot a virtual machine.** The design alternatives could be "create a reboot resource in a collection of reboots" which feels disproportionately complex, or "virtual machine has a mutable state which the client can update from RUNNING to RESTARTING" which would open questions as to which other state transitions are possible. Moreover, reboot is a well-known concept that can translate well to a custom method which intuitively meets developer expectations.
+ **Send mail.** Creating an email message should not necessarily send it (draft). Compared to the design alternative (move a message to an "Outbox" collection) custom method has the advantage of being more discoverable by the API user and models the concept more directly.
+ **Promote an employee.** If implemented as a standard update, the client would have to replicate the corporate policies governing the promotion process to ensure the promotion happens to the correct level, within the same career ladder etc.
+ **Batch methods.** For performance critical methods, it **MAY** be useful to provide custom batch methods to reduce per-request overhead. For example, [accounts.locations.batchGet](https://developers.google.com/my-business/reference/rest/v4/accounts.locations/batchGet).

A few examples where a standard method is a better fit than a custom method:

+ Query resources with different query parameters (use standard list method with standard list filtering).
+ Simple resource property change (use standard update method with field mask).
+ Dismiss a notification (use standard delete method).
+ When a batch operation needs to have different semantics from multiple single requests. For example, setting multiple HFDM properties in a single call, which does a single commit for all properties.

## Common Custom Methods

The curated list of commonly used or useful custom method names is below. API designers **SHOULD** consider these names before introducing their own to facilitate consistency across APIs.

|Method Name | Custom verb | HTTP verb | Note |
|----------|----------|--------|----|
|Cancel| :cancel| POST| Cancel an outstanding operation (build, computation etc.)|
|BatchGet\<plural noun\>| :batch-get| GET| Batch get of multiple resources. (See details in the description of List)|
|Move| :move| POST| Move a resource from one parent to another.|
|Search| :search| GET| Alternative to List for fetching data that does not adhere to List semantics.|
|Undelete| :undelete| POST| Restore a resource that was previously deleted. The recommended retention period is 30-day.|
|Filter| :filter | POST | Alternative to GET filtering to circumvent URL length limit. (See details in [Filtering](Filtering.md#Custom-method-Filter)) |
