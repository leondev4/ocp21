---

markmap:
  initialExpandLevel: 1
---

# **Important `String` Methods** 
- **Searching for Substrings**
The `startsWith()` and `endsWith()` methods look at whether the 
provided value matches part of the String. There is also an 
overloaded `startsWith()` that specifies where in the `String` to start
looking. The `contains()` method isn’t as particular; it looks
for matches anywhere in the `String`. The method signatures
are as follows:
    - ```java
      public boolean startsWith(String prefix)
      public boolean startsWith(String prefix, int fromIndex)
      public boolean endsWith(String suffix)
      public boolean contains(CharSequence charSeq)
      ```
        - The following code shows how to use these methods:
            ```java 
            System.out.println("abc".startsWith("a")); // true
            System.out.println("abc".startsWith("A")); // false
            
            System.out.println("abc".startsWith("b", 1)); // true
            System.out.println("abc".startsWith("b", 2)); // false
            
            System.out.println("abc".endsWith("c")); //true
            System.out.println("abc".endsWith("a")); //false
            
            System.out.println("abc".contains("b")); //true
            System.out.println("abc".contains("B")); //false
            ```
            - Again, nothing surprising here. Java is doing a case-
            sensitive check on the values provided. Note that the
            `contains()` method is a convenience method so you don’t
            have to write `str.indexOf(otherString) != -1`.
                - **Replacing Values**
                The `replace()` method does a simple search and replace on
                the string. There’s a version that takes `char` parameters as
                well as a version that takes `CharSequence` parameters. The
                method signatures are as follows:
                    - ```java
                        public String replace(char oldChar, char newChar)
                        public String replace(CharSequence target, CharSequence replacement)
                       ```
                       - The following code shows how to use these methods:
                            ```java
                            System.out.println("abcabc".replace('a', 'A')); // AbcAbc
                            System.out.println("abcabc".replace("a", "A")); // AbcAbc
                            ```
                            - The first example uses the first method signature, passing
                            in `char` parameters. The second example uses the second
                            method signature, passing in `String` parameters.
- **Removing Whitespace**
These methods remove blank space from the beginning
and/or end of a `String`. The `strip()` and `trim()` methods
remove whitespace from the beginning and end of a `String`.
In terms of the exam, whitespace consists of spaces along
with the `\t` (tab) and `\n` (newline) characters. Other
characters, such as `\r` (carriage return), are also included
in what gets trimmed. The `strip()` method does everything
that `trim()` does, but it supports Unicode.

    - Additionally, the `stripLeading()` method removes whitespace
    from the beginning of the `String` and leaves it at the end.
    The `stripTrailing()` method removes whitespace from the end
     of the `String` and leaves it at the beginning. The method 
     signatures are as follows:
        - ```java
          public String strip()
          public String stripLeading()
          public String stripTrailing()
          public String trim()
          ```
            - The following code shows how to use these methods:
                ```java
                System.out.println("  abc  ".strip()); // abc
                System.out.println("\t   a b c\n".strip()); // a b c

                String text = " abc\t ";
                System.out.println(text.trim().length()); // 3
                System.out.println(text.strip().length()); // 3
                System.out.println(text.stripLeading().length()); // 5
                System.out.println(text.stripTrailing().length()); // 4
                ```
                - First, **remember that `\t` is a single character**. The backslash
                escapes the `t` to represent a tab. The first example prints `"abc"` 
                because removes whitespace at the beginning and end. 
                The second example gets rid of the leading tab, subsequent 
                spaces, and the trailing newline. It leaves the spaces that are in
                 the middle of the string.
                    - The remaining examples just print the number of
                    characters remaining. You can see that `trim()` and `strip()`
                    leave the same three characters `"abc"` because they remove
                    both the leading and trailing whitespace. The `stripLeading()`
                    method only removes the one whitespace character at the
                    beginning of the `String`. It leaves the tab and space at the
                    end. The `stripTrailing()` method removes these two
                    characters at the end but leaves the character at the
                    beginning of the `String`.

