---

markmap:
  initialExpandLevel: 1
---
# **Understanding Equality**
- **Comparing `equals()` and `==`**
Consider the following code that uses `==` with objects:
    ```java
    var one = new StringBuilder();
    var two = new StringBuilder();
    var three = one.append("a");
    System.out.println(one == two); // false
    System.out.println(one == three); // true
    ```
    - Since this example isn’t dealing with primitives, we know to
    look for whether the references are referring to the same
    object. The `one` and `two` variables are both completely
    separate `StringBuilder` objects, giving us two objects.
    Therefore, the first print statement gives us `false`. The `three`
    variable is more interesting. Remember how `StringBuilder`
    methods like to return the current reference for chaining?
    This means `one` and `three` both point to the same object, and
    the second print statement gives us `true`.
        - `equals()` uses logical equality rather than object equality 
        for `String` objects.
            ```java
            var x = "Hello World";
            var z = " Hello World".trim();
            System.out.println(x.equals(z)); // true
            ```
            `String` class implements a standard method called `equals()` 
            to check the values inside the `String` rather than the string
             reference itself. If a class doesn’t have an `equals()` method, 
             Java determines whether the references point to the same
            object, which is exactly what `==` does.
            -  `StringBuilder` does not implement `equals()`. If you call `equals()` 
            on two `StringBuilder` instances, it will check reference equality. 
            You can call `toString()` on `StringBuilder` to get a `String` to check
            for equality instead.
                - Can you guess why the code doesn’t compile?
                    ```java
                    var name = "a";
                    var builder = new StringBuilder("a");
                    System.out.println(name == builder); // DOES NOT COMPILE
                    ```
                    Remember that `==` is checking for object reference equality. The 
                    compiler is smart enough to know that two references can’t possibly
                     point to the same object when they are completely different types.
- **The String Pool**
The string pool contains literal values and constants that
appear in your program. For example, `"name"` is a literal and
therefore goes into the string pool. The `myObject.toString()`
method returns a string but not a literal, so it does not go
into the string pool.
    - The JVM reuses `String` literals.
        ```java
        var x = "Hello World";
        var y = "Hello World";
        System.out.println(x == y); // true
        ```
        Remember that a `String` is immutable and literals are
        pooled. The JVM created only one literal in memory. The `x`
        and `y` variables both point to the same location in memory;
        therefore, the statement outputs `true`.
        - Consider this code:
            ```java
            var x = "Hello World";
            var z = " Hello World".trim();
            System.out.println(x == z); // false
            ```
            In this example, we don’t have two of the same `String` literal. 
            Although `x` and `z` happen to evaluate to the same string, 
            **one is computed at runtime. Since it isn’t the same at
             compile time, a new `String` object is created.**
            - Let’s try another one. What do you think is output here?
                ```java
                var singleString = "hello world";
                var concat = "hello ";
                concat += "world";
                System.out.println(singleString == concat); // false
                ```
                This prints `false`. **Calling `+=` is just like calling a method and
                results in a new `String`.**
                - Other example:
                    ```java
                    var x = "Hello World";
                    var y = new String("Hello World");
                    System.out.println(x == y); // false
                    ```
                    The first says to use the string pool normally. The second
                    says, “No, JVM, I really don’t want you to use the string
                    pool. Please create a new object for me even though it is
                    less efficient.”
                    - The `intern()` method will use an object in the string pool
                     if one is present.
                        ```java
                        public String intern()
                        ```
                        If the literal is not yet in the string pool, Java will add it at
                        this time.
                        ```java
                        var name = "Hello World";
                        var name2 = new String("Hello World").intern();
                        System.out.println(name == name2); // true
                        ```
                        - First we tell Java to use the string pool normally for `name`.
                        Then, for `name2`, we tell Java to create a new object using the
                        constructor but to intern it and use the string pool anyway.
                        Since both variables point to the same reference in the string 
                        pool, we can use the `==` operator.
                            - Let’s try another one. What do you think this prints out? Be
                            careful. It is tricky.
                                ```java
                                15: var first = "rat" + 1;
                                16: var second = "r" + "a" + "t" + "1";
                                17: var third = "r" + "a" + "t" + new String("1");
                                18: System.out.println(first == second);
                                19: System.out.println(first == second.intern());
                                20: System.out.println(first == third);
                                21: System.out.println(first == third.intern());
                                ```
                                - On line 15, we have a compile-time constant that
                                automatically gets placed in the string pool as `"rat1"`. On
                                line 16, we have a more complicated expression that is also
                                a compile-time constant. Therefore, **`first` and `second` share
                                the same string pool reference.** This makes lines 18 and 19
                                print `true`.
                                - On line 17, we have a `String` constructor. This means we no
                                longer have a compile-time constant, and `third` does not
                                point to a reference in the string pool. Therefore, line 20
                                prints `false`. On line 21, the `intern()` call looks in the string
                                pool. Java notices that `first` points to the same `String` and
                                prints `true`.
                                    - Other example:
                                        ```java
                                        var r1 = "rat" + 1;
                                        var r2 = "rat";
                                        r2 += 1;
                                        var r3 = "rat1";
                                        System.out.println(r1 == r2);//false
                                        System.out.println(r1 == r3);//true
                                        ```