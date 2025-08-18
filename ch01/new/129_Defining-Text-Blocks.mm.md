---

markmap:
  initialExpandLevel: 1
---
# **Defining Text Blocks**
- What if we want to have a `String` with something more complicated?
For example: 
  ```java
  "Java Study Guide"
    by Jeanne & Scott
  ```
  - The syntax `\"` lets you say you want a `"` rather than to end the `String`, and `\n` 
  says you want a new line. Both of these are called _escape characters_ because
  the backslash provides a special meaning. 
    - ```java
      String eyeTest = "\"Java Study Guide\"\n    by Jeanne & Scott";
      ```
    - It is hard to read.
- Java has _text blocks_, also known as multiline strings.
- ```
  String textBlock = """
      │"Java Study Guide"
      │   by Jeanne & Scot""";
  ```
    - A text block starts and ends with three double quotes (`"""`), and the
contents don’t need to be escaped.
      - Notice how the type is still `String`.  `String` methods apply to text blocks too.
  - **Essential whitespace** is part of your `String` and is important to you. 
  **Incidental whitespace** just happens to be there to make the code easier
  to read. You can reformat your code and change the amount of incidental
  whitespace without any impact on your `String` value (see the [Figure 1.3](https://1drv.ms/i/c/c83cfca51d5c2032/ESMN0Nt9N89IiLM1unqECr4BqQpN3WGnV2stjLvw1TNYaQ?e=iaQoDc) ).
    - **Imagine a vertical line drawn on the leftmost non-whitespace character 
    in your text block. Everything to the left of it is incidental whitespace, and 
    everything to the right is essential whitespace.**
      - Essential is shown in the result, incidental is not.
    - How many lines does this output, and how 
    many incidental and essential whitespace
    characters begin each line?
      ```java
      14: String pyramid = """
      15:   *
      16:  * *
      17: * * *
      18: """;
      19: System.out.print(pyramid);
      ```
      - There are four lines of output. Lines 15–17 have stars. Line 18 
      is a line without any characters.**The closing triple `"` would have 
      needed to be on line 17 if we didn’t want that blank line.** There
      are no incidental whitespace characters here. The closing `"""` on 
      line 17 or 18 are the leftmost characters, so the line is drawn at the 
      leftmost position. Line 15 has two essential whitespace characters
      to begin the line, and line 16 has one. That whitespace fills in the
      line drawn to match line 17 or 18.
  - You can use a text block with a method that takes a String. 
    - ```java
      public String label(String title, String author) {
        return"""
              Book:
              """+ title + " by " + author;
      }
      ```
        - ```java
          public void prepare() {
            String labelled=label("""
                Java Study Guide
                For Java 21
                2024 Edition""", "Jeanne & Scott");
            System.out.println(labelled);
          }
          ```
          - It prints:
            ```
            Book:
            Java Study Guide
            For Java 21
            2024 Edition by Jeanne & Scott
            ```
          - You must note the newline after `Book:`
- Text block formatting
  - **Formatting&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**          
    - **Meaning in regular String&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**   
      - **Meaning in text block**      
  - ```java
    \"
    \"""
    \"\"\"
    -------------------
    Space (at end
    of line)
    ------------------
    \s


    ------------------
    \ (at end of line 
     without space)        
    ```
    - ```java
      "
      n/a – Invalid
      """
      -------------------------------
      Space

      -------------------------------
      Two spaces (\s is a space and
      preserves leading space on the
      line)
      -------------------------------
      n/a - Invalid

      ```
      - ```java
        "
        """
        """
        ------------------------
        Ignored

        ------------------------
        Two spaces
            """
            ex \s"""
        -------------------------
        Ommits new line on that line

        ```
  - Do you see why this doesn’t compile?
  `String block = """doe"""; // DOES NOT COMPILE`
  **Text blocks require a line break after the opening `"""`, making this one invalid.**
  - How many lines do you think
are in this text blocks? 
    - ```java
      String block = """
        doe \
        deer""";
      ```
      - Just one. The output is `doe deer` since the `\` tells Java not to add a
new line before `deer`.
    - ```java
      String block = """
        doe\
        deer""";
      ```
      - Just one. The output is `doedeer` since the `\` tells Java not to add a
new line before `deer`.
  - Determine the number of lines
    ```java
    String block = """
      doe \n
      deer
      """;
    ```
    - This time we have four lines. Since the text block has the closing `"""`
    on a separate line, we have three lines for the lines in the text block
    plus the explicit `\n`. After the `doe` there is an whitespace.
  - What do you think this
    outputs?
    - ```java
      String block = """
        "doe\"\"\"
        \"deer\"""
        """;
      System.out.println("*");
      System.out.print(block);
      System.out.println("*");
      ```
      - The answer is:
        ```java
          *
          "doe"""
          "deer"""
          *
        ```
        - All of the `\"` escape the `"`. There is one space of essential whitespace
        on the `doe` and `deer` lines. All the other leading whitespace is
        incidental whitespace. 
    