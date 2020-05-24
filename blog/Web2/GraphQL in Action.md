---
title: 'GraphQL in Action'
---

# GraphQL in Action

## Introduction to GraphQL

### What is GraphQL

#### The Big picture

- GraphQL allows clients to ask for the exact data they need and make it easier for servers to aggregate data from multiple data storage resources.
- Type system ensure that the clients ask for only what is possible and provide clear and helpful errors. Clients can use types to minimize any manual parsing of data elemetns.

#### GraphQL is a specification

[GraphQL Spec](http://spec.graphql.org/)

- The syntax between languages may be different, but the rules and practices remain the same.
- You can ultimately learn EVERYTHING about the GraphQL language and runtime requirements in that official specification document.
- Alongside the specification document, Facebook alse released a reference implementation libary for GraphQL runtimes in JS, which is hosted at https://github.com/graphql/graphql-js.

#### GraphQL is a language

- Queries represent READ operation. Mutations represent WRITE-then-READ operations. You can simply think of mutations as "queries that have side effects".
- GraphQL also supports a third request type which is called a subscription and it's used for real-time data monitoring requests.
- GraphQL is a declarative language. It is structured language that computers can easily parse and use.

#### GraphQL is a service

- A GraphQL service can be written in any programming language and it can be conceptually split into two major parts: structure and behavior.
  - The structure is defined with a strongly-type shcema. The **schema** is basically a graphql of **fields** which have **types**.
  - The behavior is naturally implemented with functions that in the GraphQL world are named **resolve functions**.
- A resolver function is where we give instructions for the runtime service about how and where to access the raw data.

- GraphQL service traverses the tree of fields in the request and invokes the resolver function associated with each field in it. It'll then gather the data returned by these resolver fucntions and use it to form a single response.

### Why GraphQL

If you ask me to answer the "Why GraphQL" question with just a single word, that word would be : **Standards**.

GraphQL provides standards and structures to implement API features in maintainable and scalable ways whil the other alternatives lack such standards.

GraphQL documentation is built-in and it's first class.

It basically decouples clients from servers and allows both of them to evelve and scale independently. This enable much faster iteration in both frontend and backend products.

With GraphQL, you can basically shift this multi-request complexity to the backend and have your GraphQL runtime deal with it.

It improves frontend DX. There is a tight relationship between what data is needed by a UI and the way a developer can express a description of that data need in GraphQL.

#### What about REST APIs?

The biggest "relevant" problem with REST APIs here is the clients' need to communicate with multiple data API endpoints. Another big problem is, especially for mobile applications, mobile devices usually have processing, memory, and network constraints.

In a pure REST API a client cannot specify which fields to select for a record in that resource, which is also known as **over-fetching**.

One other big problem with REST APIs is versioning. If you need to support multiple versions that usually means new endpoints. This leads to more problem while using and maintaing these endpoints and it might be the cause of code duplication on the server.

Caching a REST API response is a lot easier than caching a GraphQL API response. 

Optimizing the code for a REST endpoint is potentially a lot easier than optimizing the code for a generic single endpoint.

#### The GraphQL Way

The concepts and design decisions behind GraphQL:

1. The Typed Graph Schema
2. The Declarative Language
3. The Single Endpoint and the Client Language
   - Having clients asking for exactly what they need enables backend developers to have more useful analytics of what data is being used and what parts of the data is in higher demand. This is very useful data, which can be used to scale and optimize the data services based on usage patterns.
4. The Simple Versioning
   - This is especially important for mobile clients because you cannot control the version of the API they are using.

#### REST APIs and GraphQL APIs in action

### GraphQL Problems

#### Security

One important threat that GraphQL makes easier is resource exhaustion attacks (AKA Denial of Service attacks). It is very simple to query for deep nested relationships or use field aliases to ask for the same field many times.

You can implement cost analysis on the query in advance and enforce some kind of limits on the amount of data one can consume. You can also implement a time-out to kill requests that take too long to resolve. Also, since a GraphQL service is just one layer in any application stack, you can handle the rate limits enforcement at a lower level under GraphQL.

Authentication and authorization are other concerns that you need to think about when working with GraphQL.

#### Caching and Optimizing

With GraphQL, you can adopt a similar basic approach and use the query text as a key to cache its response. But this approach is limited, not very efficient, and can cause problems with data consistency. The results of multiple GraphQL queries can easily overlap and this basic caching approach would not account for the overlap.

There is a brilliant solution to this problem. A Graph Query means a *Graph Cache*. If you normalize a GraphQL query response into a flat collection of records and give each record a global unique ID, you can cache those records instead of caching the full responses.

One of the other most "famous" problems that you would encounter when working with GraphQL is the problem that is commonly referred to as N+1 SQL queries. 

Luckily, Facebook is pioneering one possible solution to both the caching problem and the data-loading-optimization problem. It’s called DataLoader. DataLoader uses a combination of batching and caching to accomplish that.

#### Learning Curve

A developer writing a GraphQL-based frontend application will have to learn the syntax of the GraphQL language.  A developer implementing a GraphQL backend service will have to learn a lot more than just the language. 



## Exploring GraphQL APIs

This chapter covers:

- Using GraphQL's in-browser IDE to test GraphQL requests
- Exploring the fundamentals of sending GraphQL data requests
- Exploring read and write example operations from the GitHub GraphQL API
- Exploring GraphQL's introspective features
- Exploring auto-generated GraphQL APIs

### The GraphiQL Editor

GraphiQL can help you improve these requests, validate your improvements, and debug any of the requeststhat are running into problems.

### The Basics of the GraphQL Language

A GraphQL request is a tree of **fields**.

#### Requests

The source text of a GraphQL request is often referred to as a **document**.

A GraphQL request might also contain an object to represent values of variables that might be used in the request document text. The request might also include meta information about the operations. For example, if the request document contains more than one operation, a GraphQL request must include information about which operation to execute. If the request document contains only one operation, the GraphQL server will just execute that.

There are three types of operations that can be used in GraphQL:

- Query operations that represent a read-only fetch
- Mutation operations that reprpesent a write followed by a fetch
- Subscription operations that repesent a request for real-time data updates

#### Fields

A field in GraphQL describes on discrete piece of information that you can retrieve about an object. It could describe a scalar value, an object, a list of objects.

You can also cusomize a GraphQL schema to support more scalar values with certain formats.

### Examples from the GitHub API

#### Introspective Queries



## Customizing and Organizing GraphQL Operations

This chapter covers:

- Using arguments to customize what a field in a GraphQL request returns
- Using aliases to customize the property namesin a response object
- Using directives to describe alternate runtime executions
- Using fragments to reduce any duplicated text in a GraphQL document
- Composing queries and separating data requirement responsibilities
- Using inline fragments with interfaces and unions

### Customizing Fields with Arguments

#### Identifying a single record to return

```
query UserInfo {
  user(email: "jane@doe.name") {
  	firstName
  	lastName
  	userName
  }
}
```

