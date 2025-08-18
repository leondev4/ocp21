---

markmap:
  initialExpandLevel: 1
---
# **Creating Local Variables**
- Before you can use a variable, it needs a value.
  - A _local variable_ is a variable defined within a constructor, method,
or initializer block
- **Final Local Variables**
  - The `final` keyword can be applied to local variables and is equivalent to declaring constants.
  - 
    ```java
    5: final int y = 10;
    6: int x = 20;
    7: y = x + 10; // DOES NOT COMPILE
    ```
    - Line 7 triggers a compiler error since the value cannot be modified.
  - The `final` modifier can also be applied to local variable references.
      -   ```java
          5: final int[] favoriteNumbers = new int[10];
          6: favoriteNumbers[0] = 10;
          7: favoriteNumbers[1] = 20;
          8: favoriteNumbers = null; //DOES NOT COMPILE
          ```
      - Notice that we can modify the content, or data, in the array. The
      compiler error isn’t until line 8, when we try to change the value of
      the reference `favoriteNumbers`.
- **Uninitialized Local Variables**
- Local variables do not have a default value and must be
initialized before use. The compiler will report an error if
you try to read an uninitialized variable. 
  - ```java
    4: public int notValid() {
    5:    int y = 10;
    6:    int x;
    7:    int reply = x + y; // DOES NOT COMPILE
    8:    return reply;
    9: }
    ```
    - The `y` variable is initialized to `10`. By contrast, `x` is not 
    initialized before it is used in the expression on line 7,
      and the compiler generates an error. 
- The compiler recognizes variables that have been initialized 
after their declaration but before they are used. 
  - ```java
    public int valid() {
      int y = 10;
      int x; // x is declared here
      x = 3; // x is initialized here
      int z; // z is declared here but never  
            // initialized or used
      int reply = x + y;
      return reply;
    }
    ```
    - In this example, `x` is declared, initialized, and used in 
    separate lines.**Also, `z` is declared but never used,
    so it is not required to be initialized.**
- The compiler recognizes initializations that
are more complex.
  - ```java
    public void findAnswer(boolean check) {
      int answer; int otherAnswer; int onlyOneBranch;
      if (check) {
        onlyOneBranch = 1;
        answer = 1;
      }else {
        answer = 2;
      }
      System.out.println(answer);
      System.out.println(onlyOneBranch); // DOES NOT COMPILE
    }
    ```
    - The `answer` variable is initialized in both branches of the if statement, 
    so the compiler is perfectly happy. 
    The `otherAnswer` variable is not initialized but never used, and the 
    compiler is equally as happy. Remember,**the compiler is concerned 
    only if you try to use uninitialized local variables**.
    The `onlyOneBranch` variable is initialized only if `check` happens to be
    `true`. The compiler knows there is the possibility for `check` to be
    `false`, resulting in uninitialized code, and gives a compiler error.
  -   ```java
      int i=0; int x;
      if(i==0)
        x=1;
      x=x+1; // DOES NOT COMPILE
      ```
      -   ```java
          final int i=0; int x;
          if(i==0)
            x=1;
          x=x+1; // COMPILES
          ```