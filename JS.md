# JS Prep

You should practice making these your own and come up with a couple of your own examples. Focus on explaining the examples and/or making your own analogies. Worry less about ‘memorizing’ 💩.

## Explan Pure Functions

A pure function is a fundamental concept in functional programming. Here are the defining characteristics of a pure function:

1. **Deterministic**: For a given set of inputs, a pure function always returns the same output. This means if you call a pure function with the same arguments multiple times, you'll always get the same result.

2. **No Side Effects**: A pure function does not have any side effects. It doesn't modify any external state or variables, and it doesn't perform observable actions like making a network request, writing to a file, or modifying global variables. All it does is take its input values, compute a result, and return it.

Given these characteristics, pure functions have several benefits:

- **Predictability**: Because they are deterministic and have no side effects, pure functions are predictable. This makes them easier to test, debug, and understand.

- **Reusability**: Pure functions are often general-purpose and can be reused in different contexts without fear of unintended consequences.

- **Concurrency**: Pure functions are inherently thread-safe. Since they don't modify any external state, multiple threads can execute them concurrently without any race conditions.

- **Refactorability**: Pure functions can be rearranged, combined, or separated without affecting the overall behavior of a program.

Here's a simple example to illustrate the concept:

**Pure Function**:
```python
def add(a, b):
    return a + b
```
Every time you call `add(2, 3)`, you will always get `5`, and there are no side effects.

**Impure Function**:
```python
total = 0

def add_to_total(a):
    global total
    total += a
    return total
```
In this case, the function `add_to_total` is not pure because it modifies the global variable `total`.

In summary, pure functions are a building block of functional programming that promote predictability, reusability, and maintainability in code.

## Explain Closures with an Example

Note: You should always be providing some type of example and/or analogy anyway. Don't just give memorized words.

```javascript
// The outer function
const createGetNameFxn = (name) => {
  // The inner function
  return () => {
    // The outer function parameter is ENCLOSED in the inner function
    // In other words, the inner function maintains a reference
    // to the outer function's scope
    return name;
  };
};

const sayJim = createGetNameFxn("Jim");
const sayBob = createGetNameFxn("Bob");

console.log(sayJim()); // Jim
console.log(sayBob()); // Bob

// An incoming string is bound to `name`
const sayName = (name) => {
  return name;

  // Whatever data was bound to `name` is GARBAGE COLLECTED...
};

sayName("Jim"); // Jim
sayName("Bob"); // Bob
```

This code demonstrates the concept of closures in JavaScript. Let me break it down for you:

1. **Closures:** 
    - A closure is a function that has access to the outer (enclosing) function's variables—scope chain. This means that an inner function always has access to the variables and parameters of its outer function, even after the outer function has completed its execution. 
    - The function `createGetNameFxn` is an outer function that accepts a `name` parameter. It returns an inner function that, when invoked, returns the value of the `name` parameter. 
    - When you call `createGetNameFxn("Jim")`, it returns a function. When you invoke that returned function (via `sayJim()`), it still has access to the `name` parameter from its outer scope (`createGetNameFxn`), and thus it logs "Jim".
    - Similarly, `sayBob()` logs "Bob".

2. **Garbage Collection:** 
    - The comment in the `sayName` function implies that once the function's execution is complete, any data that was bound to its parameters or variables will be eligible for garbage collection (unless referenced elsewhere). This is true for most programming languages and not just specific to JavaScript. 
    - In this case, once `sayName("Jim")` is executed, the string "Jim" is no longer referenced by the program and can be garbage collected.

## Does React Use Closures? If So, How?

React, especially when combined with the Hooks API introduced in React 16.8, makes extensive use of closures. Here's how closures play a role in React and its ecosystem:

1. **useState Hook:**
    - Consider the `useState` hook. When you call it, you get back a piece of state and a function to update that state. The update function is a closure over the component's current render, allowing you to set state based on the current value.
    ```javascript
    const [count, setCount] = useState(0);
    const increment = () => setCount(count + 1);
    ```
    In the above snippet, `increment` is a closure over the `count` value.

2. **useEffect Hook:**
    - The `useEffect` hook often uses closures to access the component's props, state, or context. When you define an effect, it captures the values from the render in which it's defined.
    ```javascript
    useEffect(() => {
        console.log(count); // This count value is from the render when this effect was last defined or updated.
    }, [count]);
    ```

