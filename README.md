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
```
public synchronized void increment() {
    c++;
}
```
```
private Object mutex = new Object();
synchronized (mutex) {
  // do stuff
}
```
- data-oriented
  - Records, sealed interfaces, pattern matching

# Impala
SQL ingestion of json file with an array of `eventOriginal.fills`
```sql
SELECT 
  CAST(eventOriginal.id AS BIGINT) AS `order_id`,
  eventOriginal.version AS `order_version`,
  enrichedData.orderSource AS `order_source`,
  fill.id AS `fill_id`,
  fill.traderId AS `trader_id`,
  fill.contractFill.id AS `contract_fill_id`,
  fill.contractFill.price AS `contract_fill_price`,
  fill.contractFill.arbPrice AS `contract_fill_arb_price`,
  fill.contractFill.volume AS `contract_fill_volume`,
  fill.contractFill.venueMic AS `contract_fill_venue_mic`,
  CAST(${batch_id} AS BIGINT) AS batch_id,
  CAST(date_format(current_timestamp(), 'yyyyMMddHHmmss') AS BIGINT) AS `cdh_ingestion_time`,
  element_at(split(input_file_name(), '/'), -1) AS filename,
  CAST(${load_date} as INT) AS load_date
FROM 
  cgm_marx_felix_frost_orders_raw
LATERAL VIEW OUTER
  explode(eventOriginal.fills) AS fill
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
