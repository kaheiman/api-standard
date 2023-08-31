# Core Principles

## OpenAPI Specification

Every HTTP/1.1 REST API **MUST** be described using an API description format.

The API description format used **MUST** be the OpenAPI Specification (formerly known as Swagger). Current APIs **MUST** use [OpenAPI 2](https://swagger.io/specification/v2/) or [OpenAPI 3](https://swagger.io/specification/). OpenAPI 3 is recommended.

Every API description **MUST** be published in the API design platform.

### Autodesk OpenAPI Tools

+ [Documentation Pipeline](https://wiki.autodesk.com/display/FCPA/Forge+Documentation+Pipeline+Onboarding) - Documentation generation for the Forge Developer Portal.
+ [Resiliency SDK](https://git.autodesk.com/cloudplatform-apim/forge-rsdk-codegen) - Generates client SDKs from a Swagger file in multiple languages with resiliency patterns built in.

## Language

The API description **MUST** be written in American English.

## API Design Platform

[Stoplight](https://platform.stoplight.autodesk.com) is our primary platform supporting the API first workflow. Stoplight **SHOULD** be used during API design.

Every API description **MUST** be stored in a git repository. Stoplight or another API design platform **MAY** be used to edit the file, but it should be stored permanently in a repository.

The Swagger files **MUST** be the single source of truth to learn about existing APIs within the organization. The Swagger file **MUST** contain documentation for each API which will be used to generate API reference documentation.

NOTE: Stoplight supports API-first approach in multiple ways: They validate API description for correctness and automatically generate API documentation to drive the discussion between stakeholders. (No more emails with API description flying between stakeholders)

Autodesk-managed service is available at [platform.stoplight.autodesk.com](https://platform.stoplight.autodesk.com) and it is accessible from all of the company's networks, including VPN. Because it is deployed behind the corporate firewall, it can access [git.autodesk.com](https://git.autodesk.com), which allows for data synchronization between the two systems. More detailed information about the service is available on the service [main page](https://platform.stoplight.autodesk.com).

## Contract

Approved API Design, represented by its API Description, **MUST** represent the contract between API stakeholder, implementors, and consumers.

Any change to an API **MUST** be accompanied by a related update to the contract (API Description).

## Contract Validation

Every API description (contract) using HTTP(S) protocol **MUST** be tested against its API implementation.

In addition to local runs, the tests **MUST** be an integral part the API implementation's CI/CD pipeline.

The CI/CD pipeline **MUST** be configured to run the test after every change of the API implementation.

## Design Maturity

### API Design Maturity

> How to design an API

When designing an API an [API First](API_First.md) process **SHOULD** be followed. This practice surfaces design problems ealier in the lifecycle when they are easier to correct.

Every API design **MUST** be resource-centric ([Web API Design Maturity Model Level 2](http://amundsen.com/talks/2016-11-apistrat-wadm/2016-11-apistrat-wadm.pdf)). That is an API design **MUST** revolve around Web-styled resources, relations between the resources and the actions the resources may afford.

An API **MAY** choose to support Web Maturity Model Level 3 ([HATEOAS](HATEOAS.md)) if appropriate for the service.

### API Design Implementation Maturity

> How to implement the API design

Every API design implementation using the HTTP 1.1 protocol **MUST** use the appropriate HTTP Request Method ([Richardson Maturity Model Level 2](https://martinfowler.com/articles/richardsonMaturityModel.html#level2)) to implement an action afforded by a resource.

## Robustness

Every API implementation and API consumer **MUST** follow Postel's law:

> Be conservative in what you send, be liberal in what you accept.
>
>– John Postel

That is, send the necessary minimum and be tolerant as possible while consuming another service ([tolerant reader](https://martinfowler.com/bliki/TolerantReader.html)).

## Minimal API Surface

Every API design **MUST** aim for a minimal API surface without sacrificing on product requirements.

API design **SHOULD NOT** include unnecessary resources, relations, actions or data.

API design **SHOULD NOT** add functionality until deemed necessary ([YAGNI principle](https://martinfowler.com/bliki/Yagni.html)).

## Rules for Extending

Any modification to an existing API **MUST** avoid breaking changes and **MUST** maintain backward compatibility.

In particular, any change to an API **MUST** follow the following Rules for Extending:

You **MUST NOT** take anything away (related: Minimal Surface Principle, Robustness Principle)
You **MUST NOT** change processing rules
You **MUST NOT** make optional things required
Anything you add **MUST** be optional (related Robustness Principle)

See [API Versioning](Version_Versioning.md) for more details.

NOTE: These rules cover also renaming and change to identifiers (URIs). Names and identifiers should be stable over the time including their semantics.

## Loose Coupling

In addition to the robustness principle, API consumers (clients) **MUST** operate independently of API implementation's internals.

Similarly, the API consumers **MUST NOT** assume or rely on any knowledge of the API service internal implementation.
