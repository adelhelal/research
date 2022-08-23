# Object Oriented Programming

### SOLID Principle
- Single Responsibility principle - e.g. customer class shouldn't use logging functionality
- Open/closed principle - e.g. conditions of a type shouldn't be updated in another class
- Liskov substitution principle - e.g. implement interface instead of inheritance if necessary
- Interface segregation principle - e.g. Implement multiple interfaces, instead of empty functions
- Dependency inversion principle - e.g. constructor passes in interface of ILogger

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

# Programming Paradigms

- strongly-typed vs weakly-typed
  - strongly-typed = "1" + 2 will result in a type error
  - weakly-typed = "1" + 2 = "12" - "type-coercion" (implicit conversion of data types)
- static type checking (compile time) vs dynamic type checking (runtime)
- Imperative Programming (OOP)

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
