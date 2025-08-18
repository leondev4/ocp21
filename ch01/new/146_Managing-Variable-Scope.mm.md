---

markmap:
  initialExpandLevel: 1
---
# **Managing Variable Scope**
-  How many variables do you see that are scoped to this
method?
    - ```java
      public void eat(int piecesOfCheese) {
        int bitesOfCheese = 1;
      }
      ```
      - There are two variables with local scope. The `bitesOfCheese` variable
      is declared inside the method. The `piecesOfCheese` variable is a
      method parameter. Neither variable can be used outside of where it
      is defined.
- Local variables can never have a scope larger than the
method they are defined in. However, they can have a 
smaller scope.
  - ```java
    3: public void eatIfHungry(boolean hungry) {
    4:   if(hungry){
    5:     int bitesOfCheese=1;
    6:   }// bitesOfCheese goes out of scope here
    7:   int cheese = bitesOfCheese; //DOES NOT COMPILE
    8: }
    ```
    - `hungry` has a scope of the entire method, `bitesOfCheese` has a smaller 
    scope. It is only available for use in the if statement because it is declared
    inside of it. When you see a set of braces (`{}`) in the code, it means you
    have entered a new block of code. Each block of code has its own scope. 
    When there are multiple blocks, you match them from the inside out. In our 
    case, the `if` statement block begins at line 4 and ends at line 6. The 
    method’s block begins at line 3 and ends at line 8.
    - Since `bitesOfCheese` is declared in an `if` statement block, the scope is
    limited to that block. When the compiler gets to line 7, it complains that it 
    doesn’t know anything about this `bitesOfCheese` thing and gives an error.
- Remember that blocks can contain other blocks. These
smaller contained blocks can reference variables defined
in the larger scoped blocks, but not vice versa.
  - ```java
    16: public void eatIfHungry(boolean hungry) {
    17:   if(hungry){
    18:     int bitesOfCheese=1;
    19:     {
    20:       var teenyBit=true;
    21:       var t=bitesOfCheese;
    22:     }
    23:   }
    24:   var teeny=teenyBit; //DOES NOT COMPILE
    25: }
    ```
    - The variable defined on line 18 is in scope until the block 
    ends on line 23. Using it in the smaller block from lines 19 
    to 22 is fine. The variable defined on line 20 goes out of 
    scope on line 22. Using it on line 24 is not allowed.
- **Tracing Scope**
- See if you can figure out on which line each of the 
five local variables goes into and out of scope.
  - ```java
    11: public void eatMore(boolean hungry,int amountOfFood){
    12:   int roomInBelly = 5;
    13:   if(hungry){
    14:     var timeToEat = true;
    15:     while(amountOfFood> 0){
    16:       int amountEaten = 2;
    17:       roomInBelly = roomInBelly - amountEaten;
    18:       amountOfFood = amountOfFood - amountEaten;
    19:     }
    20:   }
    21:   System.out.println(amountOfFood);
    22: }
    ```
    - The first step in figuring out the scope is to identify the blocks of code. In 
    this case, there are three blocks.
Method: first line 11, last line 22. `if`: first line 13, last line 20.`while`: first 
line 15, last line 19   
    - `hungry` and `amountOfFood` are method parameters, so they are 
    available for the entire method. This means their scope is lines 11 
    to 22. The variable `roomInBelly` goes into scope on line 12 because 
    that is where it is declared. It stays in scope for the rest of the 
    method and goes out of scope on line 22. The variable `timeToEat` 
    goes into scope on line 14 where it is declared. It goes out of scope
    on line 20 where the if block ends. Finally, the variable `amountEaten` 
    goes into scope on line 16 where it is declared. It goes out of scope
    on line 19 where the `while` block ends.