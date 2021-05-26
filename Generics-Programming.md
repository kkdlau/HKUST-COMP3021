# Java Programming - Generics

[![hackmd-github-sync-badge](https://hackmd.io/JOWpSyv5TJWWBp0-Y1SV4w/badge)](https://hackmd.io/JOWpSyv5TJWWBp0-Y1SV4w)


## Some Interesting from Java Official

**Q: will the following code compile?**
```java
public final class Algorithm {
    public static <T> T max(T x, T y) {
        return x > y ? x : y;
    }
}
```

A: No, because `>` operator only works on primitive type. Using Generics means `T` must be a class.

Q: What is the converted code after type erasure?

```java
public static <T extends Comparable<T>>
    int findFirstGreaterThan(T[] at, T elem) {
    // ...
}

public class Pair<K, V> {

    public Pair(K key, V value) {
        this.key = key;
        this.value = value;
    }

    public K getKey(); { return key; }
    public V getValue(); { return value; }

    public void setKey(K key)     { this.key = key; }
    public void setValue(V value) { this.value = value; }

    private K key;
    private V value;
}
```

A:
```java
public static int findFirstGreaterThan(Comparable[] at, Comparable elem) {
    // ...
}

public class Pair {

    public Pair(Object key, Object value) {
        this.key = key;
        this.value = value;
    }

    public Object getKey()   { return key; }
    public Object getValue() { return value; }

    public void setKey(Object key)     { this.key = key; }
    public void setValue(Object value) { this.value = value; }

    private Object key;
    private Object value;
}
```

Q: Will the following code compile?

```java
class Node<T> implements Comparable<T> {
    public int compareTo(T obj) { /* ... */ }
    // ...
}

class Main {
    static public void main(String[] args) {
        Node<String> node = new Node<>();
        Comparable<Object> comp = node;
    }
}
```

A: No, only if `Comparable<Object>` is changed to `Comparable<String>`, then it will compile.

## Bounded Generic Type

  ```java
  public static <E> boolean equalArea(E object1, E object2) {
    // Object don't have getArea()
    return object1.getArea() == object2.getArea();
  }
  public static <E extends GeometricObject> boolean equalArea(E object1, E object2) {
    return object1.getArea() == object2.getArea();
  }
  ```

* The second implementation focus the generic type to be a subtype with `GeometricObject`

* To improve the above example, we can change Object to `GeometricObject`, so it will check the type in compilation time:

  ```java
  public static boolean equalArea(GeometricObject object1, GeometricObject object2) {
    // Object don't have getArea()
    return object1.getArea() == object2.getArea();
  }
  public static <E extends GeometricObject> boolean equalArea(E object1, E object2) {
    return object1.getArea() == object2.getArea();
  }
  ```

## Wild Card

* `?`: any class extends `Object`
* `? extends T`: any class extends `T`
* `? super T`: any class which be extended by `T`

## Type Erasure

* Generic Class is shared by all its instances
* Generic information is not available at run time

## Generic Restriction

* Cannot Create Instances of Type Parameters
* Cannot Declare Static Fields Whose Types are Type Parameters
  * unless it is parameterized by unbounded wildcards
  ```java
  private static T os; // error
  ```
* Cannot Use Casts or instanceof with Parameterized Types
```java
list instanceof ArrayList<Integer> // compile-time error
  
List<Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```
  
```java
List<? extends Integer> li = new ArrayList<>();
List<Number>  ln = (List<Number>) li;  // compile-time error
```
  
  ```java
  List<String> l1 = ...;
  ArrayList<String> l2 = (ArrayList<String>)l1;  // OK
  ```
  
  
* Cannot Create Arrays of Parameterized Types

```java
List<Integer>[] arrayOfLists = new List<Integer>[2];  // compile-time error
```

```java
Object[] stringLists = new List<String>[2];  // compiler error, but pretend it's allowed
stringLists[0] = new ArrayList<String>();   // OK
stringLists[1] = new ArrayList<Integer>();  // An ArrayStoreException should be thrown,
                                            // but the runtime can't detect it.

```

* A generic class cannot extend the `Throwable` class directly or indirectly

```java
public static <T extends Exception, J> void execute(List<J> jobs) {
    try {
        for (J job : jobs)
            // ...
    } catch (T e) {   // compile-time error
        // ...
    }
}
```

```java
class Parser<T extends Exception> {
    public void parse(File file) throws T {     // OK
        // ...
    }
}
```


* A class cannot have two overloaded methods that will have the same signature after type erasure.

```java
public class Example {
    public void print(Set<String> strSet) { }
    public void print(Set<Integer> intSet) { }
}
```