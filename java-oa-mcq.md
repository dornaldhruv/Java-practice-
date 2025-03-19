# Java OA Multiple Choice Questions

## Section 1: String and Object Handling

### Question 1
```java
String str1 = "Hello";
String str2 = new String("Hello");
String str3 = str2.intern();

System.out.println(str1 == str2);
System.out.println(str1 == str3);
System.out.println(str1.equals(str2));
```

What is the output?

A) 
```
false
true
true
```

B)
```
false
false
true
```

C)
```
true
true
true
```

D)
```
true
false
true
```

**Answer: A**

**Explanation:**
- `str1` is a string literal stored in the string pool
- `str2` is created using the `new` keyword, so it's a new object in the heap
- `str3` is the result of calling `intern()` on `str2`, which returns the string pool reference
- `==` compares object references, so `str1 == str2` is false (different objects)
- `str1 == str3` is true (both refer to the same string pool object)
- `equals()` compares content, so `str1.equals(str2)` is true

### Question 2
```java
public class Test {
    public static void main(String[] args) {
        String s1 = "Java";
        String s2 = "Java";
        StringBuilder sb1 = new StringBuilder("Java");
        StringBuilder sb2 = new StringBuilder("Java");
        
        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
        System.out.println(sb1 == sb2);
        System.out.println(sb1.equals(sb2));
    }
}
```

What is the output?

A)
```
true
true
false
false
```

B)
```
true
true
false
true
```

C)
```
false
true
false
false
```

D)
```
false
true
false
true
```

**Answer: A**

**Explanation:**
- `s1` and `s2` refer to the same string literal in the string pool, so `s1 == s2` is true
- String overrides `equals()` to compare content, so `s1.equals(s2)` is true
- `sb1` and `sb2` are different StringBuilder objects, so `sb1 == sb2` is false
- StringBuilder doesn't override `equals()`, so it uses Object's implementation which compares references, making `sb1.equals(sb2)` false

### Question 3
```java
class Test {
    public static void main(String[] args) {
        String[] strings = {"Hello", "World"};
        Object[] objects = strings;
        
        objects[0] = "Hi";       // Line 1
        objects[1] = new Integer(5);  // Line 2
        
        for(String s : strings) {
            System.out.print(s + " ");
        }
    }
}
```

What is the result?

A) Hi World

B) Hello World

C) Hi 5

D) ArrayStoreException at Line 2

**Answer: D**

**Explanation:**
- String[] is a subtype of Object[], so assignment is allowed
- Line 1 works because "Hi" is a String
- Line 2 tries to store an Integer in a String array, causing ArrayStoreException
- Array type information is preserved at runtime, unlike generic type information

## Section 2: Collections and Data Structures

### Question 4
```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
        Set<Integer> set1 = new HashSet<>();
        set1.add(1);
        set1.add(2);
        set1.add(3);
        
        Set<Integer> set2 = new HashSet<>();
        set2.add(3);
        set2.add(4);
        set2.add(5);
        
        set1.retainAll(set2);
        set1.addAll(set2);
        
        System.out.println(set1.size());
    }
}
```

What is printed?

A) 3

B) 4

C) 5

D) 6

**Answer: A**

**Explanation:**
- `set1` starts with {1, 2, 3}
- `set2` contains {3, 4, 5}
- `set1.retainAll(set2)` keeps only elements common to both sets, so `set1` becomes {3}
- `set1.addAll(set2)` adds all elements from `set2`, making `set1` = {3, 4, 5}
- Since sets don't allow duplicates, the size is 3

### Question 5
```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
        Map<String, Integer> map = new HashMap<>();
        map.put("A", 1);
        map.put("B", 2);
        map.put("C", 3);
        
        Integer val1 = map.remove("B");
        Integer val2 = map.remove("D");
        
        System.out.println(val1 + " " + val2);
    }
}
```

What is the output?

A) 2 null

B) 2 0

C) null null

D) NullPointerException

**Answer: A**

**Explanation:**
- `map.remove("B")` returns the value associated with key "B", which is 2
- `map.remove("D")` tries to remove a non-existent key, so it returns null
- Printing val1 + " " + val2 gives "2 null"
- No NullPointerException because autoboxing doesn't happen with string concatenation

