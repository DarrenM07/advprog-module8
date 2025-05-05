## 1. What are the key differences between unary, server streaming, and bi-directional streaming RPC (Remote Procedure Call) methods, and in what scenarios would each be most suitable?

**Answer:**
RPC supports three primary modes of communication: *unary*, *server streaming*, and *bidirectional streaming*. Unary is the simplest — the client sends one request and receives one response. It’s ideal for basic tasks like logging in or requesting a user profile. Server streaming starts with one request from the client, but the server responds with multiple messages over time. This works best when dealing with large or ongoing data, such as live stock feeds or streaming logs. Bidirectional streaming enables both client and server to send messages simultaneously and independently, making it perfect for real-time applications like chat systems or collaborative editing tools.


## 2. What are the potential security considerations involved in implementing a gRPC service in Rust, particularly regarding authentication, authorization, and data encryption?
**Answer:**
Security in Rust gRPC services revolves around three key areas: authentication, authorization, and encryption. Authentication ensures the request is made by a legitimate user, typically using API keys or tokens like JWT. Authorization controls what actions each user is allowed to perform, preventing unauthorized access to resources. Encryption—especially through TLS—is critical to protect data during transmission. Besides that, it’s important to validate all input data, avoid exposing internal error messages, and safeguard the service from misuse, such as denial-of-service (DoS) attacks. While Rust helps by preventing memory safety issues, secure design and proper configuration are still essential.


## 3. What are the potential challenges or issues that may arise when handling bidirectional streaming in Rust gRPC, especially in scenarios like chat applications?
**Answer:**
Implementing bidirectional streaming in Rust gRPC can be complex, especially in real-time systems like chat apps. Since both the client and server send and receive messages at the same time, you need to manage concurrency without blocking either side. This usually involves asynchronous tools like Tokio and channels, which can be tricky to get right. Handling connection drops or task cancellations gracefully is also important. And due to Rust’s strict ownership model, managing shared state such as user sessions requires careful synchronization to avoid issues like deadlocks or race conditions. Debugging can be harder too, since some problems only show up under real-time conditions.


## 4. What are the advantages and disadvantages of using the `tokio_stream::wrappers::ReceiverStream` for streaming responses in Rust gRPC services?

**Answer:**
Using `tokio_stream::wrappers::ReceiverStream` makes it easy to convert a channel into an async stream, which is great for streaming server responses in Rust gRPC. The main advantage is that it supports clean separation between message producers and consumers, allowing for modular and async-friendly code. However, it also comes with some responsibilities. You need to manually manage the channel’s lifecycle, ensure the receiver doesn’t get dropped prematurely, and avoid potential memory leaks. In high-traffic environments, overusing channels can create performance bottlenecks or complicate error handling if not designed properly.


## 5. In what ways could the Rust gRPC code be structured to facilitate code reuse and modularity, promoting maintainability and extensibility over time?

**Answer:**
To keep Rust gRPC projects scalable and clean, the code should be organized into modular components. Protobuf files, business logic, and data models should each be placed in separate modules. Using traits to define shared behaviors allows for flexible implementations, making it easier to mock or swap components during testing or upgrades. Utility functions and reusable middleware help reduce duplication. Grouping related functionality into distinct modules not only improves readability but also allows multiple developers to work in parallel without conflicts, supporting long-term maintainability.


## 6. In the MyPaymentService implementation, what additional steps might be necessary to handle more complex payment processing logic?

**Answer:**
For a more realistic and robust payment system, several additional layers of logic are necessary. First, the service should confirm that the user exists and has sufficient funds. Then, it may need to communicate with external payment gateways to process the actual transaction. It’s also important to log transactions and implement error handling for scenarios like failed payments. To prevent accidental double payments from repeated requests, idempotency checks should be added. Breaking the logic into smaller functions or modules will make it easier to test, maintain, and expand in the future.


## 7. What impact does the adoption of gRPC as a communication protocol have on the overall architecture and design of distributed systems, particularly in terms of interoperability with other technologies and platforms?

**Answer:**
Adopting gRPC can significantly influence how distributed systems are designed. Because it uses Protocol Buffers and binary encoding, communication is faster and more efficient than traditional REST APIs. Its schema-driven approach enforces strict contracts between services, reducing errors and improving clarity. However, gRPC works best when all services use it consistently. If your system includes non-gRPC components, additional tools like gateways or protocol translators may be needed to bridge communication gaps, which can add complexity to the architecture.


## 8. What are the advantages and disadvantages of using HTTP/2, the underlying protocol for gRPC, compared to HTTP/1.1 or HTTP/1.1 with WebSocket for REST APIs?

**Answer:**
HTTP/2, the foundation of gRPC, offers several advantages over HTTP/1.1. It supports multiplexing, meaning multiple messages can travel over a single connection simultaneously. It also uses binary format and header compression, which improves speed and reduces bandwidth usage. Compared to WebSocket, HTTP/2 provides a more structured and standardized protocol, which is beneficial for error handling and debugging. However, gRPC doesn’t work natively in browsers, so additional tools like gRPC-web are needed for frontend integration, making it less suitable for direct browser-based applications.


## 9. How does the request-response model of REST APIs contrast with the bidirectional streaming capabilities of gRPC in terms of real-time communication and responsiveness?

**Answer:**
REST APIs operate on a basic request-response model—one client request results in one server response. While this model is straightforward and effective for simple operations, it lacks flexibility for real-time use cases. In contrast, gRPC supports bidirectional streaming, where both client and server can exchange multiple messages continuously and independently. This model is far more suitable for real-time communication scenarios like messaging platforms, live notifications, or multiplayer games where low-latency interactions are critical.


## 10. What are the implications of the schema-based approach of gRPC, using Protocol Buffers, compared to the more flexible, schema-less nature of JSON in REST API payloads?

**Answer:**
gRPC’s reliance on Protocol Buffers means all data formats must be explicitly defined upfront, which improves communication consistency and reduces the likelihood of unexpected errors. It also results in smaller, faster payloads due to binary encoding. On the other hand, REST uses JSON, which is more flexible and human-readable but can lead to inconsistencies or parsing issues if data structures change unexpectedly. Overall, gRPC favors performance and safety, while JSON is better for scenarios requiring flexibility or quick prototyping.
