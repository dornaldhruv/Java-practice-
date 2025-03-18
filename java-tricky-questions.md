# Java Online Assessment Practice Questions

## String and Object Questions

### Question 1
```java
String s1 = "Java";
String s2 = "Java";
String s3 = new String("Java");
String s4 = s3.intern();

System.out.println(s1 == s2);
System.out.println(s1 == s3);
System.out.println(s1 == s4);
```

What is the output?

**Answer:**
```
true
false
true
```

**Explanation:** 
- `s1` and `s2` refer to the same string literal in the string pool
- `s3` creates a new String object in heap memory (not in the string pool)
- `s4` calls the intern() method on s3, which returns the string pool reference
- `==` compares object references, not content

### Question 2
```java
StringBuilder sb1 = new StringBuilder("Hello");
StringBuilder sb2 = new StringBuilder("Hello");
String s1 = "Hello";
String s2 = "Hello";

System.out.println(sb1 == sb2);
System.out.println(sb1.equals(sb2));
System.out.println(s1 == s2);
System.out.println(s1.equals(s2));
```

What is the output?

**Answer:**
```
false
false
true
true
```

**Explanation:**
- StringBuilder doesn't override equals(), so it uses Object's equals() which compares references
- String literals with the same content share the same reference
- String overrides equals() to compare content

### Question 3
```java
class Test {
    public static void main(String[] args) {
        String s1 = "Hello";
        String s2 = "He" + "llo";
        String s3 = "He";
        String s4 = s3 + "llo";
        
        System.out.println(s1 == s2);
        System.out.println(s1 == s4);
    }
}
```

What is the output?

**Answer:**
```
true
false
```

**Explanation:**
- `s2` is computed at compile time as the literals are constants
- `s4` is computed at runtime, creating a new String object


## Collections Questions

### Question 4
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        HashMap<Employee, String> map = new HashMap<>();
        Employee e1 = new Employee(1, "John");
        Employee e2 = new Employee(1, "John");
        
        map.put(e1, "HR");
        System.out.println(map.containsKey(e2));
        System.out.println(map.size());
    }
}

class Employee {
    int id;
    String name;
    
    Employee(int id, String name) {
        this.id = id;
        this.name = name;
    }
    
    // No hashCode() or equals() overridden
}
```

What is the output?

**Answer:**
```
false
1
```

**Explanation:**
- Without overriding hashCode() and equals(), two Employee objects with the same values are considered different
- HashMap uses hashCode() and equals() to determine equality of keys

### Question 5
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);
        
        for (Integer i : list) {
            if (i == 2) {
                list.remove(i);
            }
        }
        
        System.out.println(list);
    }
}
```

What happens when this code is executed?

**Answer:** ConcurrentModificationException will be thrown.

**Explanation:**
- Modifying a collection while iterating through it using an enhanced for loop causes ConcurrentModificationException
- Should use Iterator's remove() method or use a standard for loop with index manipulation

### Question 6
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        TreeMap<Student, String> map = new TreeMap<>();
        map.put(new Student(101, "Alice"), "CS");
        map.put(new Student(103, "Bob"), "EE");
        map.put(new Student(102, "Charlie"), "ME");
        
        for (Map.Entry<Student, String> entry : map.entrySet()) {
            System.out.print(entry.getKey().id + " ");
        }
    }
}

class Student {
    int id;
    String name;
    
    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }
}
```

What happens when this code is executed?

**Answer:** ClassCastException will be thrown.

**Explanation:**
- TreeMap requires its keys to be comparable (either implement Comparable or provide a Comparator)
- Student class doesn't implement Comparable interface

### Question 7
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        LinkedList<String> list = new LinkedList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        
        list.remove(1);
        list.addFirst("D");
        list.addLast("E");
        
        System.out.println(list);
    }
}
```

What is the output?

**Answer:**
```
[D, A, C, E]
```

**Explanation:**
- Initially list is [A, B, C]
- After removing element at index 1: [A, C]
- After adding D at the beginning: [D, A, C]
- After adding E at the end: [D, A, C, E]

## Memory and Runtime Questions

### Question 8
```java
public class Main {
    public static void main(String[] args) {
        Integer i1 = 127;
        Integer i2 = 127;
        Integer i3 = 128;
        Integer i4 = 128;
        
        System.out.println(i1 == i2);
        System.out.println(i3 == i4);
    }
}
```