### Question 6
```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
        List<String> list = new ArrayList<>();
        list.add("A");
        list.add("B");
        list.add("C");
        list.add("D");
        
        Iterator<String> iter = list.iterator();
        while(iter.hasNext()) {
            String str = iter.next();
            if(str.equals("B")) {
                iter.remove();
            }
            if(str.equals("C")) {
                list.remove(str);
            }
        }
        
        System.out.println(list);
    }
}
```

What is the result?

A) [A, D]

B) [A, C, D]

C) ConcurrentModificationException

D) [A, D, null]

**Answer: C**

**Explanation:**
- Using `iter.remove()` for "B" is the correct way to remove during iteration
- But using `list.remove(str)` for "C" modifies the list while iterating with an iterator
- This causes ConcurrentModificationException when `iter.hasNext()` is called after removing "C"
- To avoid this, always use the iterator's remove method when removing elements during iteration

## Section 3: Memory and Runtime Behavior

### Question 7
```java
public class Test {
    public static void main(String[] args) {
        Integer num1 = 127;
        Integer num2 = 127;
        Integer num3 = 200;
        Integer num4 = 200;
        
        System.out.println(num1 == num2);
        System.out.println(num3 == num4);
    }
}
```

What is the output?

A)
```
true
true
```

B)
```
false
false
```

C)
```
true
false
```

D)
```
false
true
```

**Answer: C**

**Explanation:**
- Java caches Integer objects in the range -128 to 127
- `num1` and `num2` both point to the same cached Integer object with value 127
- `num3` and `num4` are outside the cache range, so they are different objects
- `==` compares object references, not the values

### Question 8
```java
public class Test {
    private static void modifyValue(Integer i) {
        i = i + 1;
    }
    
    private static void modifyArray(int[] arr) {
        arr[0] = 10;
    }
    
    public static void main(String[] args) {
        Integer x = 5;
        modifyValue(x);
        System.out.println(x);
        
        int[] array = {5};
        modifyArray(array);
        System.out.println(array[0]);
    }
}
```

What is the output?

A)
```
5
10
```

B)
```
6
5
```

C)
```
6
10
```

D)
```
5
5
```

**Answer: A**

**Explanation:**
- `modifyValue(x)` receives a copy of the reference to the Integer object
- Integers are immutable, so `i = i + 1` creates a new Integer object
- The reference in `modifyValue` now points to the new object, but the original `x` is unchanged
- `modifyArray(array)` receives a copy of the reference to the array object
- Arrays are mutable, so `arr[0] = 10` changes the content of the original array

### Question 9
```java
public class Test {
    public static void main(String[] args) {
        try {
            System.out.print("A");
            int[] arr = new int[5];
            arr[10] = 50;
            System.out.print("B");
        } catch (StringIndexOutOfBoundsException e) {
            System.out.print("C");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.print("D");
        } catch (Exception e) {
            System.out.print("E");
        } finally {
            System.out.print("F");
        }
        System.out.print("G");
    }
}
```

What is the output?

A) ADFG

B) AEF

C) ACF

D) ACFG

**Answer: A**

**Explanation:**
- "A" is printed first
- `arr[10] = 50` throws ArrayIndexOutOfBoundsException
- This is caught by the second catch block, so "D" is printed
- The finally block always executes, so "F" is printed
- After completing the try-catch, the program continues and prints "G"

## Section 4: Java Language Features

### Question 10
```java
public class Test {
    static {
        System.out.print("A");
    }
    
    {
        System.out.print("B");
    }
    
    public Test() {
        System.out.print("C");
    }
    
    public static void main(String[] args) {
        System.out.print("D");
        Test test = new Test();
        System.out.print("E");
    }
}
```

What is the output?

A) ADCBE

B) ADBCE

C) DABCE

D) DBACE

**Answer: B**

**Explanation:**
- Static blocks execute when the class is loaded, so "A" is printed first
- Then main method starts executing, printing "D"
- When `new Test()` is called, first the instance initialization block runs printing "B"
- Then the constructor runs, printing "C"
- Finally, "E" is printed

