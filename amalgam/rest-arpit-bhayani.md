```markdown
# REST (Representational State Transfer)

## 1. Core Concepts of REST
    * **Definition**: Representational State Transfer
    * **Key Idea**: Representation of entities in a client-server architecture.
    * **Client-Server Interaction**:
        * Client sends a request.
        * Server sends a response (often in JSON representation).
    * **Everything is a Resource**:
        * Definition: An entity in your system.
        * Examples:
            * Library Management System: Student, Book.
            * E-commerce: Seller, User, Item, Purchase Order.
        * Resources can have their own storage (REST is independent of storage).
        * Any data in the service/system is an exposed resource (e.g., students exposed via HTTP endpoints).
    * **REST as a Specification**:
        * Defines how clients should ask for things and how servers should respond.
        * Does not enforce specific actions but suggests best practices.
        * Does not enforce storage mechanisms (e.g., tables, MongoDB, document storage, key-value).
        * Focuses on client-server communication format.
    * **Data Storage vs. Data Serving**:
        * Storage method (e.g., relational tables, Apache Cassandra) is independent of how data is served.
        * Example: Store student info in tables, serve as JSON. No need for exact synergy.
        * Serve data in the representation the client asks for.

## 2. Representation in REST
    * **Client Demands Representation**:
        * Client can ask for data in specific formats: JSON, Text, XML, CSV.
        * Server should support the requested representation if possible.
        * Not mandated for the server to support all, but a way to do it exists.
        * This is why it's called "Representational State Transfer."
    * **Content-Type Header**: Used in HTTP to specify sending and expecting content types.
    * **Multiple Representations**:
        * A solid REST representation might support multiple formats (e.g., XML, JSON).
        * Not mandated; choosing just JSON is perfectly fine.
    * **Internal vs. External Representation**:
        * Internal: How data is stored in the database (e.g., table rows and columns).
        * External: How data is sent to the client (e.g., JSON).
        * Depends on server implementation.
    * **Operations on Resources**:
        * All client requests happen over a resource.
        * Resource typically specified in the HTTP URL.
        * Actions (create, update, delete) specified as verbs.

## 3. REST and Protocols
    * **Protocol Agnostic**: REST is a specification, not tied to a specific protocol.
        * Can use TCP, HTTP, or any other protocol.
    * **Most Common Implementation**: REST with HTTP.
        * They gel "so beautifully well."
        * Often, when writing APIs (exposing HTTP endpoints), people think they are writing REST APIs.

## 4. Why REST and HTTP Gel So Well
    * **HTTP Verbs (Methods)**:
        * Powerful feature: GET, PUT, POST, DELETE.
        * Action specified by HTTP method.
        * Resource identified by URL.
        * Example: `DELETE /users/{user_id}`
            * `DELETE`: Action (HTTP verb).
            * `/users/{user_id}`: Resource identification.
        * **Multiplexing on the Same Resource**:
            * Same URL (e.g., `/students/1`) can be used for different actions by changing the HTTP method:
                * `GET /students/1`: Get student detail.
                * `POST /students/1`: Update student detail (Note: PUT is often used for updates to a specific resource, POST can be used for creation or updates depending on idempotency).
                * `DELETE /students/1`: Delete the student.
            * This is a well-defined HTTP specification.
        * **RESTful URL Design**:
            * URL should identify the resource, not the action.
            * Incorrect (non-RESTful): `/getStudent`, `/updateStudent`.
            * Correct (RESTful): `GET /student/{id}`, `POST /student/{id}` (or `PUT /student/{id}`).
    * **Existing Tooling and Ecosystem**:
        * **Clients**:
            * Widely available: cURL, Postman, Python `requests` library, browsers.
            * No need to reinvent the wheel for making requests and handling responses.
        * **Web Caches**:
            * Leverage existing caches: Nginx cache, Varnish, HAProxy.
            * Improves performance without custom caching mechanisms.
        * **Monitoring Tools**:
            * Well-built for HTTP: Distributed request tracing, packet sniffing, debugging, HTTP monitoring, alerting, APM.
            * No need to rebuild monitoring for a new specification.
        * **Load Balancers**:
            * HTTP-based load balancers are prevalent.
            * No need for separate load balancing strategies.
        * **Security Controls**:
            * SSL, optimizations, etc., are available out-of-the-box with HTTP/web servers.

## 5. Downsides of Using REST over HTTP
    * **Consumption is Not Easy**:
        * No standardized stub generation (unlike RPCs e.g., gRPC).
        * Client receives response (e.g., JSON) and needs to convert it to native objects.
        * Additional serialization/deserialization and conversion overhead.
        * Can be expensive for low-latency applications.
    * **Consumption is Repetitive**:
        * Anyone consuming the resource needs to implement similar logic:
            * Serialization/deserialization.
            * Conversion to/from native objects.
            * Handling failures, timeouts, retries.
            * Compression, security.
        * Requires creating shared internal libraries for standardization or repeating code.
        * RPCs might abstract some of these concerns.
    * **Some Web Servers May Not Support All HTTP Verbs**:
        * Standard servers (Jersey, Nginx, uWSGI) generally support all.
        * If a chosen server only supports GET/POST, it limits REST's potential (multiplexing actions).
        * May lead to non-RESTful endpoints (e.g., `/student/1/delete`).
        * Need to ensure tech stack supports all necessary HTTP verbs.
    * **HTTP Payloads Can Be Huge**:
        * JSON is built for readability, not low latency.
        * Parsing and creating JSON takes time.
        * Repetitive data in JSON (quotes, colons, commas) increases payload size.
        * Not suitable for ultra-low latency requirements (RPCs with Protobuf are better here).
        * HTTP overhead: TCP three-way handshake for each connection (can be mitigated with HTTP/2, but version matters).
    * **Switching Protocols is Limited**:
        * HTTP runs on TCP.
        * If a use case requires UDP (for performance, tolerating packet loss), REST over HTTP is not an option.
        * Lacks flexibility to change the underlying transport protocol (RPCs might offer this).

## 6. Conclusion on REST over HTTP
    * Despite downsides, REST over HTTP is widely adopted, battle-tested, and works well for most cases.
    * The entire internet largely relies on it.
    * Don't overthink unless there are very specific requirements (e.g., ultra-low latency).
    * Be aware of limitations when choosing.
    * Pick the right specification (REST, RPC) based on SLAs and use cases.

## 7. Comparison with RPC (Brief Mention)
    * RPCs (Remote Procedure Calls) offer:
        * Stub generation for native language integration.
        * Abstraction of concerns like retries, failures (sometimes).
        * Potentially smaller payloads (e.g., Protobuf with gRPC) for low latency.
        * Flexibility in switching underlying protocols.
    * REST over HTTP:
        * No native stub generation.
        * Manual handling of serialization/deserialization, retries, etc. (or via libraries).
        * Larger payloads (JSON).
        * Tied to TCP via HTTP.
```