---
title: Java 学习笔记
date: 2019-02-02 22:22:22
categories: 
  - Java
tags:
  - Java
---

**入门格式**

```java
public class DataTypesC {
    public static void main(String[] args) {
        System.out.println();
    }
}
```

**注释**
```java
// sigle line comment

/*
multiple lines
*/
```

**数据类型**

```java
int
boolean
char
// example
int myNumber = 42;
boolean isFun = true; 
char movieRating = 'A'; 
```

**运算符号**

```java
+
-
*
/
%

// Relational Operators
<  //  less than.
<= //  less than or equal to.
>  //  greater than.
>= //  greater than or equal to.

// Equality Operators
== //  equal to.
!= //  not equal to.

// Boolean Operators
&& // and
|| // or
!  // not
```

**Conditionals and Control Flow**
条件和控制流？

```java
public class Conditionals {
	public static void main(String[] args) {
        /* Conditionals and Control Flow */
        // If Statement
        if ( ) {
            ;
        } else if ( ) {
            ;
        } else {
            ;
        }

        // Ternary Conditional
        // () ? :
        int a = 1 < 2 ? 1 : 2;

        // Switch Statement
        switch ( ) {
            case 1 :
                ;
                break;
            case 2 :
                ;
                break;
            default:
                ;
                break;
        }

        // For Loop
        for (int counter = 0; counter < 5; counter++) {
            ;
        }
	}
}
```

**object-oriented programming (OOP)**

**面向对象编程**

```java
// Class
class class_name {
    // Instance Variables
    int parameter;
    // constructor
    public class_name(int arg = 233) {
        parameter = arg;
    }
    // Method
    public void method() {
        ;
    }
    // The main Method
    public static void main(String[] args) {
        // instance
        class_name instance = new class_name();
        // Using Method
        instance.method();
    }
}
// Inheritance
class another_class extends class_name {

}
```

**Data Structures**
**数据结构**

```java
// import java.util.*;
import java.util.ArrayList;
import java.util.HashMap;

public class Temperatures {
	public static void main(String[] args) {
        // ArrayList
        ArrayList<Integer> myArray = new ArrayList<Integer>();
        // ArrayList: Manipulation
        myArray.add(233);
        // ArrayList: Insertion
        myArray.add(0, 666); // insert 666 at [0] and others push back
        // ArrayList: Access
        myArray.get(0); // return 233
        // size()
        myArray.size();
        // For Loop
        for(int i = 0; i < myArray.size(); ++i) ;
        // For Each Loop
        for(Integer i : myArray) ;

        // HashMap
        HashMap<String, Integer> myHash = new HashMap<String, Integer>();
        // HashMap: Manipulation
        myHash.put("Index", 233);
        // HashMap: Access
        myHash.get("Index");
        // Iterating over a HashMap
        for(String index : myHash.keySet()) {
            System.out.println(myHash.get(index));
        }
	}
}
```