### Question 11
```java
class Parent {
    int value = 10;
    
    int getValue() {
        return value;
    }
}

class Child extends Parent {
    int value = 20;
    
    int getValue() {
        return value;
    }
}

public class Test {
    public static void main(String[] args) {
        Parent p = new Child();
        System.out.println(p.value + " " + p.getValue());
    }
}
```

What is the output?

A) 10 10

B) 20 20

C) 10 20

D) 20 10

**Answer: C**

**Explanation:**
- Instance variables are not overridden; they are hidden
- Methods are overridden
- `p.value` accesses the variable from the reference type (Parent), so returns 10
- `p.getValue()` calls the overridden method in Child, which returns 20

### Question 12
```java
public class Test {
    public static void main(String[] args) {
        double d = 12.345;
        float f = (float) d;
        long l = (long) f;
        int i = (int) l;
        
        System.out.println(i);
    }
}
```

What is the output?

A) 12.345

B) 12.0

C) 12

D) Compilation error

**Answer: C**

**Explanation:**
- The double 12.345 is cast to float without losing digits after the decimal
- When cast to long, the decimal part is truncated, resulting in 12
- When cast to int, the value remains 12
- The output is therefore 12

## Section 5: Exception Handling and Flow Control

### Question 13
```java
public class Test {
    public static int getValue() {
        try {
            return 1;
        } finally {
            return 2;
        }
    }
    
    public static void main(String[] args) {
        System.out.println(getValue());
    }
}
```

What is the output?

A) 1

B) 2

C) 3

D) Runtime exception

**Answer: B**

**Explanation:**
- The `finally` block always executes before a method returns
- If the `finally` block has a return statement, it overrides any return statement in the `try` block
- So the method returns 2, not 1

### Question 14
```java
public class Test {
    public static void main(String[] args) {
        int x = 0;
        int y = 0;
        
        for (int z = 0; z < 5; z++) {
            if ((++x > 2) && (++y > 2)) {
                x++;
            }
        }
        
        System.out.println(x + " " + y);
    }
}
```

What is the output?

A) 5 0

B) 6 3

C) 8 5

D) 10 5

**Answer: B**

**Explanation:**
- For z=0: x=1, y=0, condition fails, no increment
- For z=1: x=2, y=0, condition fails, no increment
- For z=2: x=3, y=1, first part true, second part false, no increment
- For z=3: x=4, y=2, first part true, second part false, no increment
- For z=4: x=5, y=3, both parts true, x incremented to 6

### Question 15
```java
import java.io.*;

public class Test {
    public static void main(String[] args) {
        try {
            throw new Exception("1");
        } catch (IOException e) {
            System.out.print("2");
        } catch (Exception e) {
            System.out.print("3");
            throw new RuntimeException("4");
        } finally {
            System.out.print("5");
        }
        System.out.print("6");
    }
}
```

What is the output?

A) 356

B) 35 followed by RuntimeException

C) 3 followed by RuntimeException

D) 36

**Answer: B**

**Explanation:**
- The code throws an Exception with message "1"
- This is caught by the second catch block (since IOException is more specific)
- "3" is printed
- A new RuntimeException is thrown
- Before propagating, the finally block executes and prints "5"
- The RuntimeException is propagated, so "6" is never printed

## Section 6: Generics and Advanced Features

### Question 16
```java
import java.util.*;

public class Test {
    public static void main(String[] args) {
        List<?> list1 = new ArrayList<String>();
        List<Object> list2 = new ArrayList<String>();  // Line 1
        List<String> list3 = new ArrayList<Object>();  // Line 2
        List<? extends String> list4 = new ArrayList<Object>();  // Line 3
        List<? super String> list5 = new ArrayList<Object>();    // Line 4
    }
}
```

Which lines cause compilation errors?

A) Line 1, Line 2, Line 3

B) Line 2, Line 3, Line 4

C) Line 1, Line 3

D) Line 2, Line 3

**Answer: D**

