# Client SDKs

The Forge platform currently supports several client SDKs to help interacting with the Forge REST APIs. The SDKs are somewhat difficult to locate on the Forge site [this page](https://developer.api.autodesk.com/en/docs/quickstarts/v1/overview/) mentions Node.js, Ruby, C# and VB.NET SDKs, and [this page](https://autodesk-forge.github.io/?SDK) mentions Node.js, .NET and Java. Ths Forge SDK page mentions PHP, Android and IOS SDKs, but does not provide any links to them. I found no place that lists _all_ SDKs in a single place. It is unclear from the documentation whether the SDKs provide 100% coverage of all APIs. 

The page also mentions generating SDKs from the Swagger definition of the APIs, but the Swagger links are broken.

## Resiliency SDKs

Tommy Li is working on a new project to generate [Resiliency SDKs](https://wiki.autodesk.com/display/KEVLAR/Resiliency+SDK+architecture+-+bearhug) for all of our Forge APIs. These SDKs are automatically generated from Swagger API definitions, and use the best practices for calling Forge APIs. For example, a client should back off and retry gracefully if a Forge api is returning 500.

In theory many different language SDKs can be generated using this system. __Note__: Tommy, what is the status of this project?

## Documentation

It would also help in the use of these SDK if the documentation for all the APIs also give SDK examples, in addition to curl.
