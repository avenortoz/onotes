
## Intro
Microservices Architecture is an approach to software development that involves breaking down a single application into a suite of small, independent services. Each service runs in its own process and communicates with other services through lightweight mechanisms, such as [[HTTP]] APIs ([[REST]], [[SOAP]]), [[gRPC]], [[RPC]], [[GraphQL]]. The architecture is built around business capabilities and allows for independent deployment of each service using automated deployment machinery. There is minimal centralized management of these services, which can be written in different programming languages and use different data storage technologies.

## Overview

**Advantages of Microservices Architecture:**
- Scalability: Microservices can be scaled independently, allowing for better resource utilization and cost savings.
- Flexibility: Each service can be developed, deployed, and updated independently, allowing for greater agility and faster time-to-market.
- Resilience: If one service fails, it does not affect the entire application, making it easier to isolate and fix issues.
- Technology Diversity: Different services can use different technologies and programming languages, allowing teams to choose the best tool for each job.
- Improved Collaboration: Smaller teams can work on individual services, leading to better collaboration and communication.

**Disadvantages of Microservices Architecture:**
- Complexity: Managing multiple services can be complex and requires additional infrastructure to ensure proper communication between services.
- Increased Overhead: Each service requires its own infrastructure, which can lead to increased overhead costs.
- Testing Challenges: Testing becomes more complex as there are more moving parts in the system.
- Distributed Systems Issues: As a distributed system, there are additional challenges around data consistency and transaction management. 
- Skillset Requirements: Teams need to have expertise in multiple technologies and programming languages.

---

## Patterns

### Decomposition patterns
Decomposition patterns are strategies for breaking down a monolithic application into smaller, more manageable services. The two decomposition patterns described:

#### Decompose by business capability
This pattern involves breaking down the application into services based on business capabilities, such as customer management or order processing.

Decompose by business capability is a decomposition pattern that involves breaking down the application into services based on business capabilities, such as customer management or order processing. Each service is responsible for a specific business capability and can be developed and deployed independently of other services. The advantage of this pattern is that it allows for greater flexibility in service design and development. Each service can be designed to reflect the specific needs of its business capability, without being constrained by the requirements of other parts of the application. This can lead to more modular and maintainable code. However, this pattern can also lead to increased complexity in service interactions and data consistency. Services may need to communicate with each other more frequently to ensure data consistency across business capabilities. Additionally, changes to one business capability may have unintended consequences for other parts of the application. Overall, the choice between decomposition patterns depends on factors such as the size and complexity of the application, as well as organizational and technical considerations.

#### Decompose by subdomain
This pattern involves breaking down the application into services based on subdomains, which are distinct parts of the overall domain model.

Decompose by subdomain is a decomposition pattern that involves breaking down the application into services based on subdomains, which are distinct parts of the overall domain model. A subdomain is a part of the business domain that has its own language, concepts, and rules. For example, in an e-commerce application, the order management subdomain might include services for processing orders, managing inventory, and handling payments. The advantage of this pattern is that it allows for greater flexibility and autonomy in service design and development. Each service can be designed to reflect the specific needs of its subdomain, without being constrained by the requirements of other parts of the application. This can lead to more modular and maintainable code. However, this pattern can also lead to increased complexity in service interactions and data consistency. Services may need to communicate with each other more frequently to ensure data consistency across subdomains. Additionally, changes to one subdomain may have unintended consequences for other parts of the application.

#### Summary
Both patterns have their advantages and disadvantages, and the choice between them depends on factors such as the size and complexity of the application, as well as organizational and technical considerations.


### Interprocess communication patterns
1. Synchronous communication: In this pattern, a client sends a request to a service and waits for a response before proceeding. This pattern is simple and easy to understand, but can lead to performance issues if requests take a long time to complete.

2. Asynchronous communication: In this pattern, a client sends a request to a service and does not wait for a response before proceeding. The service sends the response back at a later time, typically using messaging or event-driven architectures. This pattern can improve performance and scalability by allowing services to process requests in parallel.

