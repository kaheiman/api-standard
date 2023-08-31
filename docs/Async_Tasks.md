# Asynchronous Tasks

Services **SHOULD** try to respond to client requests as quickly as possible. In situations where a client's request requires long-running work, services **SHOULD** handle such work as asynchronous and respond to the client that the work has been accepted and will be processed.

## Using Task-Tracking Resources

A task-tracking resource **MAY** be used to allow clients to request long-running tasks and monitor those tasks over time. A service **MUST** document how this pattern is used along with all states for a given task so that clients can be designed to expect all possible states. The client and service flow works as follows:

1. The client sends a request that is meant to be processed asynchronously. For example, the client might send a POST request to a `/companies` URL to create a company.
1. The service responds with status code **202 Accepted** and includes a `Location` header to the task-tracking resource. The service **MUST** include the status of task in the response body. In this example, the `Location` header might include `/tasks/b557836a-9c5b-44e6-8d62-8a08553f61f8` as a URL for the task and the `status` might be `pending`.
1. The client monitors the task by using the URL provided in the `Location` header. The service **MUST** respond with **200 OK**. The service **MUST** also include the state of the task in the response body along with any relevant data about the task.
1. If the task fails, the service **MUST** respond to the task-tracking URL with **200 OK**, provide a failed status, and problem details.
1. If the task completes, the service **MUST** respond with **303 See Other** and the `Location` header set to the resource for which the task is being done.

## Using Domain-Specific Resources

A service **MAY** allow a resource to communicate its own state instead of using a separate resource to handle long-running work. Clients may monitor the resource directly instead of a separate task-tracking resource. This is a useful pattern for not only creating new resources but also changing the state of an existing resource that may take time.

For example, when creating a new resource that takes time to process, a service **MAY** respond with a successful status code and a status in the response body with something like `pending`. Clients may watch the state of the resource until it completes the creation process. The same pattern can be used for other requests, such as DELETE, where the server responds with a success status code and shows the resource in a `deleting` state.

When following this pattern, it's important to remember that assets described by resources in these states may not be available until the underlying task completes. Services **SHOULD** provide ways for clients to filter collections for states in which they are interested. Like with task-tracking resources, the service **MUST** document all of the states used for a given resource.

## Using Callbacks

Callbacks, or in some cases called webhooks, **MAY** be used to allow the client to register for various events in long-running asynchronous tasks. This pattern is useful when a service needs to update the client on several events throughout lifecycle of one or many resources over a longer period of time. This design requires a client API to be designed and developed, which is a larger burden on the client, so it should only be used in these use cases like mentioned here. If used, the client API **MUST** be subject to these API guidelines.
