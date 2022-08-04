# Programming Paradigms
- strongly-typed vs weakly-typed
  - strongly-typed = "1" + 2 will result in a type error
  - weakly-typed = "1" + 2 = "12" - "type-coercion" (implicit conversion of data types)
- static type checking (compile time) vs dynamic type checking (runtime)

# Java
- wildcard generics
  - `ArrayList<? extends Number>` allows any object that extends Number to fill the ArrayList
  - `ArrayList<? super Integer>` allows any object that is the super class of `Integer` i.e. `Number` or `Object`
- streams
  - intermediate (deferred executions) and terminal (action)
  - method reference operator `stream.forEach(System.out::println)` instead of `stream.forEach(s -> System.out::println(s))`
- synchronized - can only be invoked on one thread at a time
```java
public synchronized void increment() {
    c++;
}
```
```java
private Object mutex = new Object();
synchronized (mutex) {
  // do stuff
}
```
- data-oriented
  - Records, sealed interfaces, pattern matching

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
Ingestion of json file with an array of `eventOriginal.fills` into s3 parquet file
```sql
steps:
  - dataFrameName: cgm_marx_felix_frost_order_fills
    sql: 
      SELECT 
        CAST(eventOriginal.id AS BIGINT) AS `order_id`,
        eventOriginal.version AS `order_version`,
        enrichedData.orderSource AS `order_source`,
        fill.id AS `fill_id`,
        fill.traderId AS `trader_id`,
        fill.contractFill.id AS `contract_fill_id`,
        fill.contractFill.price AS `contract_fill_price`,
        fill.contractFill.volume AS `contract_fill_volume`,
        CAST(${batch_id} AS BIGINT) AS batch_id,
        CAST(date_format(current_timestamp(), 'yyyyMMddHHmmss') AS BIGINT) AS `cdh_ingestion_time`,
        element_at(split(input_file_name(), '/'), -1) AS filename
      FROM 
        cgm_marx_felix_frost_orders_raw
      LATERAL VIEW OUTER
        explode(eventOriginal.fills) AS fill

output:
- dataFrameName: cgm_marx_felix_frost_order_fills
    outputType: File
    format: parquet
    outputOptions:
      saveMode: Append
      path: s3_file_path
      protectFromEmptyOutput: false
      partitionBy:
        - batch_id
        - cdh_ingestion_time
```
Import from parquet file into table
```sql

DROP TABLE if exists cgm_${db_ext}.order_fills;
CREATE EXTERNAL TABLE IF NOT EXISTS cgm_${db_ext}.order_fills (
    `order_id` STRING,
    `order_version` INT,
    `order_source` STRING,
    `fill_id` INT,
    `trader_id` STRING,
    `contract_fill_id` INT,
    `contract_fill_price` INT,
    `contract_fill_volume` INT)
    PARTITIONED BY (batch_id BIGINT, cdh_ingestion_time BIGINT, load_date INT)
    STORED AS PARQUET LOCATION 's3a://${s3BasePath}/s3_file_path';
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
