# GraphQL

## Introduction

GraphQL is a query language for APIs and a runtime for executing those queries by using a type system you define for your data.

Key Concept of GraphQL:

1. Query Language:

- GraphQL allows clients to request exactly the data they need, nothing more. This contrasts with REST APIs, where you often receive more data than required or have to make multiple requests to get related data.
- Clients can ask for specific fields within resources, and even get deeply nested information in a single query.

2. Single Endpoint:

- Unlike REST, which has multiple endpoints (eg: /users, /posts, ...)
- GraphQL exposes a single endpoint (eg: /graphql) for all queries and mutations.

3. Scheme and Types:
- 