#### Synchronous communication pattern
Easy to understand. We request something from another service, wait for response. It is easy to implement.

For example. Our application have basket service and discount service. Customer want to buy everything in the basket and apply some promo code. For the basket to calculate the final price it needs to send request to the discount service to verify promo code, and get the discount, after that it can return the final price to the client.

#### Async communication
**It is important to understand choice between chosing async communication and sync communication depends on business requirements**

In this pattern, a client sends a request to a service and does not wait for a response before proceeding. The service sends the response back at a later time, typically using messaging or event-driven architectures. This means that the client can continue to perform other tasks while waiting for the response from the service. An example of this pattern is an e-commerce website that needs to process orders. When a customer places an order, the website can use asynchronous communication to send the order details to a backend service for processing. The website does not need to wait for the processing to complete before displaying a confirmation message to the customer. Instead, it can immediately display the confirmation message and then later receive a notification from the backend service when processing is complete.

1. A customer places an order on the e-commerce website by filling out an order form and clicking "Submit".

2. The website receives the order details and sends them to a backend service for processing using asynchronous communication.

3. The website immediately displays a confirmation message to the customer, thanking them for their order and providing an estimated delivery date.

4. The backend service receives the order details and begins processing them. This may involve validating payment information, checking inventory levels, and scheduling delivery.

5. Once processing is complete, the backend service sends a notification back to the website using asynchronous communication. This notification may include information such as the status of the order (e.g., "processing", "shipped", "delivered"), tracking information, or any errors that occurred during processing.

6. The website receives the notification from the backend service and updates its display accordingly. For example, if the order has been shipped, it may display a message indicating that shipping is in progress and provide a link to track the package.

By using asynchronous communication in this way, both the customer and backend service can continue to perform other tasks while waiting for processing to complete. This can improve overall system performance and provide a better user experience for customers by reducing wait times.

### Data management patterns
1. Database per service: In this pattern, each service has its own database, which helps to ensure loose coupling between services. However, this can introduce issues with data consistency and coordination between services.

2. Saga: In this pattern, each service is responsible for its own data updates and compensating actions in case of failures. This can help to maintain data consistency without relying on distributed transactions.

3. API composition: In this pattern, a client sends a request to an API gateway, which then composes the necessary requests to multiple services to fulfill the client's request. This can help to reduce the number of round trips between the client and services and simplify client code.

4. CQRS (Command Query Responsibility Segregation): In this pattern, commands (requests that modify data) and queries (requests that retrieve data) are handled separately by different services or components within a service. This can help to improve scalability and performance by allowing each component to be optimized for its specific task.

5. Event sourcing: In this pattern, changes to application state are captured as a sequence of events rather than updating state directly in a database. This can provide an audit trail of all changes made to the system and support complex workflows involving multiple services.

#### Saga Pattern
The Saga pattern is a way to maintain data consistency in a microservice architecture without relying on distributed transactions.

In a distributed transaction, multiple services participate in a single transaction and all updates must either succeed or fail together. This can be difficult to implement and can lead to performance issues. In the Saga pattern, each service is responsible for its own data updates and compensating actions in case of failures. A saga is a sequence of local transactions that are coordinated using asynchronous messaging. Each local transaction updates data within a single service using the familiar ACID transaction frameworks and libraries. If one of the local transactions fails, the saga can use compensating actions to undo any changes made by previous transactions.

For example, if a payment service fails after deducting funds from a customer's account but before updating an order status, the saga can use a compensating action to refund the customer's account and cancel the order. The saga coordinator is responsible for coordinating the sequence of local transactions and compensating actions. It uses asynchronous messaging to communicate with each service involved in the saga. Overall, the Saga pattern provides a way to maintain data consistency without relying on distributed transactions. It allows each service to be responsible for its own data updates and provides fault tolerance through compensating actions. However, it does require careful design and implementation to ensure that sagas are executed correctly and efficiently.

#### Event sourcing pattern

### Testing patterns

### Security patterns

### Deployment patterns

## Links