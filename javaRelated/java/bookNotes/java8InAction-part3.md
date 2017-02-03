# Java 8 in Action part 2

[source code]()

## Chapter 13 Thinking functionally

### Functional vs Declarative
  - Immuatbility
    A method, which modifies neither the state of its enclosing class nor the state of any other objects and returns its entire results using return, is called pure or side-effect free.
    An immutable object is an object that can’t change its state after it’s instantiated so it can’t be affected by the actions of a function.
  - Declarative programming (basis for Functional programmin). What not HOW (no ssignment, conditional branching, and loops)
    eg. `Optional<Transaction> mostExpensive = transactions.stream().max(comparing(Transaction::getValue));`
  - Whats a Function?: method with NO side effects (or at least none exposed to the system).

### Functional Style
  - Our guideline is that to be regarded as functional style, a function or method can mutate only local variables. In addition, objects it references should be immutable. By this we mean all fields are final, and all fields of reference type refer transitively to other immutable objects.
  - To be regarded as functional style, a function or method **shouldn’t throw any exceptions**. USE OPTIONAl instead.
    Might decide to use exceptions internally BUT NOT expose them via public api - use Optional.        
  - Referential transparency:  “no visible side-effects” (no mutating structure visible to callers, no I/O, no exceptions) encode the concept of referential transparency. A function is referentially transparent if it always returns the same result value when called with the same argument value. 

### Functional style in Practice
  - No mutation
    eg. p374 of finding all combinations of a list {1,4,9}. There are 8 including empty list {}
    The approach is very much scala - take 1st element & create **new lists (no mutation)** to concatenate, then recursively call.

### Recursion
    - eg. Factorial: `return n == 1 ? 1 : n * factorialRecursive(n-1);`
      Stream Factorial: `return LongStream.rangeClosed(1, n).reduce(1, (long a, long b) -> a * b);`
    - Tail Recursion eg. `return n == 1 ? acc : factorialHelper(acc * n, n-1);`
    
## Chapter 14 Functional programming techniques

### Functions everywhere
  `Function<String, Integer> strToInt = Integer::parseInt;`
  - Higher-Order
    `Comparator<Apple> c = comparing(Apple::getWeight);` takes a fn and returns a fn.
  - Currying: a technique that can help you modularize functions and reuse code. **Pass a subset of params, return fn**
    `static double converter(double x, double f, double b) {return x * f + b;}` can be made more useful
    `static DoubleUnaryOperator curriedConverter(double f, double b){return (double x) -> x * f + b;}` returning fn
    you can then define lots of diffconversions
      `DoubleUnaryOperator convertCtoF = curriedConverter(9.0/5, 32); DoubleUnaryOperator convertUSDtoGBP = curriedConverter(0.6, 0);`
      and run `double gbp = convertUSDtoGBP.applyAsDouble(1000);`
  - Persistent data structures
    