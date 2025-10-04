## Core Java & Advanced

1. **Comparable vs Comparator**  
   Explain the key differences. Which is more flexible and suitable for real-world projects?

2. **Garbage Collection Details**  
   How does the Garbage Collector decide which objects to clean? What are the differences between Minor GC & Major GC?

3. **String Immutability**  
   Why is String immutable in Java? What are its benefits in multi-threaded environments?

4. **ClassLoader Mechanics**  
   How does the ClassLoader work in Java? Can multiple classloaders exist in a single JVM?

5. **StringBuffer vs StringBuilder vs String**  
   What are the differences and which one should be used where?

***

## Collections & Performance

6. **CopyOnWriteArrayList Internals**  
   How does CopyOnWriteArrayList work internally? When is it preferred?

7. **Fail-fast vs Fail-safe Iterators**  
   What is the difference between them?

8. **Stream API vs Traditional Loops**  
   If you have millions of records to process, which do you choose and why?

9. **TreeMap Ordering**  
   How does TreeMap maintain ordering? Can custom sorting logic be provided?

10. **Finding Duplicates in Java 8**  
   How do you find duplicates using Java 8 features?

***

## Spring & Spring Boot

11. **@Transactional Scope**  
   What is the difference between @Transactional at the class level vs method level?

12. **Spring Boot Auto-configuration**  
   Explain how Spring Boot's auto-configuration works internally.

13. **Multiple Datasources Configuration**  
   How do you configure multiple datasources in Spring Boot?

14. **@RequestParam vs @PathVariable vs @RequestBody**  
   Differences and use-cases for each annotation.

15. **Monitoring & Optimizing Spring Boot Apps**  
   How do you monitor and optimize production Spring Boot applications?

***

## Microservices & System Design

16. **Distributed Logging**  
   How to implement distributed logging within microservices?

17. **Synchronous vs Asynchronous Communication**  
   What’s the difference in microservices?

18. **Service Scalability**  
   Design a service to handle 1000+ requests per second.

19. **Cross-microservice Transactions**  
   How do you handle transactions spanning multiple microservices?

20. **Circuit Breaker Pattern**  
   Can you explain the circuit breaker pattern? When have you used it?

***

## Database & Real-time Scenarios

21. **Pagination Implementation in SQL + Java**  
   How is pagination done?

22. **Query Performance Debugging**  
   If a query takes 5 seconds, how would you debug & optimize it?

23. **Optimistic vs Pessimistic Locking**  
   What are their differences and which one is better for microservices?

24. **Caching in Spring Boot + DB**  
   How do you implement caching to reduce database load?

25. **Production Failure Diagnosis**  
   If a service keeps failing in production, how do you determine whether it's due to code, infrastructure, or a DB issue?
=============================================================================

### Core Java & Advanced

#### 1. Difference between Comparable and Comparator. Which one is more flexible in real projects?
**Comparable** is an interface used to define the natural ordering of objects (single sorting rule, typically implemented in the entity class itself through `compareTo`). **Comparator** is used for custom ordering (can define multiple sorting logics externally using `compare`). For real projects, **Comparator** is more flexible since it allows sorting without altering the original class and supports multiple ways of sorting.

#### 2. How does Garbage Collector decide which objects to clean? Difference between Minor GC & Major GC?
The Java Garbage Collector cleans objects that are no longer reachable from any reference chain. **Minor GC** occurs in the Young Generation, collecting short-lived objects and promoting survivors. **Major GC** (or Full GC) happens in the Old Generation, reclaiming memory for long-lived objects and is more expensive in terms of pause time since it sweeps more memory.

#### 3. Why is String immutable in Java? Benefits in multi-threaded environments?
A Java String is immutable so that its value cannot be changed once created. This allows sharing of String objects across multiple threads safely, without synchronization. Immutability reduces memory usage (string pool), improves application security, maintains consistent hashcodes (essential in collections), and prevents bugs in concurrent scenarios. 

#### 4. How does ClassLoader work in Java? Can we have multiple classloaders in one JVM?
The ClassLoader loads Java classes into the JVM on demand. Yes, multiple classloaders can exist in one JVM (system, application, custom loaders), enabling class isolation and modular loading. This is crucial for containers and app servers.

