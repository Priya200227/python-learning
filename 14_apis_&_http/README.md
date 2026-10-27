# APIs & HTTP

> Learning notes on Application Programming Interfaces (APIs), HTTP, common API styles, and how APIs connect modern applications—including AI-powered systems.
>
> **Learning sources:**
> [What is an API? | Introduction to APIs - CampusX](https://www.youtube.com/watch?v=WJKsPchji0Q)
> 
> [Complete APIs Crash Course in 14 Minutes — Vishakha Sadhwani](https://www.youtube.com/watch?v=UXA8MJUWUqU&t=48s)
> 
> **Notes format:** Concept summaries, comparisons, examples, and revision checklist.

---

## Table of Contents

- [Learning Objectives](#learning-objectives)
- [1. What Is an API?](#1-what-is-an-api)
- [2. Why Do We Need APIs?](#2-why-do-we-need-apis)
- [3. How an HTTP API Request Works](#3-how-an-http-api-request-works)
- [4. HTTP Methods](#4-http-methods)
- [5. Common Request and Response Components](#5-common-request-and-response-components)
- [6. REST API Fundamentals](#6-rest-api-fundamentals)
- [7. API Styles and Communication Patterns](#7-api-styles-and-communication-patterns)
- [8. APIs in AI Applications](#8-apis-in-ai-applications)
- [9. API Infrastructure and Operational Concerns](#9-api-infrastructure-and-operational-concerns)
- [10. Python API Frameworks](#10-python-api-frameworks)
- [11. Important Distinctions and Corrections](#11-important-distinctions-and-corrections)
- [12. Practical Learning Roadmap](#12-practical-learning-roadmap)
- [13. Revision Checklist](#13-revision-checklist)
- [14. Glossary](#14-glossary)
- [References](#references)

---

## Learning Objectives

After studying this topic, I should be able to:

- Explain what an API is and why applications use APIs to communicate.
- Describe the client–server request/response model.
- Recognize HTTP methods, URLs/endpoints, headers, request bodies, status codes, and JSON payloads.
- Explain REST, GraphQL, SOAP, gRPC, WebSockets, webhooks, and WebRTC at a high level.
- Identify common ways APIs are used in AI applications.
- Understand why authentication, rate limits, logging, monitoring, and documentation matter.
- Know when to consume an existing API and when to build an API with a framework such as FastAPI.

## 1. What Is an API?

**API** stands for **Application Programming Interface**. It defines how one software component can interact with another through an agreed interface and contract.

An API can expose data or operations without requiring the client to know how the underlying system is implemented.

### A simple analogy: customer, waiter, and chef

- **Customer:** The client application making a request.
- **Waiter:** The API interface through which the request is placed and the result is returned.
- **Chef/kitchen:** The server-side application and its internal logic.
- **Order/result:** The request and response data.

The client does not need to enter the kitchen or understand how the food is prepared. It needs to follow the agreed process and receive the result.

### Common terms

| Term | Meaning |
|---|---|
| **Client** | The application or system making a request. |
| **Server** | The system that receives the request and provides a response or performs an operation. |
| **API** | The defined interface/contract used by software components to interact. |
| **Endpoint** | A particular address and operation exposed by an API, commonly represented by a URL path plus an HTTP method. |
| **HTTP** | A protocol commonly used for communication between web clients and servers. |
| **JSON** | A text-based data format commonly used to represent request and response payloads. |
| **Request** | The message sent by a client. |
| **Response** | The message returned by a server. |

> **Remember:** An API is not the same thing as HTTP or JSON. HTTP is a communication protocol; JSON is a data format; an API is the interface/contract. APIs can also use protocols and formats other than HTTP and JSON.

## 2. Why Do We Need APIs?

### Before a system is split into independent components

A monolithic application may contain its user interface, business logic, and data-access logic in one deployable application. This can be appropriate for many systems, but as a product grows, tightly coupled components can become harder to change, test, scale, or reuse independently.

### The API-based approach

A backend can expose a defined interface that multiple clients use:

- A website
- An Android application
- An iOS application
- An internal service
- An automation script
- An AI agent or application

The clients can share backend capabilities instead of implementing the same business logic separately.

### Example

A food-ordering product might expose operations for:

- Listing restaurants and menus
- Creating an order
- Checking an order's status
- Processing a payment
- Retrieving delivery information

A website and mobile app can call the same backend API for these operations.

**Key idea:** APIs make integration and reuse possible. They can support separation between components, but an API does not automatically make a system microservices-based or guarantee loose coupling; those outcomes depend on the overall architecture and design.

## 3. How an HTTP API Request Works

A typical request/response interaction follows this sequence:

1. A client prepares an HTTP request.
2. The request identifies a server and resource/endpoint.
3. The request may include query parameters, headers, and a body.
4. The server validates the request and performs the requested operation.
5. The server returns an HTTP status code, headers, and—often—a response body.
6. The client checks the result and handles success or failure.

### Example request

```http
GET /v1/posts?sort=latest&page=1&page_size=10 HTTP/1.1
Host: api.example.com
Accept: application/json
Authorization: Bearer <access-token>
```

What each part means:

- `GET`: asks to retrieve a resource.
- `/v1/posts`: the path identifies the posts resource; `v1` indicates an API version in this example.
- `sort=latest`, `page=1`, `page_size=10`: query parameters that request a particular ordering and page of results.
- `Accept: application/json`: tells the server that JSON is an acceptable response format.
- `Authorization: Bearer ...`: sends a bearer access token, if the API requires one.

A server might respond with:

```http
HTTP/1.1 200 OK
Content-Type: application/json
```

```json
{
  "page": 1,
  "page_size": 10,
  "items": [
    {
      "id": 101,
      "title": "Introduction to APIs"
    }
  ]
}
```

This is an illustrative example, not a real API endpoint.

### Request components

| Component | Purpose | Example |
|---|---|---|
| Method | Indicates the intended operation | `GET` |
| URL/path | Identifies the target resource | `/v1/posts` |
| Query parameters | Filter, sort, or paginate results | `?page=1&page_size=10` |
| Headers | Carry metadata, preferences, or credentials | `Accept`, `Authorization` |
| Request body | Carries data submitted to the server, when applicable | JSON sent in a `POST` request |

### Response components

| Component | Purpose | Example |
|---|---|---|
| Status code | Indicates the outcome category | `200 OK` |
| Headers | Describe the response | `Content-Type: application/json` |
| Response body | Contains returned data or an error description | JSON object |

### API versioning

A path such as `/v1/posts` is one common versioning convention. Versioning helps teams evolve an API while managing compatibility with existing clients. Not every API uses URL-based versioning; some use headers or other conventions.

### Pagination

Pagination splits a large result set into smaller responses. Common approaches include page/page-size parameters, offsets, or cursors. It can reduce response size and help clients process large datasets efficiently.

## 4. HTTP Methods

HTTP methods describe the intended action for a request. Their exact behavior depends on the API contract, but the following are common conventions.

| Method | Common purpose | Example |
|---|---|---|
| `GET` | Retrieve a resource | `GET /restaurants/101/menu` |
| `POST` | Create a resource or submit data for processing | `POST /orders` |
| `PUT` | Replace a resource representation, or create/replace at a known URI when supported | `PUT /users/501` |
| `PATCH` | Partially update a resource | `PATCH /orders/501` |
| `DELETE` | Delete a resource | `DELETE /cart/items/10` |

### Example: creating an order

```http
POST /orders HTTP/1.1
Content-Type: application/json
Accept: application/json
```

```json
{
  "restaurant_id": 101,
  "items": [
    {
      "item_id": 22,
      "quantity": 2
    }
  ]
}
```

The server validates the input and may return a created order and a status such as `201 Created`.

**Important HTTP properties:**

- **Safe:** A method is safe when it is intended to read information without requesting a state-changing action. `GET` should be safe.
- **Idempotent:** Repeating the same request has the same intended effect as making it once. `GET`, `PUT`, and `DELETE` are defined as idempotent methods; `POST` is not generally idempotent. The exact result of an operation still depends on the API implementation.
- `PUT` and `PATCH` are not interchangeable: `PUT` commonly replaces a representation, while `PATCH` applies a partial modification.

## 5. Common Request and Response Components

### Headers

Headers carry metadata about a request or response. Common examples include:

- `Accept`: response media types the client can accept.
- `Content-Type`: media type of the message body, such as `application/json`.
- `Authorization`: credentials or an access token.
- `User-Agent`: information about the client.
- `Cache-Control`: caching instructions.

### JSON

JSON is a common text-based format for structured data.

```json
{
  "user_id": 42,
  "name": "Priya",
  "is_active": true,
  "skills": ["Python", "SQL", "APIs"]
}
```

JSON supports objects, arrays, strings, numbers, booleans, and `null`. It is widely used because it is human-readable and supported across programming languages.

### Common HTTP status codes

| Code | Meaning | Typical interpretation |
|---|---|---|
| `200 OK` | Request succeeded | A successful read or operation |
| `201 Created` | Resource created | Often returned after successful creation |
| `204 No Content` | Request succeeded with no response body | Common after a successful deletion or update |
| `301` / `302` | Redirect | Resource or request is redirected |
| `400 Bad Request` | Invalid request | The request cannot be processed as sent |
| `401 Unauthorized` | Authentication is missing or invalid | Valid credentials may be required |
| `403 Forbidden` | Request understood but not allowed | Caller lacks permission |
| `404 Not Found` | Resource/route not found | Check the URL or resource identifier |
| `409 Conflict` | Conflict with current resource state | For example, a duplicate or state conflict |
| `422 Unprocessable Content` | Request content fails validation or cannot be processed under the API's rules | Check submitted fields and constraints |
| `429 Too Many Requests` | Rate limit exceeded | Slow down; follow `Retry-After` if provided |
| `500 Internal Server Error` | Unexpected server error | Failure on the server side |
| `502 Bad Gateway` | Gateway received an invalid upstream response | May be a temporary upstream issue |
| `503 Service Unavailable` | Service temporarily unavailable | Retry carefully if appropriate |

Status-code conventions vary by API. Always check the API's documentation and error response body rather than relying on the code alone.

## 6. REST API Fundamentals

**REST** stands for **Representational State Transfer**. It is an architectural style commonly used to design web APIs. REST is not itself a protocol.

A REST-style API generally models information as resources and uses standard HTTP semantics to interact with those resources. JSON is common, but REST does not require JSON specifically.

### Example resource-oriented endpoints

| Operation | Method | Endpoint |
|---|---|---|
| List users | `GET` | `/users` |
| Retrieve one user | `GET` | `/users/10` |
| Create a user | `POST` | `/users` |
| Replace a user | `PUT` | `/users/10` |
| Partially update a user | `PATCH` | `/users/10` |
| Delete a user | `DELETE` | `/users/10` |

### Common REST concepts

- **Resource-oriented design:** URLs identify resources.
- **Stateless interaction:** Each request contains the information needed to understand that request; the server should not depend on hidden client session state for the meaning of a request.
- **Uniform interface:** Consistent conventions make APIs easier to understand.
- **Cacheability:** Responses can indicate whether they may be cached.
- **Client–server separation:** The client and server have distinct responsibilities.

### REST trade-off: over-fetching and under-fetching

- **Over-fetching:** The response includes more data than the client needs.
- **Under-fetching:** The client needs additional requests to obtain related data.

For example, a dashboard may need a user's name, recent orders, reward points, and saved addresses. A REST implementation might expose these through several endpoints, although the API could also provide a tailored endpoint to reduce round trips.

GraphQL is one alternative when clients need more flexible control over requested fields.

## 7. API Styles and Communication Patterns

These approaches solve different problems. They should not be treated as interchangeable protocols or as a simple ranking from “old” to “new.”

| Style / pattern | Core idea | Common use cases | Trade-offs / considerations |
|---|---|---|---|
| **REST** | Resource-oriented API design commonly using HTTP methods | Public web APIs, CRUD operations, mobile/web backends | Familiar conventions; clients may need multiple requests or receive more data than required |
| **GraphQL** | Clients specify the fields/data they need through a query language and schema | Flexible data fetching for complex client interfaces | Requires query validation, authorization, performance controls, and careful caching strategy |
| **SOAP** | A formal messaging protocol with XML-based envelopes and defined standards | Some enterprise, financial, telecom, and legacy integrations | Formal contracts and mature standards, but messages can be verbose and implementation can be complex |
| **gRPC** | Remote procedure calls commonly defined with Protocol Buffers and transported over HTTP/2 | Internal service-to-service communication, typed contracts, streaming | Efficient and strongly typed; requires tooling and is less directly human-readable |
| **WebSockets** | Persistent, bidirectional communication over a connection | Chat, collaborative apps, live dashboards, real-time updates | Supports ongoing messages in both directions; connection lifecycle and scale require care |
| **Webhooks** | One system sends an HTTP request to another system's endpoint when an event occurs | Payment notifications, repository events, order updates | Avoids constant polling, but requires reachable endpoints, signature verification, retries, and duplicate-event handling |
| **WebRTC** | A set of technologies/APIs for real-time audio, video, and data communication, often peer-to-peer | Video calls, screen sharing, real-time data exchange | Networking, connectivity, NAT traversal, and signaling can make implementation complex |

### REST vs GraphQL

Imagine a customer dashboard that needs a user's name, the three most recent orders, reward points, saved addresses, and recommendations.

With REST, the client may call several endpoints depending on the API's design. With GraphQL, the client can request the relevant fields through a query against a GraphQL endpoint.

GraphQL can reduce unnecessary data transfer or round trips in suitable applications, but it does not guarantee better performance. Server-side query complexity, authorization, caching, and database access still matter.

### SOAP

SOAP messages use XML envelopes and operate under a formal protocol specification. SOAP can be used with different underlying transport mechanisms; it is not simply “REST with XML.” It remains relevant in systems that depend on established enterprise contracts and standards.

### RPC and gRPC

**RPC** means **Remote Procedure Call**: a program invokes an operation on another system as though calling a function, while the RPC framework handles communication details.

gRPC commonly uses Protocol Buffers for service and message definitions and supports unary and streaming communication patterns. Performance depends on workload, payload, network, implementation, and configuration; avoid treating a fixed “times faster than REST” claim as universally true.

### WebSockets

A WebSocket connection supports ongoing, bidirectional messages after the connection is established. This differs from the usual independent HTTP request/response interaction. AI applications may stream output using WebSockets, Server-Sent Events (SSE), or provider-specific streaming protocols; not all AI streaming uses WebSockets.

### Webhooks

A webhook is an event-driven callback. Instead of repeatedly asking whether an event has happened (polling), a receiving application exposes an endpoint and the source system sends it a request when the event occurs.

Examples include:

- A payment provider notifying an application that a payment succeeded.
- A code-hosting platform notifying a service that a repository event occurred.
- An e-commerce system notifying an inventory service that an order was placed.

Production webhook handlers should verify authenticity, respond appropriately, handle retries, and make processing safe against duplicate event delivery.

### WebRTC

WebRTC supports real-time audio, video, and data exchange between browsers or applications. Media/data may travel peer-to-peer when possible, but real deployments can use relay servers (such as TURN) when direct connectivity is unavailable. Applications also need signaling to establish sessions.

## 8. APIs in AI Applications

APIs connect AI models and applications to data, tools, and business workflows. A model can generate content, but APIs allow an application to retrieve information, execute operations, and exchange data with other systems.

### Common AI API workflows

1. **Calling a model API**
   - Send text, images, audio, or structured input.
   - Receive generated output and relevant metadata.
2. **Connecting tools**
   - Search an information source.
   - Query an authorized database or business service.
   - Access calendar, ticketing, or other application functionality.
3. **Orchestrating a workflow**
   - Retrieve context.
   - Call a model.
   - Validate the output.
   - Invoke a tool when needed.
   - Save or return the result.

### Illustrative AI workflow

```text
User request
    |
    v
Application / API endpoint
    |
    v
Retrieve relevant context -----> Search or database API
    |
    v
Call model API
    |
    v
Validate model output
    |
    +----> Tool call, if needed -----> External service API
    |
    v
Return result to the user
```

This is a conceptual workflow. Actual architectures vary.

### Authentication and model APIs

A provider may require an API key or another credential. Some APIs accept credentials through an `Authorization: Bearer ...` header, but authentication methods differ by provider.

A bearer token is not necessarily a JWT. A JWT is a token format with a defined structure and can contain claims; a server may still need to check expiry, issuer, audience, permissions, revocation, or other conditions. Do not assume that every token is a JWT or that validating a JWT always eliminates database lookups.

### OpenAI-compatible APIs

Some model providers offer APIs that follow conventions similar to OpenAI's API formats. This can reduce integration changes when switching providers, but compatibility is not guaranteed to be complete. Differences can exist in supported models, parameters, tool calling, structured output, streaming, and error behavior.

### Model Context Protocol (MCP)

**Model Context Protocol (MCP)** is a standard for connecting AI applications to tools and external context through a defined protocol. It is related to API integration, but it is not itself a replacement for HTTP, REST, or every provider's model API. An MCP client communicates with MCP servers that expose tools or resources according to the protocol.

### Practical reliability considerations for AI APIs

- Keep API credentials out of source code and version control.
- Set request timeouts.
- Handle authentication failures, invalid input, rate limits, and provider outages.
- Respect rate limits and `Retry-After` guidance.
- Validate external data and model output before relying on it.
- Track latency, errors, and usage/cost where applicable.
- Avoid retrying non-idempotent operations blindly; retries can create duplicate actions.

## 9. API Infrastructure and Operational Concerns

A production API is more than a route that returns data. It also needs appropriate controls and observability.

### API gateway

An API gateway can sit in front of backend services and centralize some cross-cutting responsibilities, such as:

- Routing requests to the correct service.
- Authentication or integration with authorization systems.
- Rate limiting and traffic controls.
- Request/response logging and metrics.
- Other policy enforcement, depending on the gateway.

Not every application needs a separate gateway; the architecture and scale determine whether it is useful.

### Authentication vs authorization

- **Authentication:** Who or what is making the request?
- **Authorization:** Is that caller allowed to perform this operation?

For example, a user may be authenticated but still not have permission to access another user's order.

### Rate limiting

Rate limits restrict request volume over a period or within a quota. They protect services from overload and help enforce usage plans. Clients should handle `429 Too Many Requests` and follow server guidance such as `Retry-After` when provided.

### Logging and monitoring

Useful signals include:

- Request volume and latency.
- Error rates and status-code distribution.
- Upstream provider failures.
- Rate-limit events.
- Resource utilization.
- AI token usage or costs, when relevant.

Logs should avoid exposing secrets, access tokens, passwords, or unnecessary sensitive data.

### Documentation

API documentation explains available endpoints, inputs, authentication, response schemas, errors, and examples. **OpenAPI** is a specification for describing HTTP APIs. **Swagger UI** is a tool that can render interactive documentation from an OpenAPI description.

## 10. Python API Frameworks

Frameworks help developers define endpoints, process requests, validate data, and produce responses.

| Framework | Language / ecosystem | High-level use |
|---|---|---|
| **FastAPI** | Python | Modern APIs with type hints, Pydantic integration, and generated OpenAPI documentation |
| **Flask** | Python | Lightweight web applications and APIs with a minimal core |
| **Django REST Framework** | Python / Django | APIs in applications built on the Django ecosystem |
| **Express** | JavaScript / Node.js | Web applications and APIs in the Node.js ecosystem |
| **Spring Boot** | Java | Enterprise applications and services |
| **ASP.NET Core** | C# / .NET | Web APIs and services in the .NET ecosystem |

### What happens inside an API endpoint?

A typical endpoint:

1. Accepts a request.
2. Parses and validates input.
3. Runs application/business logic.
4. May call a database, model, or external API.
5. Returns a response with an appropriate status code.

For a Python learning path, **FastAPI** is a useful next step after understanding HTTP and API fundamentals. Learn to consume APIs as a client before focusing heavily on building servers.

## 11. Important Distinctions and Corrections

These distinctions make the notes more precise:

- **API vs HTTP vs JSON:** An API is an interface/contract; HTTP is a protocol; JSON is a data format.
- **REST vs HTTP:** REST is an architectural style often implemented over HTTP, not a protocol.
- **Endpoint vs API:** An endpoint is one exposed operation/address within an API; an API may contain many endpoints.
- **Authentication vs authorization:** Identity and permissions are different checks.
- **Bearer token vs JWT:** A bearer token is a credential sent by the holder; it may or may not be a JWT.
- **PUT vs PATCH:** PUT commonly replaces a resource representation; PATCH applies a partial update.
- **Webhooks vs polling:** Webhooks notify a receiver when an event occurs; polling repeatedly asks for updates.
- **WebSockets vs webhooks:** WebSockets provide a persistent bidirectional connection; webhooks are event-triggered HTTP callbacks.
- **WebSockets vs AI streaming:** AI streaming may use WebSockets, SSE, or other mechanisms; WebSockets are not the only option.
- **gRPC performance:** gRPC can be efficient for suitable workloads, but no fixed speed advantage applies to every comparison.
- **GraphQL and GitHub:** GitHub provides both REST and GraphQL APIs; it is inaccurate to describe its entire API as GraphQL.
- **APIs and monoliths:** A monolith can expose APIs. Using an API does not automatically mean the system is distributed or composed of microservices.
- **JSON is common, not universal:** APIs may use XML, binary formats, plain text, or other representations.
- **JWT validation:** Signature validation alone is not a complete authorization policy. Expiration, claims, permissions, and application requirements must also be considered.

## 12. Practical Learning Roadmap

These notes cover the concepts and comparisons. To turn them into working knowledge, follow this sequence.

### Stage 1 — Understand HTTP and API basics

Learn and practice:

- Client/server and request/response.
- URLs, paths, query parameters, headers, and bodies.
- JSON.
- HTTP methods and common status codes.
- REST conventions.
- API documentation and authentication basics.

**Practice:** Use an API client such as Postman or an interactive API documentation page to send requests and inspect responses.

### Stage 2 — Consume an API using Python

Use a synchronous HTTP client such as `requests` and practise:

- Making `GET` and `POST` requests.
- Sending query parameters, headers, and JSON.
- Reading status codes, headers, and response bodies.
- Handling HTTP errors and network exceptions.
- Setting timeouts.
- Handling pagination and rate limits.
- Validating returned data with Pydantic.
- Reading credentials from environment variables rather than hardcoding them.

**Mini-project idea:** Build a public-API data collector that retrieves records, validates the response, handles failures, and saves clean data to JSON or CSV.

### Stage 3 — Build a small API using FastAPI

Practise:

- Creating a FastAPI application and routes.
- Path and query parameters.
- Request bodies and Pydantic models.
- Response models and status codes.
- Error handling.
- Basic CRUD operations.
- Exploring generated documentation at `/docs`.
- Testing endpoints with pytest and FastAPI's test tools.

**Mini-project idea:** Build an API that serves, filters, and paginates the records collected in Stage 2.

### Stage 4 — Learn reliability and AI integrations

Once the basics are comfortable, practise:

- API-key handling and secret management.
- Timeouts, rate limits, retries, and error handling.
- Calling a model provider's API.
- Validating inputs and outputs.
- Async HTTP requests with `httpx` after synchronous requests are familiar.
- Streaming, webhooks, and more advanced patterns when a project requires them.

Do not try to master REST, GraphQL, SOAP, gRPC, WebSockets, webhooks, and WebRTC in implementation-level detail all at once. Understand their purpose first, then deepen the relevant ones through projects.

## 13. Revision Checklist

Use this checklist to test understanding without looking at the notes.

- [ ] Can I explain what an API is using a real-world analogy?
- [ ] Can I distinguish an API, endpoint, HTTP, and JSON?
- [ ] Can I describe the full HTTP request/response cycle?
- [ ] Can I identify the method, path, query parameters, headers, and body in a request?
- [ ] Can I explain GET, POST, PUT, PATCH, and DELETE?
- [ ] Can I explain safe and idempotent methods at a basic level?
- [ ] Can I interpret common 2xx, 4xx, and 5xx status codes?
- [ ] Can I explain REST and its common trade-offs?
- [ ] Can I compare REST, GraphQL, SOAP, and gRPC?
- [ ] Can I distinguish WebSockets, webhooks, and WebRTC?
- [ ] Can I describe three ways APIs are used in AI applications?
- [ ] Can I explain authentication vs authorization?
- [ ] Can I explain rate limiting, API gateways, logging, monitoring, and OpenAPI?
- [ ] Can I call an API from Python with a timeout and proper error handling?
- [ ] Can I build and test a basic FastAPI endpoint?
- [ ] Can I explain how I would protect API credentials and avoid committing them to Git?

## 14. Glossary

| Term | Short definition |
|---|---|
| API | Interface/contract through which software components interact |
| Client | System that makes a request |
| Server | System that receives requests and provides responses or performs operations |
| Endpoint | An exposed API operation/address |
| HTTP | Web communication protocol |
| JSON | Text-based structured data format |
| REST | Architectural style for designing networked APIs |
| GraphQL | Query language and runtime for APIs that lets clients request selected fields |
| SOAP | Formal XML-based messaging protocol |
| RPC | Remote Procedure Call |
| gRPC | RPC framework commonly using Protocol Buffers and HTTP/2 |
| WebSocket | Persistent, bidirectional communication protocol |
| Webhook | Event-triggered HTTP callback |
| WebRTC | Technologies for real-time audio, video, and data communication |
| JWT | JSON Web Token, a structured token format |
| API gateway | Layer that can route and apply policies to API traffic |
| Rate limit | Constraint on request volume or usage |
| OpenAPI | Specification for describing HTTP APIs |
| Pydantic | Python library for parsing and validating data against declared models |
| FastAPI | Python framework for building APIs |

---

## References

- [Complete APIs Crash Course in 14 Minutes — Vishakha Sadhwani](https://www.youtube.com/results?search_query=Complete+APIs+Crash+Course+In+14+Minutes+Vishakha+Sadhwani)
- [MDN: HTTP overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/Overview)
- [MDN: HTTP request methods](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Methods)
- [Requests: Quickstart](https://requests.readthedocs.io/en/latest/user/quickstart/)
- [FastAPI: Tutorial](https://fastapi.tiangolo.com/tutorial/)
- [OpenAPI Specification](https://spec.openapis.org/oas/latest.html)
- [Model Context Protocol documentation](https://modelcontextprotocol.io/introduction)

---

## Learning Reflection

**What I learned:** APIs provide a defined way for software components to communicate. HTTP methods, request/response structure, data formats, authentication, and error handling form the foundation of many web integrations. Different API styles serve different requirements, and APIs are especially important in AI applications because they connect models with tools, data, and real-world workflows.

**Next step:** Practise making real HTTP requests from Python, then build and test a small API with FastAPI.