**Explanation:**
- Line 1: Valid. Wildcard `?` accepts any type parameter
- Line 2: Invalid. List<Object> is not a supertype of List<String>
- Line 3: Invalid. List<String> is not a subtype of List<Object>
- Line 4: Invalid. <? extends String> means String or a subclass, but Object is a superclass
- Line 5: Valid. <? super String> means String or a superclass, Object is a superclass

### Question 17
```java
class Animal {}
class Dog extends Animal {}
class Cat extends Animal {}

public class Test {
    public static void main(String[] args) {
        Animal[] animals = new Dog[5];
        animals[0] = new Dog();  // Line 1
        animals[1] = new Cat();  // Line 2
        
        List<Animal> animalList = new ArrayList<Dog>();  // Line 3
    }
}
```

Which lines cause errors?

A) Line 1 and Line 3

B) Line 2 only

C) Line 2 and Line 3

D) Line 3 only

**Answer: C**

**Explanation:**
- Line 1: Valid. You can put a Dog in a Dog array
- Line 2: Runtime Error (ArrayStoreException). Can't put a Cat in a Dog array
- Line 3: Compile Error. Generic type information is checked at compile time
- Unlike arrays, generics don't allow this kind of covariance

### Question 18
```java
public class Test {
    private static void print(int... numbers) {
        System.out.print("1");
    }
    
    private static void print(Integer... numbers) {
        System.out.print("2");
    }
    
    private static void print(long... numbers) {
        System.out.print("3");
    }
    
    private static void print(Object... objects) {
        System.out.print("4");
    }
    
    public static void main(String[] args) {
        print(1, 2, 3);
        print(1L, 2L, 3L);
        print(new Integer(1), new Integer(2));
        print("Hello", "World");
    }
}
```

What is the output?

A) 1234

B) 1334

C) 1324

D) Compilation error

**Answer: C**

**Explanation:**
- `print(1, 2, 3)` matches most specifically to int... so prints "1"
- `print(1L, 2L, 3L)` matches most specifically to long... so prints "3"
- `print(new Integer(1), new Integer(2))` matches Integer... so prints "2"
- `print("Hello", "World")` matches only Object... so prints "4"

## Section 7: Concurrency and Threading

### Question 19
```java
class Counter {
    private int count = 0;
    
    public void increment() {
        count++;
    }
    
    public void decrement() {
        count--;
    }
    
    public int getCount() {
        return count;
    }
}

public class Test {
    public static void main(String[] args) throws Exception {
        final Counter counter = new Counter();
        
        Thread t1 = new Thread(new Runnable() {
            public void run() {
                for(int i = 0; i < 1000; i++) {
                    counter.increment();
                }
            }
        });
        
        Thread t2 = new Thread(new Runnable() {
            public void run() {
                for(int i = 0; i < 1000; i++) {
                    counter.decrement();
                }
            }
        });
        
        t1.start();
        t2.start();
        t1.join();
        t2.join();
        
        System.out.println(counter.getCount());
    }
}
```

What is the most likely output?

A) 0

B) 1000

C) -1000

D) A value that can vary from run to run

**Answer: D**

**Explanation:**
- Both threads are accessing the same Counter object concurrently
- The increment and decrement operations are not atomic
- Without proper synchronization, race conditions can occur
- The final count could be any value, not necessarily 0
- This is a classic example of non-thread-safe code

### Question 20
```java
public class Test {
    public static void main(String[] args) {
        Thread t = new Thread() {
            public void run() {
                if(Thread.currentThread().isDaemon()) {
                    System.out.print("Daemon");
                } else {
                    System.out.print("Non-Daemon");
                }
            }
        };
        
        t.setDaemon(true);
        t.start();
    }
}
```

What is the output?

A) Daemon

B) Non-Daemon

C) No output or Daemon (depending on timing)

D) Compilation error

**Answer: C**

**Explanation:**
- The thread is set as a daemon thread before starting
- Daemon threads don't prevent the JVM from exiting when all non-daemon threads finish
- The main thread is a non-daemon thread
- If the main thread completes before the daemon thread prints, the program will exit with no output
- If the daemon thread has time to print before the main thread exits, it will print "Daemon"
- The exact behavior depends on timing and JVM implementation

## Section 8: Miscellaneous Tricky Questions