#### 5. Difference between StringBuffer, StringBuilder, and String. When to use which?
- **String:** Immutable, thread-safe, for constants and infrequent changes.
- **StringBuffer:** Mutable, thread-safe (synchronized), for use in multi-threaded scenarios needing frequent changes.
- **StringBuilder:** Mutable, not thread-safe, for single-threaded performance-critical cases requiring many modifications.

***

### Collections & Performance

#### 6. How does CopyOnWriteArrayList work internally? When would you prefer it?
CopyOnWriteArrayList is a thread-safe variant of ArrayList. Every time a write operation (add, set, remove) occurs, it creates a new copy of the underlying array. Readers work on the snapshot from the moment their iterator was created, so they don't see modifications made after. Prefer CopyOnWriteArrayList when reads vastly outnumber writes, such as in mostly-read shared lists in concurrent environments, as writes are costly.

#### 7. What is the difference between fail-fast and fail-safe iterators?
**Fail-fast** iterators (like ArrayList) throw ConcurrentModificationException if the collection is structurally modified during iteration—these operate on the actual collection and detect changes via a mod count. **Fail-safe** iterators (like those in CopyOnWriteArrayList, ConcurrentHashMap) operate on a clone or snapshot, thus they do not throw such exceptions but may not reflect the latest changes during iteration. Fail-safe is better for iterating in multi-threaded environments where concurrent modifications are expected.

#### 8. If you have millions of records to process, would you use Stream API or traditional loops? Why?
The Stream API allows declarative, functional-like data processing, better readability, and supports parallelization with large datasets via parallel streams. For massive datasets or when parallel processing is required, the Stream API is typically preferred. However, for simple logic where ultimate performance is required and mutability is needed, tuned loops may be a bit faster.

#### 9. How does TreeMap maintain ordering? Can we provide custom sorting logic?
TreeMap uses a Red-Black tree under the hood, keeping its elements sorted according to their natural order (via Comparable) or using a Comparator passed at construction. For custom sorting, supply a Comparator in the TreeMap constructor that defines the desired ordering.

