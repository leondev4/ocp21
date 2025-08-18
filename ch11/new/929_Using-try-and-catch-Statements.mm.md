---

markmap:
  initialExpandLevel: 2
---

# **Using try and catch Statements**
- Java uses a `try` statement to separate the logic that 
might throw an exception from the logic to handle that
exception. 
  - The syntax of a `try` statement
      - ```js
        try {//try keyword and open and close braces are required
            // Protected code
        } catch (Exception e){    //catch keyword
                                  // The type of exception being caught
                                  //The identifier of the exception object
            // Exception handler 
        }
        ```
      - The code in the `try` block is run normally. If any of the statements throws an exception
that can be caught by the exception type listed in the `catch` block, the `try` block stops 
running, and execution goes to the `catch` statement. If none of the statements in the 
`try` block throws an exception that can be caught, the `catch` clause is not run.
- ```js
  3: void explore() {
  4:   try {
  5:     fall();
  6:     System.out.println("never get here");
  7:   } catch (RuntimeException e) {
  8:     getUp();
  9:  }
  10:   seeAnimals();
  11: }
  12: void fall() { throw new RuntimeException(); }
  ```
  - The curly braces are required 
  for `try` and `catch` blocks.
    - ```
      try
      // DOES NOT COMPILE
        fall();
      catch (Exception e)
        System.out.println("get up");
      ```
    - ```
      try { // DOES NOT COMPILE
        fall();
      } //catch o ﬁnally is missing
      ```
# **Chaining catch Blocks**
-  First, you must be able to recognize if the exception is a checked 
or an unchecked exception. Second, you need to determine
whether any of the exceptions are subclasses of the others.
- `catch` blocks from top to bottom. First place the child exceptions 
and then the parent exception; if there is no inheritance relationship 
then they can go in any order.
  - ```js
    class AnimalsOutForAWalk extends RuntimeException {}
    class ExhibitClosed extends RuntimeException {}
    class ExhibitClosedForLunch extends ExhibitClosed {}
    public void test() {
      try {
            seeAnimal();
      }catch (AnimalsOutForAWalk e){ // first catch block
            System.out.print("try back later");
      }catch(ExhibitClosedForLunch e){ // second catch block
            System.out.println("having lunch");
      }catch (ExhibitClosed e){ // third catch block
            System.out.print("not today");
      }catch(RuntimeException e){ // fourth catch block
            System.out.println("RuntimeException");
      }catch(Exception e){ //fifth catch block
            System.out.println("Exception");
          }
    }
    ```
    - The order of the first and second `catch` blocks could be 
    reversed because the exceptions don’t inherit from each
     other.  For the others, there is an inheritance relationship.
    - A rule exists for the order of the `catch` blocks. Java looks 
    at them in the order they appear. If it is impossible for one 
    of the `catch` blocks to be executed, a compiler error about 
    unreachable code occurs.
    - It is not possible to execute all the catch blocks, only one, 
    if it is compatible with the exception thrown.
- The following example shows exception types that do inherit from each other:

  ```js
  public void visitMonkeys() {
      try {
        seeAnimal();
      }catch(ExhibitClosedForLunch e){ // Subclass exception
        System.out.print("try back later");
      }catch(ExhibitClosed e){        // Superclass exception
        System.out.print("not today");
    }
  }
  ```
  - The reverse does not work
    - ```
      public void visitMonkeys() {
        try {
          seeAnimal();
        } catch (ExhibitClosed e){
          System.out.print("not today");
        }catch(ExhibitClosedForLunch e){ // DOES NOT COMPILE
            System.out.print("try back later");
        }
      }
      ```
    - If the more specific `ExhibitClosedForLunch` exception is 
    thrown, the `catch` block for `ExhibitClosed` runs— ­which 
    means there is no way for the second `catch` block to
     ever run. Java correctly tells you there is an unreachable 
     catch block.
- ```js
  public void visit() {
    try {
      }catch(IllegalArgumentException e){
      }catch(NumberFormatException e){ // DOES NOT COMPILE
    }
  }
  ```
  - Second `catch` block unreachable
  - Remember
    - `NumberFormatException` is
      a subclass of `IllegalArgumentException`
    - `FileNotFoundException` is a subclass of `IOException`
- Remember that an exception defined by the `catch` 
statement is only in scope for that `catch` block. 
  - ```js
    public void visit() {
      try {
      }catch(NumberFormatException e1) {
        System.out.println(e1);
      } catch (IllegalArgumentException e2) {
        System.out.println(e1); // DOES NOT COMPILE
      }
    }
    ```
# **Applying a Multi-­catch Block**
  -  A _multiple catch block_ allows multiple types of exceptions (with no 
  inheritance relationship) to be caught by the same catch block.
- ```js
  String name =  null;
  try{
      name.toString();
  }catch(NullPointerException | IndexOutOfBoundsException e){
      System.out.println(e);
  }
  ```
  -  If you wanted, you could still have a second `catch` block for `Exception` 
  in case you want to handle other types of exceptions differently.
  - It is like a regular `catch` clause, except two or more exception types are specified, 
  separated by a pipe. The pipe (`|`) is also used as the “or” operator, making it easy 
  to remember that you can use either/or of the exception types. Notice how there is 
  only one variable name in the catch clause. Java is saying that the variable named 
  `e` can be of type `NullPointerException` or `IndexOutOfBoundsException`.
- Exceptions can be listed in any order within the `catch` clause. However, 
the variable name must appear only once and at the end.
  - ```
    catch(Exception1 e | Exception2 e | Exception3 e) // DOES NOT COMPILE`
    catch(Exception1 e1 | Exception2 e2 | Exception3 e3) // DOES NOT COMPILE
    catch(Exception1 | Exception2 | Exception3 e)// CORRECT SYNTAX
    ```
- _Multi-­catch_ to be used for exceptions that aren't related, and it 
prevents you from specifying redundant types in a multi-­catch.
  - ```js
    try {
      throw new IOException();
    }catch(FileNotFoundException | IOException p) {} // DOES NOT COMPILE
    ```
    - The exception `FileNotFoundException` is 
already caught by the alternative `IOException`
    - Correct:
      ```js
      try {
        throw new IOException();
      } catch (IOException e) {}
      ```