What is the output?

**Answer:**
```
true
false
```

**Explanation:**
- Integer caches values between -128 and 127
- For values in this range, the same object is reused
- For values outside this range, new objects are created

### Question 9
```java
public class Main {
    static void modify(StringBuilder s) {
        s.append(" World");
    }
    
    static void replace(StringBuilder s) {
        s = new StringBuilder("Hello Universe");
    }
    
    public static void main(String[] args) {
        StringBuilder s1 = new StringBuilder("Hello");
        StringBuilder s2 = new StringBuilder("Hello");
        
        modify(s1);
        replace(s2);
        
        System.out.println(s1);
        System.out.println(s2);
    }
}
```

What is the output?

**Answer:**
```
Hello World
Hello
```

**Explanation:**
- In `modify()`, the reference points to the same object, so changes affect the original
- In `replace()`, the reference is reassigned to a new object, leaving the original unchanged
- Java is always pass-by-value, but the value being passed for objects is a reference

### Question 10
```java
public class Main {
    public static void main(String[] args) {
        try {
            System.out.print("A");
            throw new RuntimeException();
        } catch (Exception e) {
            System.out.print("B");
            return;
        } finally {
            System.out.print("C");
        }
        System.out.print("D");
    }
}
```

What is the output?

**Answer:**
```
ABC
```

**Explanation:**
- "A" is printed first
- Exception is caught, "B" is printed
- `return` is encountered but `finally` block executes before actual return
- "C" is printed
- "D" is never printed due to the return statement in catch

## Package and Access Questions

### Question 11
```java
package com.test;

class Base {
    protected void display() {
        System.out.println("Base");
    }
}

public class Main extends Base {
    public static void main(String[] args) {
        Base obj = new Base();
        obj.display();  // Line 1
        
        Main child = new Main();
        child.display();  // Line 2
        
        Base polyObj = new Main();
        polyObj.display();  // Line 3
    }
    
    protected void display() {
        System.out.println("Main");
    }
}
```

Which lines will compile without error?

**Answer:** All lines will compile without error.

**Explanation:**
- `protected` members are accessible within the same package
- Since Main extends Base and they're in the same package, all calls are valid
- If they were in different packages, Line 1 would fail

### Question 12
```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(1, 2, 3);
        list.add(4);
        System.out.println(list);
    }
}
```

What happens when this code is executed?

**Answer:** UnsupportedOperationException will be thrown.

**Explanation:**
- Arrays.asList() returns a fixed-size list backed by the original array
- This list doesn't support add or remove operations
- To create a modifiable list: `new ArrayList<>(Arrays.asList(1, 2, 3))`

## Compile-time vs. Runtime Errors

### Question 13
```java
public class Main {
    public static void main(String[] args) {
        final int[] arr = {1, 2, 3};
        arr[0] = 10;  // Line 1
        arr = new int[]{4, 5, 6};  // Line 2
        System.out.println(arr[0]);
    }
}
```

Which line causes a compilation error?

**Answer:** Line 2 will cause a compilation error.

**Explanation:**
- final with arrays makes the reference final, not the content
- Elements can still be modified (Line 1)
- Reassigning the reference (Line 2) is not allowed

### Question 14
```java
public class Main {
    static int count = 0;
    
    public static void recursiveMethod() {
        count++;
        recursiveMethod();
    }
    
    public static void main(String[] args) {
        try {
            recursiveMethod();
        } catch (Throwable e) {
            System.out.println("Error: " + e.getClass().getSimpleName());
        }
        System.out.println("Count: " + count);
    }
}
```

What is the output?

**Answer:** The exact count will vary based on JVM settings, but will be similar to:
```
Error: StackOverflowError
Count: 11034
```

**Explanation:**
- The recursion without a base case causes a StackOverflowError
- The count variable shows how many recursive calls were made before the stack overflow
- StackOverflowError is a runtime error, not a compilation error

### Question 15
```java
public class Main {
    public static void main(String[] args) {
        Object obj = new Integer(10);
        String str = (String) obj;
        System.out.println(str);
    }
}
```

What happens when this code is executed?

**Answer:** ClassCastException is thrown at runtime.

**Explanation:**
- This is a runtime error, not a compile-time error
- The compiler allows casting between Object and String
- At runtime, an Integer cannot be cast to a String
