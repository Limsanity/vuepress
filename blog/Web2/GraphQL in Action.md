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