#### 10. How do you find duplicates in a list using Java 8 features?
With Java 8, use the Stream API along with grouping or filtering. For example, by collecting the elements with `Collectors.groupingBy` to a map and filtering where count > 1, or by maintaining a seen set while streaming and selecting items already present in the set. Example:
```java
Set<Integer> seen = new HashSet<>();
list.stream().filter(n -> !seen.add(n)).collect(Collectors.toSet());
***

### Spring & Spring Boot

#### 11. What is the difference between @Transactional at class vs method level?
When @Transactional is used at the class level, all public methods of the class inherit the annotation and are transactional by default, unless overridden at the method level. When applied at the method level, only that method is transactional. Keep in mind, transactions are created only for public methods; private or protected methods with this annotation will be ignored by Spring’s proxy-based system.

#### 12. Can you explain how Spring Boot auto-configuration works internally?
Spring Boot uses auto-configuration to automatically set up application configurations based on the libraries in the classpath and application properties. It uses the `@EnableAutoConfiguration` annotation (included in `@SpringBootApplication`) and conditional logic to check for certain classes or beans, automatically configuring them with sensible defaults or user overrides. The logic is found in `spring-boot-autoconfigure` and can be customized or excluded as needed.

#### 13. How do you configure multiple datasources in Spring Boot?
To configure multiple datasources, define multiple DataSource beans with separate configurations in your application’s properties file. Annotate the configuration classes with `@Configuration`, and use `@Primary` for the main datasource. Register separate JPA or JDBC templates and entity/transaction managers for each datasource. This allows each datasource to be used independently within the application.

#### 14. What’s the difference between @RequestParam, @PathVariable, and @RequestBody?
- `@PathVariable` is used to extract values from the URI path (like `/users/{id}`).
- `@RequestParam` extracts query parameters from the URL (like `/users?name=John`).
- `@RequestBody` is used to bind the entire body of an HTTP request (usually JSON/XML) to a Java object, used mainly in POST/PUT requests for passing complex data.[13][14][15]

#### 15. How do you monitor and optimize Spring Boot applications in production?
Monitor using Spring Boot Actuator, which exposes endpoints for health, metrics, and more. Integrate third-party monitoring tools like Prometheus, Grafana, and ELK for metrics aggregation and visualization. Key practices include tracking response times, CPU/memory usage, request traffic, and exception logs. Optimize by analyzing the metrics to tune thread pools, database connections, and cache, while using alerts for anomaly detection.
***

### Microservices & System Design

**16. How do you implement distributed logging in microservices?**  
Aggregate logs from all services using log shippers (e.g., Fluentd, Logstash), store centrally (e.g., Elasticsearch), use correlation IDs for tracing requests, ensure structured (e.g., JSON) and standardized log formats, and leverage distributed tracing tools (e.g., Jaeger, Zipkin) for monitoring and debugging across services.

**17. What’s the difference between synchronous vs asynchronous communication in microservices?**  
Synchronous communication is a blocking, request/response pattern (e.g., HTTP REST), leading to real-time feedback but potential bottlenecks. Asynchronous uses message brokers (e.g., Kafka, RabbitMQ) where the sender proceeds without waiting, improving scalability, resilience, and decoupling but increasing design complexity.

**18. How would you design a service to handle 1000+ requests per second?**  
Use stateless services with horizontal scaling, load balancers, caching (in-memory like Redis), database sharding/partitioning, connection pooling, and asynchronous processing where possible. Monitor bottlenecks, and leverage cloud autoscaling or managed serverless options for adaptive scale.

**19. How do you handle transactions that span across multiple microservices?**  
Use distributed transaction patterns—Saga (preferred for microservices, uses local transactions and compensating steps), 2-Phase Commit (2PC, for strict consistency), or event-driven/eventual consistency using outbox/event-store patterns and message brokers to coordinate state.

**20. Can you explain the circuit breaker pattern? When have you used it?**  
The circuit breaker protects services from repeated failed calls to a dependent service. When errors cross a threshold, it opens (“breaks” the circuit), blocking calls to the failing service. The system later tries “half-open” test calls before closing (“restoring”) normal flow. This prevents cascading failures and is used for external API/service resilience in microservices.[10][11]

***

### Database & Real-time Scenarios

**21. How do you implement pagination in SQL + Java?**  
Use SQL `LIMIT` and `OFFSET` clauses to fetch subsets of data, and manage pagination variables in Java. Example:  
```sql
SELECT * FROM table LIMIT ? OFFSET ?;
```
Calculate `offset = (pageNumber - 1) * pageSize` in Java, use prepared statements, and iterate through result sets page by page.[12][13]

**22. If a query is taking 5 seconds, how would you debug & optimize it?**  
Analyze the query’s execution plan for table scans, missing indexes, inefficient joins, or suboptimal filters. Optimize by adding or adjusting indexes, rewriting the query, making it SARGable (search-friendly), and reviewing stats/index maintenance. Avoid cursors, use set-based queries, and check server resource bottlenecks.

**23. What’s the difference between optimistic and pessimistic locking? Which is better in microservices?**  
Pessimistic locking blocks a record for exclusive access until commit (good for high-conflict updates but can cause bottlenecks). Optimistic locking allows many reads/writes but uses version/timestamp checks at commit, rolling back on conflict (ideal for microservices with low contention and higher throughput).

**24. How do you implement caching in Spring Boot + DB to reduce load?**  
Use Spring Cache abstraction with providers (like Redis, EhCache) to cache frequent queries or computed results. Apply correct eviction and TTL policies. For DB, use query/result caches (Hibernate 2nd level, manual key-value) to minimize DB hits. Place caches thoughtfully between service and data layers.

**25. Real-world scenario: If one service keeps failing in production, how will you identify whether it’s a code, infra, or DB issue?**  
Check service logs and monitoring dashboards for errors or spikes. Correlate metrics—if CPU/memory is high, suspect infra; if DB latency/connection errors, check database; if stack traces or logic errors appear, it’s likely a code issue. Use distributed tracing and log correlation to pinpoint the fault domain across the stack.

***


