# API Standards - Current State and Issues

The APIs on the Forge platform do not follow any single standard. Each service provides an API appropriate for its own domain, but with little attention to consistency with other service APIs, and interoperability between APIs. A couple of the BIM360 services use the [JSON API](http://jsonapi.org/) standard, Design Automation uses the [OData standard](http://www.odata.org/), and most of the other APIs follow other internal standards, or no standard at all.

Teams at Autodesk have proposed and used several standards over the years, but no single standard has been used in all cases. Here are some the the internal and external standards.

### Internal Standards

+ [A360 Guidelines](https://wiki.autodesk.com/display/PRO/Rest+API+Guidelines)
+ [RESTful API style guide](https://wiki.autodesk.com/display/APIP/RESTful+API+style+guide)
+ [BIM360 API Design Guidelines](https://wiki.autodesk.com/display/COLLAB/API+Design+Guidelines)
+ [EA REST API guidelines ](https://wiki.autodesk.com/display/PDCA/API+design+and+the+proposed+interfaces)

### External Standards

+ [JSON API](http://jsonapi.org/)
+ [OData standard](http://www.odata.org/)

It is great that various teams used standards for their APIs, but it is time for Autodesk to start using a single standards for all external Forge APIs.


## Guiding Principles

+ An API is designed primarily for ease of human consumption.
+ Learning the API should be easier if you are already familiar with an existing Autodesk API.
+ APIs minor updates should always provide backward compatibility with previous versions. Major updates will provide a deprecation period where the old major version will still be supported.
+ Client SDKs should be provided for all APIs in all relevant languages.
+ Resource naming should always follow the same conventions.
+ Custom HTTP verbs should follow the same pattern.
+ Asynchronous operations should all follow the same pattern.
+ Search result pagination should follow the same pattern.
+ Authentication requirements should be clear and consistent across all APIs.

## Issues

The API Standards task force is collecting a list of issues that should be addressed by this standard.  These include:

+ Resource Structure
  + Links/Navigation/[HATEOAS](https://spring.io/understanding/HATEOAS)
  + Relationships
  + Caller context
+ Http Semantics
  + Request Methods
  + Headers
  + Custom Http verbs
+ Pagination
  + Known Page Size
  + Unknown Page Size
+ Versioning
  + Representing versions
  + Backward compatibility
+ Bulk Operations
+ Caching
  + Date based invalidation/Etag
  + File responses
+ Asynchronous Operations/Background jobs
  + Model async operations
  + Push Notifications
+ Resiliency
  + Implementation Challenges (Distributed Tx etc.)
  + Retries & Backoff Behavior
+ Optimization
  + Reduce response size
  + JSON API style “includes”
+ Internal details in response headers exposed to public
  + Derivative Service _x-ads-*_ headers
+ Standardization of authentication scopes
+ 4XX, 5XX response structure
+ Error codes and error messages
+ Link to documentation
+ Redirects upon 2 step process
+ Embeded resources (JSON API style “includes”)
+ Partial resources (specify the fields to return)
+ Who are the consumers of api? Human, service, machine. Which one are we optimized for?
+ Ease of use/comprehension
+ As a user (I can be a service or a human being), 
   + When I want to get an object give me the canonical form of the resource. 
   + Give me the additional collateral info only if I ask for it.  
   + Collaterals could be additional data including linkages to other entities, as well as (potentially) operations that I can perform on this entity base on my entitlement.   
   + Not all entities would have such requirement

## BIM360 Standard 
The BIM360 team has recently developed a standard which we would like to use as the starting point for the Forge API standard.
+ [BIM360 API Standard](BIM360_Standard.md)

A key part of this standard is to take an [API First](API_First.md) approach to service development. The [OpenAPI 2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md) standard (formerly known as Swagger) will be used to describe all interfaces prior to implementation. This enables a host of extremely useful functionality, including automatically generated client SDKs, automated mock services and automated generation of documentation.


