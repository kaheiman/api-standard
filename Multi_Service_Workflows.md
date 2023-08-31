# Multi-service Workflows

Getting the services to work together can be difficult in the Forge platform. 

## Inconsistent APIs.

As mentioned in the [API Standards](API_Standards.md) document, the public Forge APIs do not follow a single standard, so developers cannot leverage their knowledge of one API to learn another. The resource structure, parameters, responses and status codes can all be handled in different ways. Adopting a standard for all APIs, and implementing that standard for existing and new services will go a long way towards alleviating this problem.

## Different authentication requirements & scope

The is probably fixed by better documentation of the exact [authentication](Authentication.md) requirements for each API. This problem can also be addressed with good authentication support in our client SDKs.

## Storage service mismatch

The Forge services are not consistent with each other regarding storage locations of data. For example, the Reality Capture service creates its output in Amazon S3, and returns a signed URL to the client to access the file. The Model Derivative service takes its input from an OSS item in a bucket. To translate the result returned by the RC service, clients must download it from S3 and upload it to OSS. The adds extra complexity to the developer, reduced performance for the user, and extra cost to Autodesk for having to store the data twice.

Different services have different storage requirements, so the best way to handle this issue it to have SDK utilities that make it easy to copy data between different services.

## Different data models for similar services

BIM360 Docs and Fusion Team have very similar, but different data models. 
![](BIM360.png)


![](FusionTeam.png)

This is not explained well in the documentation of the services used to access this data. For example, what is the relationship between Data Management projects and BIM360 HQ projects? 

This is a problem that can be solved with better documentation and tutorials.

## Different parameter encoding standards

Different APIs require the same data encoded in different ways. For example, the model derivative service takes an OSS URN for the location of the input data. The docs say:
>>First, you need to encode the source URN (objectId) that you retrieved when calling the PUT 
>> buckets/{bucket_key}/objects/{object_name} endpoint) to Base64 format.
>>
>> We recommend using the unpadded option (RFC 6920), since it uses URL safe alphabets.

Is that a requirement, or just a good idea? Good luck finding a tool that generates RFC 6920 compatible encodings. The derivative service sample application does not use the unpadded version. 

The Data Management APIs do not store the URN in this form, so it is up to the developer to figure out how to do this encoding.


