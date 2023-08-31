# Resource Oriented Design

The architectural style of REST was first introduced in 2000, primarily designed to work well with HTTP/1.1. Its core principle is to define named resources that can be manipulated using a small number of methods. The resources and methods are known as nouns and verbs of APIs. With the HTTP protocol, the resource names naturally map to URLs, and methods naturally map to HTTP methods POST, GET, PUT, PATCH, and DELETE.

On the Internet, HTTP REST APIs have been recently hugely successful. In 2017, about 83% of public network APIs were HTTP REST APIs.

## What is a REST API?

A REST API is modeled as collections of individually-addressable resources (the nouns of the API). Resources are referenced with their resource names and manipulated via a small set of methods (also known as verbs or operations).

[Standard methods](Standard_Methods.md) for REST APIs (also known as REST methods) are List, Get, Create, Update, and Delete. [Custom methods](Custom_Methods.md) (also known as custom verbs or custom operations) are also available to API designers for functionality that doesn't easily map to one of the standard methods, such as database transactions.

NOTE: Custom verbs does not mean creating custom HTTP verbs to support custom methods. For HTTP-based APIs, they simply map to the most suitable HTTP verbs.

## Design flow

The Design Guide suggests taking the following steps when designing resource-oriented APIs (more details are covered in specific sections below):

- Determine what types of resources an API provides.
- Determine the relationships between resources.
- Decide the resource name schemes based on types and relationships.
- Decide the resource schemas.
- Attach a minimum set of methods to resources.

## Resources

A resource-oriented API is generally modeled as a resource hierarchy, where each node is either a simple resource or a collection resource. For convenience, they are often called as a resource and a collection, respectively.

- A [collection resource](Collection_Resources.md) contains a list of resources __of the same type__. For example, a user has a collection of contacts. Collection resources __MUST__ be given plural names, unless a plural is not available (like _furniture_).
- A resource has some state and zero or more sub-resources. Each sub-resource can be either a simple resource or a collection resource.

For example, Gmail API has a collection of users, each user has a collection of messages, a collection of threads, a collection of labels, a profile resource, and several setting resources.

While there is some conceptual alignment between storage systems and REST APIs, a service with a resource-oriented API is not necessarily a database and has enormous flexibility in how it interprets resources and methods. For example, creating a calendar event (resource) may create additional events for attendees, send email invitations to attendees, reserve conference rooms, and update video conference schedules.

## Methods

The key characteristic of a resource-oriented API is that it emphasizes resources (data model) over the methods performed on the resources (functionality). A typical resource-oriented API exposes a large number of resources with a small number of methods. The methods can be either the standard methods or custom methods. For this guide, the standard methods are: List, Get, Create, Update, and Delete.

Where API functionality naturally maps to one of the standard methods, that method __SHOULD__ be used in the API design. For functionality that does not naturally map to one of the standard methods, custom methods __MAY__ be used. Custom methods offer the same design freedom as traditional RPC APIs, which can be used to implement common programming patterns, such as database transactions or data analysis.

## Examples

The following sections present a few real-world examples on how to apply resource-oriented API design to large-scale services.

### Gmail API

The Gmail API service implements the Gmail API and exposes most of Gmail functionality. It has the following resource model:

#### API service: gmail.googleapis.com

- A collection of users: `users`. Each user has the following resources.
  - A collection of messages: `users/{userId}/messages`.
  - A collection of threads: `users/{userId}/threads`.
  - A collection of labels: `users/{userId}/labels`.
  - A collection of change history: `users/{userId}/history`.
  - A resource representing the user profile: `users/{userId}/profile`.
  - A resource representing user settings: `users/{userId}/settings`.
