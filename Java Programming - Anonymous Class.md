# Java Programming - Inner Class & Anonymous Inner Class

[TOC]

## Introduction to Inner Class

* can reference to outer class **without passing reference**
* Inner class is compiled independently as the following:
  ```java
  <outer_class_name>$<inner_class_name>.class
  ```
* Inner class can be either `public`, `private`, `protected` or `static`

## Static Inner Class

* an inner class can be either `static`, `public`, `private`, and `abstract`
* a static inner class means **its context is static**
  * so it cannot access outer class instance variable / methods
* Static class **behaves just like a normal class**, so it can have constructor, methods and other isntance variables

## Creating an inner class instance

* Merthod 1: Create an inner instance using overriden `new` operator
```java
class Outer {
  class Inner {
    // detail skipped
  }
  
  
  public static void main() {
    Outer o = new Outer();
    Inner i = o.new Inner();
  }
}
```

* Method 2: directly create a static inner class

```java
class Outer {
    static class Inner {
        Inner() {
        }
    }
}
Inner in = new Outer.Inner();
```

### Accessing `this` of outer class

Recall the above example, to access `this` of outer class:
* `<outer_class>.this`: A special reference that references to outer class's `this`

```java
class A {
    int a;
    class B {
        B() {
            A.this.a = 10;
        }
    }
}
```

## Anonymous Inner Class

* An inner class extends a superclass or implement an interface
  * but cannot extends / implements others explicitly
* All abstract methods must be implemented
  * must at least override a method
* Best matched Super constructor is used for Anonymous Super Class
* Object constructor is used for Anonymous Interface
* It is compiled to `OutClassName$n.class`


### Passing parameters to Anonymous Inner Class
```java
abstract class Test {
  Test(int useless) {
  
  }
}


class App {
  public static void main() {
    idk(new Test(10) { // 10 is passed to useless 
      // must override something
    })
  }
  
  static void idk(Object o) {
  
  }
}
```

:::warning
Constructor cannot be abstract, and the body must be implemented.
:::

## Lambda Expresson & Functional Interface

* Lambda Expresson: for implementing Functional Interface with shorter syntax
* Functional Interface:
  ```java
  @FunctionalInterface
  interface Action {
    void run(String param);
  }
  ```
  * Having more than more one abstract methods will be considered as compiler error

* If an interface declares an abstract method overriding one of the public methods of `java.lang.Object`, that also does not count toward the interface’s abstract method count
* A functional interface is valid even if the `@FunctionalInterface` annotation would be omitted


* The capture list of lambda expression / inner class is final, that means **all local variables are immutable**
  * if you want to modify the variables, you can use `final Object[]` to contain those variable