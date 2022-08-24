# Object Oriented Programming

### SOLID Principle

- _Single Responsibility_ principle - e.g. customer class shouldn't use logging functionality
- _Open/closed principle_ - e.g. conditions of a type shouldn't be updated in another class
- _Liskov substitution principle_ - e.g. implement interface instead of inheritance if necessary
- _Interface segregation principle_ - e.g. Implement multiple interfaces, instead of empty functions
- _Dependency inversion principle_ - e.g. constructor passes in interface of ILogger

### Concepts

- Encapsulation - Private Methods
- Inheritance - `Class : BaseClass, Interface`
- Polymorphism
  - Method Overriding (run-time) or Method Overloading (compile-time);
  - Assign instance of Derived to a variable of type Base
- Abstraction - Abstract Classes
- Aggregation - child class can exist independently of the parent class
- Composition - child class cannot exist independently of the parent class
- Composibility - reusable components/functions/libraries
- Coupling - A class that relies on another class so changes affect both
- Cohesion
  - High cohesion a class is focused on what it should be doing
  - Low cohesion a class does a great variety of actions, broad

### Design Patterns

- _Singleton_ - One single instance of an object (Singleton.Instance)
- _Command_ - Undo / Redo the collection of commands in an object
- _Composite_ - Tree structure using Leaf, Parent, Root nodes
- _Proxy_ - One class uses all functionality of the real class.
- _Abstract Factory_ - Client class providing all interaction & functionality of abstract class without knowledge of deriving classes (concrete classes)
- _Method Factory_ - same as Abstract Factory but Main() Subclass instantiates and uses functionality

* Inversion of Control (IoC)
  * Dependency Injection
    * Registration of service instances based on contracts (interfaces) at bootstrap
    * Identify the services listed in the constructor’s signature by a name mapping
    * Lookup service in Service Factory Repository (IoC Container), instantiate if null
    * Provide instance of service through constructor
    * e.g. AutoFac, StructureMap, Ninject, Unity Application Block, Spring.NET, CastleWindsor
  * Events and Delegates
  * Observer Pattern (Push-based notification IObserver<T> & IObservable<T> provider)
  * Service Locator (HttpContext.ApplicationServices.GetService(typeof(ISomeService)))

- Map/Reduce (NoSQL Data retrieval)
  - Map uses Views to build Queries, allowing for distributed query processing 
  - Reduce takes result of Map query, and aggregates data as results

* Saga Pattern
  * Uses compensation instead of ACID rollback
  * e.g. Book flight -> Hotel reservation -> cancel reservation -> cancel flight

- Sidecar pattern / sidekick pattern
  - attached to a parent application and provides supporting features for the application
  - separate process or container to provide isolation and encapsulation

* UI Patterns - MVC & MVVM (Silverlight) & MVP (Model View Presenter)

- Domain-Driven Design (Bounded Contexts - Single Responsibility Principle) [Eric Evans]
  - Entities (The “nouns” with Identity and Lifecycle)
    - Infrastructure Ignorance - Persistence and UI Ignorance
    - A uniquely identifiable object (`IEquatable<T>`) (Readonly identity property)
    - Control of access to children objects within the aggregate
    - e.g. `Customer.AddOrder(theOrder)` NOT `Customer.Orders.Add(theOrder)`
    - e.g. `Order.CanBeAddedToCustomer` NOT `Order.Item.Count() == 0`
    - Behaviors e.g. `Order.Cancel()` NOT `Order.Status == Status.Cancel`
  - Value Objects (Descriptors or properties of primitive properties)
    - Intrinsic value represents its uniqueness (property by property comparison)
    - fields are immutable
    - e.g. `public class Customer { public Name FullName { get; set; } }`
    - where `public class Name { public string FirstName { get; set; } public string LastName { get; set; } }`
  - Aggregate Roots (Combining Entities and holding references) - “Parent”
  - Domain Services (Operations or Processes, stateless and cohesive)
    - Behaviours that don't belong to existing Entities
  - Repositories (In-Memory Collections)
    - `CustomerRepository.GetById(int Id)` is a data-retrieval action (`PersistenceRepository<Customer>`)
    - `GetCustomer(Customer customer)` is a domain-centric action (`ICustomerRepository`)
    - Shouldn't return `IQueryable<Customer> GetCustomers()`
    - Divide repository/API of same Type rather than all api calls in one
    - `ICustomerRepository { RemoveCustomer(Customer customer), AddCustomer(Customer customer) }`
      - found in the Domain project as the "shape", but the implementation exists in Infrastructure project)
    - e.g. `public class Customers : PersistenceRepository<Customer>, ICustomerRepository`
  - Validation
    - Persistence Rule (NOT NULL column) vs Business Rule (ship order for valid customers under credit limit)
    - Validation for Value Objects should exist in its constructor. No need to validate anywhere else because of immutability.
    - Validation for multiple entities could be looked at behaviorally (e.g. `order.CanShipTo(customer)` or `customer.CanShip(order)`)
    - Validation can be separated into Validator service (e.g. `shippingValidator.CanShip(customer, order)`)
  - Anti-patterns
    - Don't suffix layers e.g. `CustomerRepository` or `CustomerEntity` or `CustomerService`
    - Don't use repository like DAL e.g. `repository.Save()`, `repository.Get()`, `repository.Load()`
    - Don't separate Data and Behaviour

