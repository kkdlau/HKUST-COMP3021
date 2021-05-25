# Java Programming - Exception Handling and File I/O

[TOC]

## Exception Handling

* Java focus programmers to add exception handling
  * otherwise compiler considers that are errors
* Inside a catch block, **a new exception can be thrown**

### Syntax

```java
try {
  if (<condition>)
    throw new <exception_name>(<args>);
} catch (<error>) {

  return; // optinally, even if there is "return", the finally is still executed
} finally {
  // statements to be executed regardless of an exception is thrown or not
}
```

* Exception is an object instance
* If the program exits before the finally block, then the finally block won't be executed

## Exception Throwing with Method Overriding

```java
class A {
    Number test() throws Exception {
      return null;
    }
    
    Number test2() throws IOException {
      return null;
    }
}

class B extends A {
    Double test() throws IOException {
      return null;
    }
    
    // error, Exception is not a subtype of IOException
    Double test2() throws Exception, NullPointerException {
      return null;
    }
    
    // no error, you can add new exception!
    Double test2() throws NullPointerException { 
      return null;
    }
}
```

* Same as return type, exception can only be casted to a more specific type



## Nested Exception Throwing in Catch block

* Exception can be thrown inside a catch block
  * in that case, the original `try catch` wont handle it
  * need to add `throws` in the method signature
* When the nested exception is thrown, the `finally` block is called instantly

:::success
**Example:**
```java
class Test {
    static void idk() throws NullPointerException {
        throw new NullPointerException();
    }

    static void idk2() throws IOException {
        try {
            idk();
        } catch (NullPointerException e) {
            System.out.println("catch 1");
            throw new IOException();
        } finally {
            System.out.println("finally 1");
        }
    }
}
class Main {

    public static void main(String[] args) throws Exception {
        try {
            Test.idk2();
        } catch (IOException e) {
            System.out.println("catch 2");
        } finally {
            System.out.println("finally 2");

        }
    }
}
```
**Output:**
```sh
catch 1
finally 1 // nested exception is thrown, finally block is called instantly
catch 2
finally 2
```
:::

## Unchecked Exception vs Checked Exception

* Unchecked Exception: no need to implement try catch
  * All unchecked exceptions are subclass of `RuntimeException`
  * Exceptions will be handled by JVM
  * Usually are runtime error
  * Can be avoid if you write the code very careful
* Checked Exception: Classes other than `runtimeException`
  * Usually are errors that not under programmer control
  * If the exceptions are not be handled, compiler error raises


:::info
In general, Java compiler only let you to compile iff you handle the exceptions using `try...catch` / `throws`.
:::

:::success
**Q: Do you need to handle `Exception` if you call the following method?**
```java
void test() throws Exception {
    throw new NullPointerException();
}
```
A: The answer is **yes**, because `Exception` is not a subclass of `RuntimeException`. You can always use `e instanceof RuntimeException` to check is the exception unchecked or not. 


**Q: Which line will be printed?**
```java
try {
    test();
} catch (NullPointerException e) {
    System.out.println("catch nullptr");
} catch (Exception e) {
    System.out.println("catch e");
}
```
A: `catch nullptr`. Although in the method signature, it only says `Exception` will be thrown, but still, JVM **runtime determines** which catch block should be called. This also implies that generic types are not allowed for catch block parameter, **because JVM cannot determine the generic type in runtime**.
:::

## Multi-catch

* The most general catch block should be put as the last


## File I/O

* PrintWriter - `print()`: all data is actaully stored in a buffer
  * If `close()` doesnt be called, it is possible that the data will be lost

## Omitting Catch block / Finally block

* A try block must come up with a `catch` / `try` block
* If you are handling with resources
  * then you can omit finally block
  * because Java will add it for you


:::info
**Example:**

* no error, because a try block comes up with a catch block
```java
public static void main(String[] args) {
    try {

    } catch (Exception e) {

    }
}
```

* no error, because a try block comes up with a finally block
```java
public static void main(String[] args) {
    try {

    } finally {

    }
}
```

* error, must come up with at least a catch / finally block
```java
public static void main(String[] args) {
    try {

    }
}
```

* error, exception caused `Scanner sc = new Scanner(new File(""));` by must be handled
```java
public static void main(String[] args) {
    try (Scanner sc = new Scanner(new File(""));){

    }
}
```

* no error, exception handling is shifted to callee
* Also, an implicit finally block is added because of auto resource release
```java
public static void main(String[] args) throws FileNotFoundException {
    try (Scanner sc = new Scanner(new File(""));){

    }
}
```
:::