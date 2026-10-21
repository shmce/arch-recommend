# API & Inter-Service Communication Guide

Use this guide when evaluating protocols, network interfaces, and RPC frameworks for internal or external service communication.

## Protocol Comparison

1. **REST over HTTP/JSON (OpenAPI / RESTful)**
   * **Strengths:** Universal client compatibility, standard HTTP caching, intuitive debugging with cURL, extensive gateway/firewall support.
   * **Weaknesses:** Verbose serialization, payload overhead, no enforced contract without external tooling, over-fetching or under-fetching.
   * **Best For:** Public APIs, developer-facing platforms, edge-to-service communication.

2. **gRPC over HTTP/2 (Protobuf)**
   * **Strengths:** Compact binary serialization, strict schema contracts, multiplexing, bidirectional streaming, automatic client code generation across 10+ languages.
   * **Weaknesses:** Poor browser support without gRPC-Web proxy, difficult manual inspection/debugging, requires HTTP/2-aware load balancers (e.g., Envoy) to avoid connection pinning.
   * **Best For:** Internal backend-to-backend microservice RPCs, high-throughput pipelines, polyglot server communication.

3. **GraphQL**
   * **Strengths:** Client specifies exact fields needed, single request fetches across multiple entity relationships, strong type system.
   * **Weaknesses:** Complex server-side caching, N+1 query vulnerability requiring DataLoader patterns, unpredictable client query complexity and DoS risks.
   * **Best For:** Multi-client applications (web + mobile + tablet) querying complex federated data graphs with diverse screen requirements.

4. **tRPC / Type-Safe RPC (Full-Stack TypeScript)**
   * **Strengths:** Zero code generation step, instant end-to-end type safety from server procedures to frontend hooks via TypeScript compiler inference.
   * **Weaknesses:** Tightly coupled to TypeScript on both client and server; not suitable for polyglot systems or public third-party APIs.
   * **Best For:** Monorepos or tightly coupled frontend/backend systems using TypeScript across the entire boundary.

## Common Architecture Traps

* **gRPC Connection Pinning:** Naive L4 load balancers routing an entire HTTP/2 connection (with all multiplexed RPCs) to a single backend pod, starving others.
* **GraphQL Overuse for Simple CRUD:** Introducing Apollo/Relay federation overhead for simple services where simple REST or tRPC routes would suffice.