# Programming Paradigms

- Imperative Programming (OOP)
- Strongly-typed - "1" + 2 will result in a type error
- Weakly-typed = "1" + 2 = "12" - "type-coercion" (implicit conversion of data types)
- Static type checking (compile time)
- Dynamic type checking (runtime)

# Architecture

- CQRS (Command Query Responsibility Segregation)
  - One model to read information, another one to update information.
- Clean Architecture
  - Framework & Drivers (Web | Devices | DB | UI)
    - Interface Adapters (Controllers | Presenters | Gateways)
      - Application Business Rules (Use Cases)
        - Enterprise Business Rules (Entities)

# Distributed Systems
- CAP Theorem (NoSql trade-offs)
  - Consistency (all nodes return the same data) most recent write guaranteed
    - MongoDB, Redis, HBase
  - Availability (request receives response, success or fail) most recent write not guaranteed
    - Cassandra, DynamoDB
  - Partition Tolerance (system operates despite arbitrary partitioning due to network failures)

* Four Primitives / Categories
  * Lifecycle - Packaging, Healthcheck, Deployment, Rollback, Scaling, Configuration
    * Kubernetes, Knative
  * Networking - Service discovery, Load balancing, Retry, Timeout, Circuit-breakers, Rate limiting, Observability
    * Service mesh (Istio), Consul, Envoy, Dapr, Azure Service Fabric
    * Rate limiting
      * Concurrency limit - resources limited at any given time 
      * Token bucket limit - availability of resources after a given interval
      * Fixed window limit - number of resources for a given time before time reset
      * Sliding window limit - number of resources for a given time from now
  * Binding - Message transformation, Message routing, Protocol conversion, Pub/Sub
    * Camel-K
  * State - Workflow management, Caching, Idempotency, Application state, Scheduling, Transactionality (SAGA)
    * AWS Step Functions, Redis

![Distributed Systems](resources/distributed.systems.png)

- Eight Fallacies
  1. The network is reliable
  2. Latency is zero
  3. Bandwidth is infinite
  4. The network is secure
  5. Topology doesn't change
  6. There is one administrator
  7. Transport cost is zero
  8. The network is homogeneous

* Service Mesh (Networking)
  * moving networking-related concerns from the service containing the business logic, outside and into a separate runtime
  * advanced release strategies, advanced routing, managing security, metrics, tracing, recovery from errors, simulating errors without touching the service, application-specific protocols

- Service Mesh evolution

![Service Mesh](resources/service.mesh.png)

# Javascript

### Web Workers

```javascript
// main.js
const worker = new Worker('worker.js');
worker.postMessage([5, 4]);

worker.onmessage = function(e) {
  console.log(e.data); // prints 9
}

// worker.js
onmessage = function(e) {
  const result = e.data[0] + e.data[1];
  postMessage(result);
}
```

# Impala

Ingestion of json file with an array of `order.items` into s3 parquet file

```sql
steps:
  - dataFrameName: orders_dataframe
    sql: 
      SELECT 
        CAST(order.id AS BIGINT) AS `order_id`,
        order.version AS `order_version`,
        order.source AS `order_source`,
        item.id AS `item_id`,
        123 AS `batch_id`,
        CAST(date_format(current_timestamp(), 'yyyyMMddHHmmss') AS BIGINT) AS `process_time`
      FROM 
        order_raw
      LATERAL VIEW OUTER
        explode(order.items) AS item

output:
  - dataFrameName: orders_dataframe
      outputType: File
      format: parquet
      outputOptions:
        saveMode: Append
        path: s3_file_path
        protectFromEmptyOutput: false
        partitionBy:
          - batch_id
          - process_time
```
Import from parquet file into table
```sql

DROP TABLE if exists orders;
CREATE EXTERNAL TABLE IF NOT EXISTS orders (
    `order_id` STRING,
    `order_version` INT,
    `order_source` STRING,
    `item_id` INT
) PARTITIONED BY (batch_id BIGINT, process_time BIGINT)
STORED AS PARQUET LOCATION 's3a://s3_bucket/s3_file_path';
```

# Machine Learning

### Steps
1. Import data
2. Clean data (duplicates, irrelevant, incomplete)
3. Split data (training / testing)
4. Create model (create algorithm to analyze data)
5. Train model
6. Make prediction
7. Evaluate and improve

# Quantum Programming

* Q# (.NET quantum programming framework)
* Cirq (Google open source quantum programming framework)
