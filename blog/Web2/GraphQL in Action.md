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