- **Working with Indentation**
Now that Java supports text blocks, it is helpful to have
methods that deal with indentation.
    ```java
    public String indent(int numberSpaces)
    public String stripIndent()
    ```
    - The `indent()` method adds the same number of blank spaces
    to the beginning of each line if you pass a positive number.
    If you pass a negative number, it tries to remove that number 
    of whitespace characters from the beginning of the line. If you
     pass zero, the indentation will not change.

        - This seems straightforward enough. However, `indent()` also
        normalizes whitespace characters. What does *normalizing*
        whitespace mean, you ask? **First, a line break is added to
        the end of the string if not already there. Second, any line
        breaks are converted to the `\n` format.** Regardless of
        whether your operating system uses `\r\n` (Windows) or `\n`
        (Mac/Unix), Java will standardize on `\n` for you.
            - The `stripIndent()` method is useful when a `String` was built
            with concatenation rather than using a text block. **It gets
            rid of all incidental whitespace. This means that all nonblank 
            lines are shifted left so the same number of whitespace
             characters are removed from each line and the first character 
             that remains is not blank.** Like `indent()`, `\r\n` is turned into `\n`. 
             However, the `stripIndent()` method **does not add a trailing line
              break if it is missing.**
                - **TABLE 4.1** Rules for `indent()` and `stripIndent()` <br> &nbsp;

                    | &nbsp;Method&nbsp;|&nbsp; Indent change &nbsp;|&nbsp;Normalizes <br/>&nbsp;existing line&nbsp; <br> &nbsp;breaks &nbsp;|&nbsp;Adds line &nbsp; <br> &nbsp;break at <br>&nbsp;end if <br>&nbsp;missing&nbsp;|
                    |-|-|-|-|
                    |`indent(n)` <br> where `n > 0` | Adds `n` spaces to <br>beginning of each <br> line |Yes|Yes|
                    | `indent(n)` <br> where `n == 0` | No change |Yes|Yes|
                    |`indent(n)` <br> where `n < 0`|Removes up to `n` <br> whitespace characters<br>from each line|Yes|Yes|
                    |`stripIndent()`|Removes all<br>leading incidental<br>whitespace|Yes|No|

                    - The following code shows how to use these methods.
                        ```java
                        10: var block = """
                        11:             a
                        12:              b
                        13:             c""";
                        14: var concat = " a\n"
                        15:            + "  b\n"
                        16:            + " c";
                        17: System.out.println(block.length());                // 6
                        18: System.out.println(concat.length());               // 9
                        19: System.out.println(block.indent(1).length());      // 10
                        20: System.out.println(concat.indent(-1).length());    // 7
                        21: System.out.println(concat.indent(-4).length());    // 6
                        22: System.out.println(concat.stripIndent().length()); // 6
                        ```
                        - Lines 10–16 create similar strings using a text block and a
                        regular `String`, respectively. We say “similar” because `concat`
                        has a whitespace character at the beginning of each line
                        while `block` does not.
                        Line 17 counts the six characters in `block`, which are the
                        three letters, the blank space before `b`, and the `\n` after `a`
                        and `b`. Line 18 counts the nine characters in `concat`, which
                        are the three letters, one blank space before `a`, two blank
                        spaces before `b`, one blank space before `c`, and the `\n` after `a`
                        and `b`.  
                        On line 19, we ask Java to add a single blank space to each
                        of the three lines in `block`. However, the output says we
                        added 4 characters rather than 3 since the length went
                        from 6 to 10. This mysterious additional character is thanks
                        to the line termination normalization. Since the text block
                        doesn’t have a line break at the end, `indent()` adds one!
                        - On line 20, we remove one whitespace character from each
                        of the three lines of `concat`. This gives a length of seven. We
                        started with nine, got rid of three characters, and added a
                        trailing normalized new line.
                        On line 21, we ask Java to remove four whitespace
                        characters from the same three lines. Since there are not
                        four whitespace characters, Java does its best. The single
                        space is removed before `a` and `c`. Both spaces are removed
                        before `b`. The length of six should make sense here; we
                        removed one more character here than on line 20.
                        Finally, line 22 uses the `stripIndent()` method. All of the
                        lines have at least one whitespace character. **Since they do
                        not all have two whitespace characters, the method gets rid
                        of only one character per line. Since no new line is added
                        by `stripIndent()`,** the length is six, which is three less than
                        the original nine.

    - If you call `indent()` with a negative number and try to
    remove more whitespace characters than are present at
    the beginning of the line, Java will remove all that it can
    find.

- **Checking for Empty or Blank `String`s**
Java provides convenience methods for whether a `String`
has a length of zero or contains only whitespace
characters.
    - ```java
      public boolean isEmpty()
      public boolean isBlank()
      ```
        - The following code shows how to use these methods:
        <br>
            ```java
            System.out.println(" ".isEmpty()); // false
            System.out.println("".isEmpty()); // true
            System.out.println(" ".isBlank()); // true
            System.out.println("".isBlank()); // true
            ```
            - The first line prints `false` because the `String` is not empty; it
            has a blank space in it. The second line prints `true` because
            this time, there are no characters in the `String`. The final
            two lines print `true` because there are no characters other
            than whitespace present.
- **Method Chaining**
Ready to put together everything you just learned about?
    -  It is common to call multiple methods, as shown here:
        ```java
        var start = "\t AniMaL   \n  ";
        var trimmed = start.trim();                 //"AniMaL"
        var lowercase = trimmed.toLowerCase();      //"animal"
        var result = lowercase.replace('a', 'A');   //"AnimAl"
        System.out.println(result);
        ```
        - This is just a series of `String` methods. Each time one is
        called, the returned value is put in a new variable. There
        are four `String` values along the way, and `AnimAl` is output.
            - However, on the exam, there is a tendency to cram as much code as possible into a 
            small space. You’ll see code using a technique called  method chaining. Here’s an example:
                ```java
                String result = "\t AniMaL   \n  ".trim().toLowerCase().replace('a','A');
                System.out.println(result);
                ```
                - This code is equivalent to the previous example. It also
                creates four `String` objects and outputs `AnimAl`. **To read code
                that uses method chaining, start at the left and evaluate the
                first method. Then call the next method on the returned
                value of the first method. Keep going until you get to the
                semicolon.**
                    - What do you think the result of this code is?
                        ```java
                        5: String a = "abc";
                        6: String b = a.toUpperCase();
                        7: b = b.replace("B", "2").replace('C', '3');
                        8: System.out.println("a=" + a);
                        9: System.out.println("b=" + b);
                        ```
                        - On line 5, we set `a` to point to `"abc"` and never pointed a to
                        anything else. Since none of the code on lines 6 and 7
                        changes `a`, the value remains `"abc"`. 
                        However, `b` is a little trickier. Line 6 has `b` pointing to `"ABC"`,
                        which is straightforward. On line 7, we have method chaining. 
                        First, `"ABC".replace("B", "2")` is called. This returns `"A2C"`. Next, 
                        `"A2C".replace(’C’, ’3’)` is called. This returns `"A23"`.  Finally, `b` 
                        changes to point to this returned `String`. When line 9 executes, 
                        `b` is `"A23"`.