3. **Event Handlers:**
    - Event handlers in React components often utilize closures. When you define an event handler inside a component, it can access the component's props, state, or other variables from that render.
    ```javascript
    function MyComponent() {
        const [message, setMessage] = useState("Hello");

        const handleClick = () => {
            alert(message); // This is a closure over the `message` state.
        };

        return <button onClick={handleClick}>Click me</button>;
    }
    ```

4. **Custom Hooks:**
    - Custom hooks often return functions that are closures over the internal state or logic of the hook. This allows consumers of the hook to interact with its internal state without directly accessing it.

5. **React's Render Mechanism:**
    - React's reconciliation and rendering mechanism also makes use of closures, especially when it comes to rendering child components and preserving state and props across renders.

6. **Function Components Themselves:**
    - Every time a function component renders, it effectively creates a new closure, capturing the current props, state, and context.

In conclusion, closures are fundamental to the way React works and allow for the powerful and flexible patterns that developers have come to appreciate in React. The encapsulation and preservation of state and logic across re-renders and interactions are deeply intertwined with the concept of closures in JavaScript.


## What Does It Mean that JS Has 'First-Class' Functions?

When we say that a programming language has "first-class functions", it means that functions in that language are treated as first-class citizens. This implies the following:

1. **Functions as Values**: Functions can be assigned to variables, stored in data structures, passed as arguments to other functions, and returned from other functions just like any other values (e.g., numbers, strings).

   ```javascript
   // JavaScript example
   const greet = function(name) {
       return "Hello, " + name;
   }
   ```

2. **Functions as Arguments**: You can pass functions as arguments to other functions.

   ```javascript
   // JavaScript example
   const runFunction = function(fn, value) {
       return fn(value);
   }
   runFunction(greet, "Alice");  // returns "Hello, Alice"
   ```

3. **Functions as Return Values**: Functions can return other functions.

   ```javascript
   // JavaScript example
   const multiplier = function(factor) {
       return function(x) {
           return x * factor;
       }
   }
   const double = multiplier(2);
   double(5);  // returns 10
   ```

4. **Anonymous Functions**: The language allows for the creation of functions that don't necessarily have a name (often referred to as lambda functions or anonymous functions).

   ```javascript
   // JavaScript example
   const numbers = [1, 2, 3];
   numbers.map(function(num) {
       return num * 2;
   });  // returns [2, 4, 6]
   ```

5. **Closure**: Functions can close over variables from their outer scope, retaining access to those variables even after the outer function has finished executing.

   ```javascript
   // JavaScript example
   const createCounter = function() {
       let count = 0;
       return function() {
           return count++;
       }
   }
   const counter = createCounter();
   counter();  // returns 0
   counter();  // returns 1
   ```

Not all programming languages have first-class functions, but many modern languages, like JavaScript, Python, and Haskell, do. The presence of first-class functions often allows for more flexible and expressive programming patterns, such as functional programming.

---

Here's an example of some legacy Java code that doesn't have first-class functions:

```java
// Define an interface
public interface Greeting {
    void sayHello(String name);
}

public class Main {
    public static void main(String[] args) {
        // Create an anonymous inner class to implement the Greeting interface
        Greeting greetEnglish = new Greeting() {
            @Override
            public void sayHello(String name) {
                System.out.println("Hello, " + name);
            }
        };
        
        greetEnglish.sayHello("Alice");
        
        // But note: we're still not really passing around "functions" in the way that 
        // languages with first-class functions do. We're working with objects.
    }
}
```

This approach is more verbose and lacks the flexibility and expressiveness of true first-class functions. With the introduction of Java 8, Java introduced lambdas and functional interfaces which provide capabilities similar to first-class functions. However, it's still a bit different in feel and implementation compared to languages where functions have always been first-class citizens, like JavaScript or Python.

## Discuss JS' Data Types

[ChatGPT Conversation](https://chat.openai.com/share/34070283-ecdc-49c2-a8e7-f7a04a2b1da7)

## Discuss the Module Systems Within JS (e.g. CommonJS vs ESM)

[ChatGPT Conversation](https://chat.openai.com/share/1b5d857e-1266-4a3c-b950-2b766f46c253)
