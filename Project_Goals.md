# API Rationalization Project Goals

The overall goal of this project is to make it easier for customers to build reliable applications in the Forge ecosystem.

At the Fall 2017 Autodesk Architects' Forum, Tim Wiegand gave a [presentation](https://1drv.ms/p/s!AkbD7E-oLaMyhDfqDxzyXqNKbnl6) on the Forge platform, and difficulties using it.

## Pain Points

Tim's presentation contained a list of developer pain points. This project should identify the most important pain points and propose a plan to address these in priority order.

+ It’s like the APIs were written by different companies.
+ Different subsets of features available for 2LO vs 3LO, and are not well documented.
+ Different terminology for the same thing.
+ Multiple ways of implementing the same thing.
+ Friction / Impedance mismatches between services.
+ Internal architectural details leaking out (x-ads-acm-*).
+ Inconsistent behavior between API and product UI.
+ Documentation that is economical with the truth.
+ Lack of investment in SDKs.

These problems can be categorized into five major areas.

+ [API Standards](API_Standards.md)
+ [Documentation](Documentation.md)
+ [Client SDKs](Client_SDK.md)
+ [Authentication](Authentication.md)
+ [Multi-service Workflows](Multi_Service_Workflows.md)

## Prioritization

From a very high level, a list of priorities for this project will include.
1. Create a new API standard.
1. New services use the standards.
1. Replace Apigee configuration with Swagger file + custom tags.
1. API Documentation improvements. API reference generated from Swagger.
1. [Resiliency SDKs](https://wiki.autodesk.com/display/KEVLAR/Resiliency+SDK+architecture+-+bearhug) for Forge services, offered in the major languages.
1. Existing services adopt the standard as they are able.
1. Address important issues in multi-service workflows.

These items obviously cover a lot of ground, and we need to narrow the scope of each item to address the most important parts of each priority, and come up with an achievable plan for this project. We don't want to boil the ocean. 
