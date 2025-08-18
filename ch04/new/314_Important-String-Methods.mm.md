---

markmap:
  initialExpandLevel: 1
---

# **Important `String` Methods**
- For all these methods, you need to remember that a string
is a sequence of characters and Java counts from `0` when
indexed.
    -  The [Figure 4.1](https://1drv.ms/i/c/c83cfca51d5c2032/Eaqzv39-4whGvJ_UQ09B7LoBT5-TT8xP-l8ZAWqfsPM_cg?e=AqLQks)  shows how each character in 
       the string `"animals"` is indexed. 
         

          - You also need to know that a `String` is  immutable, or
            unchangeable. This means calling a method on a `String` will
            return a different `String` object rather than changing the value 
            of the reference.
        
- **Determining the Length**
The method `length()` returns the number of characters in
the `String`. The method signature is as follows:
    ```java
    public int length()
    ``` 
    - The following code shows how to use `length()`:
        ```java 
        var name = "animals";
        System.out.println(name.length()); // 7
        ```
        - Wait. It outputs `7`? Didn’t we just tell you that Java counts
        from zero? The difference is that zero counting happens
        only when you’re using indexes or positions within a list.
        When determining the total size or length, Java uses normal
        counting again.
            - **Getting a Single Character**
                The method `charAt()` lets you query the string to find out
                what character is at a specific index. The method signature
                is as follows:
                ```java
                public char charAt(int index)
                ```
                - The following code shows how to use `charAt()`:
                    ```java 
                    var name = "animals";
                    System.out.println(name.charAt(0)); //a
                    System.out.println(name.charAt(6)); //s
                    System.out.println(name.charAt(7)); //exception
                     ```
                     - Since indexes start counting with zero, `charAt(0)`    returns the
                    “first” character in the sequence. Similarly, `charAt(6)`
                    returns the “seventh” character in the sequence. However,
                    `charAt(7)` is a problem.  Java throws an exception.

                        ```java
                        java.lang.StringIndexOutOfBoundsException: 
                            String index out of range: 7
                        ``` 
- **Working with Code Points**
Around the world some characters use a longer encoding
 called Unicode which has a wider range and doesn’t fit in 
 a `char`, such as a stylized quote (’). A code point is bigger
  than a character, so it is expressed as a number. The 
  relevant method signatures are as follows:
    - ```java
        public int codePointAt(int index)
        public int codePointBefore(int index)
        public int codePointCount(int beginIndexInclusive, 
            int endIndexExclusive)
        ```
        - The `codePointAt()` returns the numeric value of the code
          point at the specified index. The `codePointBefore()` method
          does the same, but looks at the value before the index.
          Finally, the `codePointCount()` method returns the number of
          code points between two indexes.
            - ```java
                var s = "Weâ€™re done feeding the animals";
                System.out.println(s.charAt(0) + " " + s.codePointAt(0)); // W 87
                System.out.println(s.charAt(2) + " " + s.codePointAt(2)); // â€™ 8217
                System.out.println(s.codePointBefore(3));                 // 8217
                System.out.println(s.codePointCount(0,4));                // 4
                ```
                - You do not need to memorize the ASCII or Unicode
                 values. You just need to know that if you see `codePointAt()` 
                 on the exam that it functions similarly to `charAt()` for 
                 ASCII characters, returning the numeric value of the 
                 character at the location.
- **Getting a Substring**
    The method `substring()` is similar to `charAt()` except it
    returns a group of characters from the string. The first
    parameter is the index to start with for the returned string.
    As usual, this is a zero-based index. There is an optional
    second parameter, which is the end index you want to stop
    at.
    - Notice we said “stop at” rather than “include.” This means
    the `endIndex` parameter is allowed to be one past the end of
    the sequence if you want to stop at the end of the
    sequence. That would be redundant, though, since you
    could omit the second parameter entirely in that case. In
    your own code, you want to avoid this redundancy. Don’t be
    surprised if the exam uses it, though. The method
    signatures are as follows:
        - ```java 
          public String substring(int beginIndex)
          public String substring(int beginIndex, int endIndex)
          ```
            - The following code shows how to use `substring()`:
                ```java
                 var name = "animals";
                System.out.println(name.substring(3));                 //mals
                System.out.println(name.substring(name.indexOf('m'))); //mals
                System.out.println(name.substring(3, 4));              //m
                System.out.println(name.substring(3, 7));              //mals
                ```
                - The `substring()` method is the trickiest `String` method on the
                exam. The first example says to take the characters starting
                with index `3` through the end, which gives us `"mals"`. The
                second example does the same thing, but it calls `indexOf()`
                to get the index rather than hard-coding it. This is a
                common practice when coding because you may not know
                the index in advance. 
                - The third example says to take the characters starting with
                index `3` until, but not including, the character at index `4`.
                This is a complicated way of saying we want a `String` with
                one character: the one at index 3. This results in `"m"`. The
                final example says to take the characters starting with
                index `3` until we get to index `7`. Since index `7` is the same as
                the end of the string, it is equivalent to the first example.
                    - The next examples are less obvious:
                        ```java
                        System.out.println(name.substring(3, 3)); // empty string
                        System.out.println(name.substring(3, 2)); // exception
                        System.out.println(name.substring(3, 8)); // exception
                        ```
                        - The first example in this set prints an empty string. The
                        request is for the characters starting with index `3` until we
                        get to index `3`.**Since we start and end with the same index,
                        there are no characters in between.** The second example in
                        this set throws an exception because the indexes can’t be
                        backward. Java knows perfectly well that it will never get to
                        index `2` if it starts with index `3`. The third example says to
                        continue until the eighth character. There is no eighth
                        position, so Java throws an exception. 
                        Let’s review this one more time since `substring()` is so
                        tricky. The method returns the string starting from the
                        requested index. If an end index is requested, it stops right
                        before that index. Otherwise, it goes to the end of the
                        string.
- **Finding an Index**
    The method `indexOf()` looks at the characters in the string
    and finds the first index that matches the desired value.
    The `indexOf` method can work with an individual character
    or a whole `String` as input. It can also start and end the
    search from specific positions. Note, the **starting index is
    inclusive, and the ending index is exclusive**. Remember that
    a `char` can be passed to an `int` parameter type. On the exam,
    you’ll only see a `char` passed to the parameters named `ch`.
    The method signatures are as follows:
    - ```java 
        public int indexOf(int ch)
        public int indexOf(int ch, int fromIndex)
        public int indexOf(int ch, int fromIndex, int endIndex)
        public int indexOf(String str)
        public int indexOf(String str, int fromIndex)
        public int indexOf(String str, int fromIndex, int endIndex)
        ```
        - The following code shows you how to use `indexOf()`:
            ```java
            10: var name = "animals";
            11: System.out.println(name.indexOf('a'));        // 0
            12: System.out.println(name.indexOf("al"));       // 4
            13: System.out.println(name.indexOf('a', 4));     // 4
            14: System.out.println(name.indexOf("al", 5));    // -1
            15: System.out.println(name.indexOf('a', 2, 4));  // -1
            16: System.out.println(name.indexOf("al", 2, 6)); // 4
            ```
            - Since indexes begin with 0, the first `'a'` matches at that
            position. Therefore, line 11 outputs 0. On line 12, Java looks
            for a more specific string, so it matches later. On line 13,
            Java shouldn’t even look at the characters until it gets to
            index 4. Line 14 doesn’t find anything because it starts
            looking after the match occurred. Unlike `charAt()`, the
            `indexOf()` method doesn't throw an exception if it can't find
            a match, instead returning `–1`. Because indexes start with `0`,
            the caller knows that `–1` couldn’t be a valid index. This
            makes it a common value for a method to signify to the
            caller that no match is found.
                - Line 15 looks for a match starting at index `2` and earlier
                than index `4`. This means indices `2` or `3`. Since neither of
                those matches, the method returns `-1`. Finally, line 16 looks
                for a match starting at index `2` since starting indexes are
                inclusive. It ends before at index `6` since the end index is
                exclusive. This means indices `2`, `3`, `4`, and `5`. The characters
                at index `4` and `5` match the target. The first one of those is `4`,
                which is returned.
- **Adjusting Case**
    Whew. After that mental exercise, it is nice to have
    methods that act exactly as they sound! These methods
    make it easy to convert your data. The method signatures
    are as follows:
    ```java
    public String toLowerCase()
    public String toUpperCase()
    ```
    - The following code shows how to use these methods:
        ```java 
        var name = "animals";
        System.out.println(name.toUpperCase());     // ANIMALS
        System.out.println("Abc123".toLowerCase()); // abc123
        ```
        - These methods do what they say. The `toUpperCase()` method
        converts any lowercase characters to uppercase in the
        returned string. The `toLowerCase()` method converts any
        uppercase characters to lowercase in the returned string.
        These methods leave alone any characters other than
        letters. Also, remember that strings are immutable, so the
        original string stays the same.
- **Checking for Equality**

    The `equals()` method checks whether two `String` objects
    contain exactly the same characters in the same order. The
    `equalsIgnoreCase()` method checks whether two `String` objects
    contain the same characters, ignoring the case. The method 
    signatures are as follows:
    
    ```java 
    public boolean equals(Object obj)
    public boolean equalsIgnoreCase(String str)
    ```
    - You might have noticed that `equals()` takes an `Object` rather
    than a `String`. This is because the method is the same for all
    objects. If you pass in something that isn’t a `String`, it will
    just return `false`. By contrast, the `equalsIgnoreCase()` method
    applies only to `String` objects, so it can take the more
    specific type as the parameter.

        - In Java, `String` values are case-sensitive. That means `"abc"`
        and `"ABC"` are considered different values. With that in
        mind, the following code shows how to use these methods:
            - ```java
                System.out.println("abc".equals("ABC")); // false
                System.out.println("ABC".equals("ABC")); // true
                System.out.println("ABC".equals(6)); // false
                System.out.println("abc".equalsIgnoreCase("ABC")); // true
                ```
                - In the first example, the values aren’t exactly the same. In
                the second, they are exactly the same. The third example
                shows what happens if you pass a different type. In the last 
                example, the values differ only by case, but it is OK because 
                we called the method that ignores differences in case.