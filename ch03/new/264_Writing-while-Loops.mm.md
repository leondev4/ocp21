---

markmap:
  initialExpandLevel: 1
---
# **Writing `while` Loops**
- A loop is a repetitive control structure that can execute a
  statement or block of code multiple times in succession.
  - The following loop executes exactly 10 times:
    - ```java 
      int counter = 0;
      while(counter<10){
        double price = counter * 10;
        System.out.println(price);
        counter++;
      }
      ```
- The simplest repetitive control structure is the `while` statement, 
described in [Figure 3.8](https://1drv.ms/i/c/c83cfca51d5c2032/EU6cEVmt7D9Ig08NQlEt1zABHLaZWuEwM-PF-HCsDkOfyg?e=Qfx5ZX). Like all repetition control structures, it 
has a termination condition, implemented as a `boolean`
expression. A loop will continue as long as this expression
evaluates to `true`.
  - A `while` loop is similar to an `if` statement in that it is composed of a `boolean` expression 
  and a statement, or a block of statements. During execution, the `boolean` expression is 
  evaluated before each iteration of the loop and exits if the evaluation returns `false`.
    - Let’s see how a loop can be used to model a mouse eating a meal:
      ```java
      int roomInBelly = 5;
      void eatCheese(int bitesOfCheese) {
         while(bitesOfCheese > 0 && roomInBelly > 0){
          bitesOfCheese--;
          roomInBelly--;
        }
        System.out.println(bitesOfCheese+" pieces of cheese left");
      }
      ```
      - This method takes an amount of food—in this case, cheese
      —and continues until the mouse has no room in its belly or
      there is no food left to eat. With each iteration of the loop,
      the mouse “eats” one bite of food and loses one spot in its
      belly. By using a compound `boolean` statement, you ensure
      that the `while` loop can end for either of the conditions.
- One thing to remember is that a `while` loop may terminate
after its first evaluation of the `boolean` expression, how many 
times is `Not full!` printed in the following example?
  - ```
    int full = 5;
    while(full < 5){
      System.out.println("Not full!");
      full++;
    }
    ```
    - The answer? Zero! On the first iteration of the loop, the condition is reached, 
    and the loop exits. **This is why `while` loops are often used in places 
    where you expect zero or more executions of the loop.**

- **The `do/while` Statement**
The second form a `while` loop can take is called a `do/while`
loop, which, like a `while` loop, is a repetition control structure 
with a termination condition and statement, or a block of
statements, as shown in the [Figure 3.9](https://1drv.ms/i/c/c83cfca51d5c2032/EYJez7xEiilDlsO6ujerUx0BYUpuNMLmXZZqzKrAclxrYQ?e=tftC8o)
  - A `do/while` loop guarantees that the statement or block will be executed at 
  least once. For example, what is the output of the following statements?
    - ```java
      int lizard = 0;
      do {
        lizard++;
      }while(false);
      System.out.println(lizard);
      ```
      - Java will execute the statement block first and then check
the loop condition. Even though the loop exits right away,
the statement block is still executed once, and the program
prints `1`.
- **Infinite Loops**
Example
  - ```java
    int pen = 2;
    int pigs = 5;
    while(pen < 10)
      pigs++;
    ```
    - You may notice one glaring problem with this statement: it will never end. 
    The variable `pen` is never modified, so the expression `(pen < 10)` will 
    always evaluate to `true`. The result is that the loop will never end, creating 
    what is commonly referred to as an infinite loop. **An infinite loop is a 
    loop whose termination condition is never reached  during runtime.**
    - Make sure the loop condition, or the variables the condition is dependent 
    on, are changing between executions. Then, ensure that the termination 
    condition will be eventually reached in all circumstances.