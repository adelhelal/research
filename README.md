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

# Machine Learning
### Steps
1. Import data
2. Clean data (duplicates, irrelevant, incomplete)
3. Split data (training / testing)
4. Create model (create algorithm to analyze data)
5. Train model
6. Make prediction
7. Evaluate and improve