### Question 21
```java
public class Test {
    public static void main(String[] args) {
        String s1 = "Java";
        String s2 = "Java";
        StringBuilder sb1 = new StringBuilder();
        sb1.append("Ja").append("va");
        String s3 = sb1.toString();
        
        System.out.println(s1 == s2);
        System.out.println(s1 == s3);
        System.out.println(s1.equals(s3));
    }
}
```

What is the output?

A)
```
true
true
true
```

B)
```
true
false
true
```

C)
```
false
false
true
```

D)
```
true
false
false
```

**Answer: B**

**Explanation:**
- `s1` and `s2` are string literals, so they reference the same object in the string pool
- `s3` is created from a StringBuilder, so it's a new string object in the heap (not in the pool)
- `s1 == s2` is true (same object reference)
- `s1 == s3` is false (different object references)
- `s1.equals(s3)` is true (same content)

### Question 22
```java
public enum Season {
    WINTER, SPRING, SUMMER, FALL;
    
    private Season() {
        System.out.print(this.name() + " ");
    }
    
    public static void main(String[] args) {
        Season season = Season.WINTER;
    }
}
```

What is the output?

A) WINTER

B) WINTER SPRING SUMMER FALL

C) No output

D) Compilation error

**Answer: B**

**Explanation:**
- When an enum is first accessed, all enum constants are initialized
- The constructor is called for each enum constant in the order they are declared
- So all four constructor calls happen, each printing its name
- The output is "WINTER SPRING SUMMER FALL"

### Question 23
```java
public class Test {
    public static void main(String[] args) {
        int x = 10;
        int y = ++x * x++;
        
        System.out.println("x = " + x + ", y = " + y);
    }
}
```

What is the output?

A) x = 11, y = 121

B) x = 12, y = 132

C) x = 12, y = 121

D) x = 12, y = 110

**Answer: C**

**Explanation:**
- `++x` increments x to 11 before using it
- `x++` uses the current value of x (11) and then increments x to 12
- So y = 11 * 11 = 121
- After the calculation, x = 12 and y = 121

### Question 24
```java
public class Test {
    public static void main(String[] args) {
        Object obj = null;
        Integer num = (Integer) obj;
        int value = num;
        
        System.out.println(value);
    }
}
```

What is the result?

A) 0

B) null

C) NullPointerException at line with Integer cast

D) NullPointerException at line with unboxing

**Answer: D**

**Explanation:**
- Casting null to Integer works and results in a null reference
- Trying to unbox null Integer to an int causes NullPointerException
- The exception occurs during the unboxing operation (int value = num)
- This is a common pitfall with autoboxing/unboxing

### Question 25
```java
public class Test {
    public static void main(String[] args) {
        double x = 0.0;
        double y = 0.0;
        
        System.out.println(x/y);
    }
}
```

What is the output?

A) 0.0

B) Infinity

C) NaN

D) ArithmeticException

**Answer: C**

**Explanation:**
- In IEEE 754 floating-point arithmetic, dividing 0.0 by 0.0 results in NaN (Not a Number)
- This is different from integer division where 0/0 would cause ArithmeticException
- Dividing a non-zero number by 0.0 would result in Infinity or -Infinity
- NaN is a special value that represents undefined or unrepresentable values

### Question 26
```java
public class Test {
    static Integer i;
    
    public static void main(String[] args) {
        int j = 0;
        
        try {
            int result = i + j;
            System.out.print("Result: " + result);
        } catch (Exception e) {
            System.out.print("Exception: " + e.getClass().getSimpleName());
        } finally {
            System.out.print(" Finally");
        }
    }
}
```

What is the output?

A) Result: 0 Finally

B) Exception: NullPointerException Finally

C) Exception: NumberFormatException Finally

D) Exception: ArithmeticException Finally

**Answer: B**

**Explanation:**
- Static fields are initialized to their default values, so `i` is null
- When attempting to use `i` in an arithmetic operation, it needs to be unboxed
- Trying to unbox a null Integer results in NullPointerException
- The exception is caught, and "Exception: NullPointerException" is printed
- The finally block always executes, so " Finally" is printed
