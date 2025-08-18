---

markmap:
  initialExpandLevel: 1
---
# **Using the StringBuilder Class**
-  The `StringBuilder` class creates a `String` without storing interim
 values. Unlike the `String` class, `StringBuilder` is mutable.

- **Mutability and Chaining**
 The exam will likely try to trick you with respect to `StringBuilder` being
  mutable and `String` being immutable.
Chaining makes this even more interesting. When we chained `String`
 method calls, the result was a new `String` with the answer. Chaining
 `StringBuilder` methods doesn’t work this way. Instead, the `StringBuilder` 
 changes its own state and returns a reference to itself.
    - Let’s look at an example to make this clearer:
        ```java
        4: StringBuilder sb = new StringBuilder("start");
        5: sb.append("+middle"); // sb = "start+middle"
        6: StringBuilder same = sb.append("+end"); // "start+middle+end"
        ```
        - Line 5 adds text to the end of `sb`. It also returns a reference
        to `sb`, which is ignored. Line 6 also adds text to the end of `sb`
        and returns a reference to `sb`. This time the reference is
        stored in `same`. This means `sb` and `same` point to the same
        object and would print out the same value.
            - The exam won’t always make the code easy to read by
            having only one method per line. What do you think this
            example prints?
                ```java
                4: StringBuilder a = new StringBuilder("abc");
                5: StringBuilder b = a.append("de");
                6: b = b.append("f").append("g");
                7: System.out.println("a=" + a);
                8: System.out.println("b=" + b);
                ```
                - There’s only one `StringBuilder` object here. We know that 
                because `new StringBuilder()` is called only once. On line 5, 
                there are two variables referring to that object which has a 
                value of `"abcde"`. On line 6, those two variables are still 
                referring to that same object, which now has a value
                 of `"abcdefg"`. Incidentally, the assignment back to `b` does 
                 absolutely nothing. `b` is already pointing to that `StringBuilder`.
                    - **Creating a StringBuilder**
                    There are three ways to construct a `StringBuilder`:
                        ```java
                        StringBuilder sb1 = new StringBuilder();
                        StringBuilder sb2 = new StringBuilder("animal");
                        StringBuilder sb3 = new StringBuilder(10);
                        ```

                        - The first says to create a `StringBuilder` containing an empty
                        sequence of characters and assign `sb1` to point to it. The
                        second says to create a `StringBuilder` containing a specific
                        value and assign `sb2` to point to it. The first two examples tell 
                        Java to manage the implementation details. The final
                        example tells Java that we have some idea of how big the
                        eventual value will be and would like the `StringBuilder` to
                        reserve a certain capacity, or number of slots, for characters.
- **Important `StringBuilder` Methods**
These are the ones you might see on the exam.
    - **Using Common Methods**
    These four methods work exactly the same as in the `String` class.
        ```java
        var sb = new StringBuilder("animals");
        String sub = sb.substring(sb.indexOf("a"), sb.indexOf("al"));
        int len = sb.length();
        char ch = sb.charAt(6);
        System.out.println(sub + " " + len + " " + ch); // anim 7 s
        ```
        -  The `indexOf()` method calls return `0` and `4`, respectively. 
        The `substring()` method returns the `String` starting with
         index `0` and ending right before index `4`.
         The `length()` method returns `7` because it is the number of
        characters in the `StringBuilder` rather than an index. Finally,
        `charAt()` returns the character at index `6`. Here, we do start
        with `0` because we are referring to indexes. 
        Notice that `substring()` returns a `String` rather than a
        `StringBuilder`. That is why `sb` is not changed
            - **Appending Values**
            The `append()` method is by far the most frequently used
            method in `StringBuilder`. It adds the parameter to the 
            `StringBuilder` and returns a reference to the current
            `StringBuilder`. One of the method signatures is as follows:
                ```java
                public StringBuilder append(String str)
                ```
                There are more than 10 method signatures that look similar but
                take different data types as parameters, such as `int`, `char`, etc.
                -  you can write code like this:
                    ```java
                    var sb = new StringBuilder().append(1).append('c');
                    sb.append("-").append(true);
                    System.out.println(sb); // 1c-true
                    ```
                    The `append()` method is called directly after the constructor. 
                    By having all these method signatures, you can just call `append()`
                     without having to convert your parameter to a `String` first.
                    - **Applying Code Points**
                    The `codePointAt()`, `codePointBefore()`, and `codePointCount()`
                    methods from `String` are also available on `StringBuilder`.
                    There’s one more method you need to know for code points
                    that is only on `StringBuilder`:
                        ```java
                        public StringBuilder appendCodePoint(int codePoint)
                        ```
                        - It works like the `append()` method in the previous section
                        except <b>it takes an integer representing the Unicode value,
                        converts it to a character, and appends</b> it to the `StringBuilder`.
                            ```java
                            var sb = new StringBuilder()
                                .appendCodePoint(87).append(',')
                                .append((char)87).append(',')
                                .append(87).append(',')
                                .appendCodePoint(8217);
                            System.out.println(sb); // W,W,87,â€™
                            ```
