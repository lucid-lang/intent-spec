# Lucid: Intention Specification

**Lucid** is a programming language that allows both dynamic and static typing, immutable data structures, atom-like state management, modern concurrency mechanisms, and seamless integration of S-expressions with C-like syntax. It transpiles directly to PHP, enabling execution within the PHP ecosystem.
(I am not sure if guaranteeing interop is worth the effort.)

#### Core Features

1. **Data Structures**
   - **Maps**: Key-value collections supporting heterogeneous types.
     ```swift
     let user = {:name "Alice",
                 :age 30,
                 :preferences ["reading", "gaming"]}
     ```
   - **Vectors**: Ordered, indexable collections with mixed types.
     ```swift
     let numbers = [1, 2, 3, "apple", "mango"]
     ```

2. **Immutability and Persistence**
   - **Immutable by Default**: Data structures are immutable unless explicitly mutable.
   - **Persistent Structures**: Modifications generate new instances efficiently without full duplication.
     ```swift
     let updatedUser = user.assoc(:age, 31)
     ```

3. **Typing System**
   - **Dynamic Typing**: Define functions and variables without type annotations.
     ```swift
     func greet(name) {
         return "Hello, " + name
     }
     ```
   - **Static Typing**: Optional type annotations for parameters and return types, enforced at compile-time.
     ```swift
     func add(a: Int, b: Int) -> Int {
         return a + b
     }
     ```
   - **Gradual Typing**: Mix dynamic and static types within the same codebase.

4. **Functional Programming**
   - **Higher-Order Functions**: Functions that accept or return other functions.
   - **Pure Functions**: Functions without side effects, ensuring predictability.
   - **Built-In Functions**: Includes `assoc`, `dissoc`, `conj`, `update`, `get` for data manipulation.
     ```swift
     let newColors = colors.conj("yellow")
     ```

5. **Concurrency Model**
   - **Atom**: Manages shared, mutable state safely.
     ```swift
     let counter = Atom({:count 0})
     counter.swap { data in
        data.assoc(:count, data["count"] + 1)
     }
     ```
   - **Thread Safety**: Immutable data structures prevent race conditions.
   - **Structured Concurrency**: Supports asynchronous operations and task management.

6. **Macro System**
   - **Metaprogramming**: Enables code generation and transformation.
     ```swift
     macro unless(condition, block) {
         return if (!condition) { block }
     }
     
     unless(user.isLoggedIn) {
         redirectToLogin()
     }
     ```

7. **REPL-Driven Development**
   - **Interactive Environment**: Facilitates real-time code evaluation, debugging, and testing.
   - **Immediate Feedback**: Supports iterative development and experimentation.

8. **Interoperability**
   - **Database Integration**: Native support for relational databases.
     ```swift
     let db = Database.connect("relational.db")
     let users = db.query("SELECT * FROM users")
     ```
   - **External Libraries**: Compatible with existing systems and tools, enabling seamless integration.

9. **S-Expressions Integration**
   - **Native Support**: S-expressions are integral to Lucid's syntax.
     ```scheme
     (define (factorial n)
         (if (<= n 1)
             1
             (* n (factorial (- n 1)))))
     ```
   - **Interoperability with Traditional Syntax**: Allows mixing S-expressions with standard code.
     ```scheme
     let greeting = "Hello, World!"
     
     (define (printGreeting)
         (print greeting))
     
     func main() {
         printGreeting()
     }
     ```
   - **Seamless Parsing**: Compiler/interpreter handles mixed S-expression and traditional syntax without errors.

10. **Transpilation to PHP**
    - **Compilation Process**: Transpiles Lucid source code into PHP code for execution within the PHP runtime.
    - **PHP Integration**: Leverages PHP's ecosystem, including libraries and frameworks.
    - **Interoperability**: Allows use of PHP functions and classes within Lucid programs.
    - **Performance Optimization**: Transpilation includes optimizations to ensure efficient PHP output.
    - **Error Handling**: Maps Lucid's type and runtime errors to corresponding PHP exceptions.
    - **Deployment**: Generated PHP code deploys on standard PHP servers.

    **Example Transpilation:**
    - **Lucid Code:**
      ```swift
      func factorial(n: Int) -> Int {
          if (n <= 1) {
              return 1
          } else {
              return n * factorial(n - 1)
          }
      }
      
      func main() {
          let result = factorial(5)
          print(result)  // Outputs: 120
      }
      
      main()
      ```
    - **Transpiled PHP Code:**
      ```php
      <?php
      function factorial(int $n): int {
          if ($n <= 1) {
              return 1;
          } else {
              return $n * factorial($n - 1);
          }
      }
      
      function main() {
          $result = factorial(5);
          echo $result;  // Outputs: 120
      }
      
      main();
      ?>
      ```

#### Syntax and Usage

1. **Defining Data Structures**
   - **HashMap**:
     ```swift
     let product = {:id 101, :name "Laptop", :price 999.99, :tags ["electronics", "computers"]}
     ```
   - **Vector**:
     ```swift
     let colors = ["red", "green", "blue", 42]
     ```

2. **Function Definitions**
   - **Dynamic Typing**:
     ```swift
     func concatenate(a, b) {
         return a + b
     }
     ```
   - **Static Typing**:
     ```swift
     func multiply(a: Int, b: Int) -> Int {
         return a * b
     }
     ```

3. **Data Manipulation**
   - **Add Key-Value Pair**:
     ```swift
     let updatedProduct = product.assoc(:stock, 50)
     ```
   - **Remove Key**:
     ```swift
     let simplifiedProduct = product.dissoc(:tags)
     ```
   - **Append to Vector**:
     ```swift
     let newColors = colors.conj("yellow")
     ```

4. **Concurrency Operations**
   - **Atomic State Management**:
     ```swift
     let sharedState = Atom({:balance 1000})
     
     func deposit(amount: Int) {
         sharedState.swap { data in data.assoc(:balance, data["balance"] + amount) }
     }
     
     func withdraw(amount: Int) {
         sharedState.swap { data in data.assoc(:balance, data["balance"] - amount) }
     }
     ```

5. **Macro Usage**
   - **Conditional Macro**:
     ```swift
     macro unless(condition, block) {
         return if (!condition) { block }
     }
     
     unless(user.isLoggedIn) {
         redirectToLogin()
     }
     ```

6. **S-Expressions Usage**
   - **Defining Functions with S-Expressions**:
     ```scheme
     (define (square x)
         (* x x))
     
     let result = square(5)
     print(result)  // Outputs: 25
     ```
   - **Mixing with Traditional Syntax**:
     ```scheme
     let message = "Factorial of 5 is: "
     
     (define (factorial n)
         (if (<= n 1)
             1
             (* n (factorial (- n 1)))))
     
     func main() {
         let fact = factorial(5)
         print(message + fact)
     }
     ```
