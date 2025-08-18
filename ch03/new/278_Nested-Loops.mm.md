---

markmap:
  initialExpandLevel: 1
---
# **Nested Loops**
- A nested loop is a loop that contains another loop, including 
`while`, `do/while`, `for`, and for-each loops.
  - ```java
    int[][] myComplexArray = {{5,2,1,3}, {3,9,8,9}, {5,7,12,7}};

    for(int[] mySimpleArray : myComplexArray){
     for(int i = 0; i < mySimpleArray.length; i++){
        System.out.print(mySimpleArray[i]+"\t");
      }
    System.out.println();
    }
    ```
    - Notice that we intentionally mix a `for` loop and a for-each
loop in this example. The outer loop will execute a total of
three times. Each time the outer loop executes, the inner
loop is executed four times.
      - When we execute this code, we see the following output:
        ```java
        5   2    1    3
        3   9    8    9
        5   7    12   7
        ```
- Nested loops can include `while` and `do/while`, as shown in
this example.
  - ```java
    int hungryHippopotamus = 8;
    while(hungryHippopotamus > 0){
      do{
        hungryHippopotamus -= 2;
      }while (hungryHippopotamus>5);
      hungryHippopotamus--;
      System.out.print(hungryHippopotamus+", ");
    }
    ```
    - The first time this loop executes, the inner loop repeats until the value of 
    `hungryHippopotamus` is `4`. The value will then be decremented to `3`,
     and that will be the output at the end of the first iteration of the outer loop.
    - On the second iteration of the outer loop, the inner `do/while` will be executed 
    once, even though `hungryHippopotamus` is already not greater than `5`. As 
    you may recall, `do/while` statements always execute the body at least once. 
    This will reduce the value to `1`, which will be further lowered by the decrement 
    operator in the outer loop to `0`. Once the value reaches `0`, the outer loop 
    will terminate. The result is that the code will output the following:
    `3, 0,`
- **Adding Optional Labels**
  - A label is an optional pointer to the head of a statement 
  that allows the application flow to jump to it or break from 
  it. It is a single identifier that is followed by a colon (`:`).
    - ```java
      int[][] myComplexArray = {{5,2,1,3}, {3,9,8,9}, {5,7,12,7}};
      
      OUTER_LOOP: for (int[] mySimpleArray : myComplexArray) {
        INNER_LOOP:for (int i = 0; i < mySimpleArray.length; i++){
          System.out.print(mySimpleArray[i]+"\t");
        }
        System.out.println();
      }
      ```
      - Labels follow the same rules as identifiers. For readability, we show 
      them in snake_case, with uppercase letters and underscores between 
      words. **When dealing with only one loop, labels do not add any 
      value**.  they are extremely useful in nested structures.