- **Inserting Data**
The `insert()` method adds characters to the `StringBuilder` at
the requested index and returns a reference to the current
`StringBuilder`. There are lots of method signatures for different
 types. Here’s one:
    ```java
    public StringBuilder insert(int offset, String str)
    ```
    - Pay attention to the offset in these examples. It is the index
    where we want to insert the requested parameter.
        ```java
        3: var sb = new StringBuilder("animals");
        4: sb.insert(7, "-"); // sb = animals-
        5: sb.insert(0, "-"); // sb = -animals-
        6: sb.insert(4, "-"); // sb = -ani-mals-
        7: System.out.println(sb);
        ```
        - Line 4 says to insert a dash at index `7`, which happens to be
        the end of the sequence of characters. Line 5 says to insert
        a dash at index `0`, which happens to be the very beginning.
        Finally, line 6 says to insert a dash right before index `4`. The
        exam creators will try to trip you up on this. As we add and
        remove characters, their indexes change. When you see a
        question dealing with such operations, draw what is going
        on using available writing materials so you won’t be
        confused.
            - **Deleting Contents**
            The `delete()` method is the opposite of the `insert()` method. It removes 
            characters from the sequence and returns a reference to the current
             `StringBuilder`. The `deleteCharAt()` method is convenient when you want 
             to delete only one character. The method signatures are as follows:
                ```java
                public StringBuilder delete(int startIndex, int endIndex)
                public StringBuilder deleteCharAt(int index)
                ```
                - The following code shows how to use these methods:
                    ```java
                    var sb = new StringBuilder("abcdef");
                    sb.delete(1, 3);    // sb = adef
                    sb.deleteCharAt(5); // exception
                    ```
                    First, we delete the characters starting with index `1` and
                    ending right before index `3`. This gives us `adef`. Next, we ask
                    Java to delete the character at position `5`. However, the
                    remaining value is only four characters long, so it throws a
                    `StringIndexOutOfBoundsException`.

                    - The `delete()` method is more flexible than some others when
                    it comes to array indexes. If you specify a second parameter 
                    that is past the end of the `StringBuilder`, Java will just assume 
                    you meant the end. That means this code is legal:
                        ```java
                        var sb = new StringBuilder("abcdef");
                        sb.delete(1, 100); // sb = a
                        ```
                        - **Reversing**
                        The `reverse()` method reverses the characters in the 
                        sequences and returns a reference to the current 
                        `StringBuilder`. The method signature is as follows:
                            ```java
                            public StringBuilder reverse()
                            ```
                            The following code shows how to use this method:
                            ```java
                            var sb = new StringBuilder("ABC");
                            sb.reverse();
                            System.out.println(sb); // CBA
                            ```
- **Replacing Portions**
The `replace()` method works differently for `StringBuilder`
than it did for `String`. The method signature is as follows:
    ```java
    public StringBuilder replace(int startIdx, int endIdx, String newStr)
    ```
    - The following code shows how to use this method:
        ```java
        var builder = new StringBuilder("pigeon dirty");
        builder.replace(3, 6, "sty");
        System.out.println(builder); // pigsty dirty
        ```
        First, Java deletes the characters starting with index `3` and
        ending right before index `6`. This gives us `pig dirty`. Then
        Java inserts the value `sty` in that position.

        - In this example, the number of characters removed and
        inserted are the same. What do you think this does?
            ```java
            var builder = new StringBuilder("pigeon dirty");
            builder.replace(3, 100, "");
            System.out.println(builder);
            ```
            It prints `pig`. Remember, the method is first doing a logical 
            delete. The `replace()` method **allows specifying a second 
            parameter that is past the end of the `StringBuilder`**.
            That means only the first three characters remain.