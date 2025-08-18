---

markmap:
  initialExpandLevel: 1
---

# **Creating and Manipulating Strings** <!-- fold all -->
  -  A string is basically a sequence of characters
      - ```java
          String name = "Fluffy";
          ```
          - All give you a reference variable named 
          `name` pointing to the `String` object 
          `"Fluffy"`. They are subtly different, as you 
          see later in this chapter. For now, just
           remember that the `String` class is special 
          and doesn’t need to be instantiated with `new`.
      - ```java
          String name = new String("Fluffy");
         ```
          - Since a `String` is a sequence characters,
          you probably won’t be surprised to hear that it 
          implements the interface `CharSequence`.
      - ```java 
          String name = """ 
                          Fluffy""";
        ```
          - If we use `==` on all three variables, 
          only the first and third give us `true`.
  - **Concatenating** 
    In Chapter 2, "Operators," you learned how to add numbers. `1 + 2` 
    is clearly `3`. But what is `"1"+"2"`? It’s `"12"` because Java
    combines the two `String` objects. Placing one `String` before the 
    other `String` and combining them is called string concatenation.
      - Rules
            1. If both operands are numeric, `+` means numeric addition.
            2. If either operand is a `String`, `+` means concatenation.
            3. The expression is evaluated left to right.
        - ```java
           System.out.println(1 + 2);         //3
           System.out.println("a" + "b");     //ab
           System.out.println("a" + "b" + 3); //ab3
           System.out.println(1 + 2 + "c");   //3c
           System.out.println("c" + 1 + 2);   //c12
           System.out.println("c" + null);    //cnull
           ```
          - The first example uses the first rule. Both operands are
            numbers, so we use normal addition. The second example is
            simple string concatenation, described in the second rule.
            - In the fourth example, we start with the third rule,      which tells
             us to consider `1 + 2`. Both operands are   numeric, so the first 
             rule tells us the answer is `3`. Then we have `3 + "c"`, which 
             uses the second rule to give us `"3c"`.
          - The third example combines the second and third rules. Since we 
          start on the left, Java figures out what `"a" + "b"` evaluates to.
           You already know that one: it’s `"ab"`. Then Java looks at the 
           remaining expression of `"ab" + 3`. The second rule tells us to 
           concatenate since one of the operands is a `String`.
            - The fifth example shows the importance of the third rule.
              First we have `"c" + 1`, which uses the second rule to give us
              `"c1"`. Then we have `"c1" + 2`, which uses the second rule
              again to give us `"c12"`.
          - Finally, the last example shows how `null` is represented as a string when concatenated or printed, giving us `"cnull"`.
- ```java
      int three = 3;
      String four = "4";
      System.out.println(1 + 2 + three + four);
  ```
  - In this example, we start with the third rule, which tells us to consider 
  `1 + 2`. The first rule gives us `3`. Next, we have `3 + three`. Since `three` 
  is of type `int`, we still use the first rule,  giving us `6`.      Then, we have 
  `6 + four`. Since `four` is of type  `String`, we switch to the       second rule and
   get a final answer of  `"64"`. When you see questions    like this, just take
    your time and   check the types.
       - There is one more thing to know about concatenation, but
        it is easy. In this example, you just have to remember what
        `+=` does. Keep in mind, `s += "2"` means the same
         thing as `s = s + "2"`.
         - ```java 
            4: var s = "1";   // s currently holds "1"
            5: s += "2";     // s currently holds "12"
            6: s += 3;       // s currently holds "123"
            7: System.out.println(s);  // 123
             ```
              - On line 5, we are “adding” two strings, which means we
              concatenate them. Line 6 tries to trick you by adding a number,
               but it’s just like we wrote `s = s + 3`. We know that a string 
               “plus” anything else means to use concatenation.
- **To review the rules one more time: use numeric 
addition if two numbers are involved, use concatenation
 otherwise, and evaluate from left to right.**