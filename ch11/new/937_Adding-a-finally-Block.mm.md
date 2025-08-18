---

markmap:
  initialExpandLevel: 2
---

# Adding a finally Block
- The `try` statement also lets you run code at the end with
 a `finally` clause, regardless of whether an exception is 
 thrown.
  - There are two paths through code with both a `catch` and a `finally`. If an 
  exception is thrown, the `finally` block is run after the `catch` block. If no 
  exception is thrown, the `finally` block is run after the `try` block completes.
    - The syntax of a `try` statement with `finally`
      - ```js
        try {
          // Protected code
          // The catch block is optional when using finally 
          // (not for checked exceptions, but you can use throws)
        } catch (Exception e) {
              // Exception handler
        } finally { // finally keyword
              //The finally block always executes, whether
              // or not an exception occurs.
        }
        ```
- ```js
  25: try { // DOES NOT COMPILE
  26:   fall();
  27: } finally {
  28:   System.out.println("all better");
  29: }catch (Exception e) {
  30:   System.out.println("get up");
  31: }
  ```
  - Does not compile because the `catch` and 
  `finally` blocks are in the wrong order.
    - ```
      33:try{ // DOES NOT COMPILE
      34:     fall();
      35:  }
      ```
      - Does not compile because there
must be a `catch` or `finally` block
- ```js
  37:try {
  38:      fall();
  39: }finally{
  40:       System.out.println("all better");
  41: }
  ```
  -  It is just fine. The `catch` block is not 
required if `finally` is present.
- ```
  public static void main(String[] unused) {
    StringBuilder sb = new StringBuilder();
      try {
          sb.append("t");
      } catch (Exception e) {
          sb.append("c");
      } finally {
          sb.append("f");
      }
    sb.append("a");
    System.out.print(sb.toString());
  }
  ```
  - This code outputs `tfa`. The `try` block is executed. Since no 
  exception is thrown, Java goes straight to the `finally` block. 
  Then the code after the `try` statement is run.
- The `finally` block will always be executed, regardless 
of whether the code completes successfully, unless 
you execute `System.exit(0)`.
  - ```js
    12:int goHome() {
    13:    try {
    14:      // Optionally throw an exception here
    15:      System.out.print("1");
    16:      return -­1;
    17:     } catch (Exception e) {
    18:       System.out.print("2");
    19:       return -­2;
    20:     } finally {
    21:        System.out.print("3");
    22:        return -­3;
    23:     }
    24:}
    ```
    - If line 14 does not throw an exception, print: `13`
If line 14 throws an exception, print: `32`
    - The method always returns `-3`. Because the `finally` block is 
    executed shortly before the method completes, it interrupts 
    the `return` statement from inside both the `try` and `catch` blocks.
- While a `finally` block will always execute, it may not terminate.
 For example, if an exception is thrown inside it.
  - When `System.exit()` is called in the `try` or 
  `catch` block, the `finally` block does not run.
# **Try-­with-­Resources**
-  Java includes the _try-­with-­resources_ statement to automatically close all 
resources opened in a `try` clause. This feature is also known as _automatic 
resource management_, because Java automatically takes care of the closing.
- Example:
  - ```js
    4: void read(){
    5:   try (FileInputStream is = new FileInputStream("myfile.txt")) {
    6:      // Read file data
    7:   } catch (IOException e) {
    8:       e.printStackTrace();
    9:   }
    10:}
    ```
    - Using a try-­with-­resources statement, we guarantee that as soon as a connection 
    passes out of scope, Java will attempt to close it within the same method
    - Behind the scenes, the compiler replaces a try-­with-­resources block with a `try` 
    and `finally` block. We refer to this “hidden” `finally` block as an implicit `finally` block 
    since it is created and used by the compiler automatically. You can still create a 
    `finally` or `catch` blocks when using a try-­with-­resources statement; just be aware 
    that the **implicit one will be called first**.
- When **multiple resources are opened, they are closed in the reverse of 
the order in which they were created**. Also, notice that **parentheses are 
used to list those resources, and semicolons are used to separate the 
declarations**. This works just like declaring multiple indexes in a for loop.
  - You learned that a normal try statement must have one or more `catch` blocks or a `finally` block. 
  In a try-­with-­resources statement neither of these is required, although a developer may add 
  both. For the exam, you need to know that the implicit `finally` block runs before any 
  programmer-­coded ones.
- Syntax of a basic try-­with-­resources
  - ```js
    try (var in = new FileInputStream("data.txt"); //Required semicolon between resources
      var out = new FileOutputStream("output.txt");) {//optional semicolon at the end
        //resources are in and out
        //Protected code
    } //Resources are closed here in reverse order.
    catch (IOException e) { //Optional if add it in throws clause
        // Exception handler
    } finally {  //Optional
        // finally block` 
    }
    ```
- `catch` block is optional with a try-­with-­resources statement. For 
example, we can rewrite the previous `read()` example so that
the method declares the exception to make it even shorter: 
  - ```js
    4: void read() throws IOException{
    5:    try (FileInputStream is = new FileInputStream("myfile.txt")) {
    6:       // Read file data
    7:   }
    10:}
    ```
- **Only classes that implement the `AutoCloseable` or `Closable` 
interface can be used in a try-with-resources statement**.
  - ```
    interface AutoCloseable{
      public void close() throws Exception;
    }
    ``` 
  - ```js
    interface Closeable extends AutoCloseable{
      public void close() throws IOException;
    }
    ```
  - ```java
    public class MyFileClass implements AutoCloseable {
      private final int num;
      public MyFileClass(int num) { this.num = num; }
        public void close() {
          System.out.println("Closing: " + num);
      } }
      ```
- **Declaring Resources:** While try-­with-­resources does support 
declaring multiple variables, each variable must be declared in 
a separate statement
  - ```java
    try (MyFileClass is = new MyFileClass(1), // DOES NOT COMPILE
      os = new MyFileClass(2)) { //Missing semicolon, also the 
                                 //type in the second resource
    }
    ```
  - ```java
    try(MyFileClass ab = new MyFileClass(1), // DOES NOT COMPILE
        MyFileClass cd = new MyFileClass(2)) { //missing semicolon
    }
    ```
  - You can declare a resource using `var`:
    ```js
    try (var f = new BufferedInputStream(new FileInputStream("it.txt"))) {
      // Process file
    }
    ```
 # **Scope of Try-­with-­Resources**
- Resources created in the `try` clause are in scope only
within the `try` block. Implicit finally runs before any 
catch/finally blocks that you code yourself
  - ```js
    3: try (Scanner s = new Scanner(System.in)) {
    4:   s.nextLine(); //THE SCOPE OF s IS ONLY HERE
    5: } catch(Exception e) {
    6:   s.nextInt(); // DOES NOT COMPILE
    7: } finally {
    8:   s.nextInt(); // DOES NOT COMPILE
    9: }
    ```
# **Following Order of Operations**
- In try-­with-­resources statement, the resources are closed
 in the reverse of the order in which they are created.
 -  ```js
    try(MyFileClass bookReader = new MyFileClass(1);
        MyFileClass movieReader = new MyFileClass(2)) {
      System.out.println("Try Block");
      throw new RuntimeException();
    } catch (Exception e) {
      System.out.println("Catch Block");
    } finally {
      System.out.println("Finally Block");
    }
    ```
    -  The output is as follows:
        ```
        Try Block
        Closing: 2
        Closing: 1
        Catch Block
        Finally Block
        ```
# **Applying Effectively Final**
- It is possible to declare resources ahead of time, provided
they are **marked `final` or effectively final**. The syntax uses 
the resource name in place of the resource declaration,
separated by a semicolon (`;`).
  - ```js
    11: public static void main(String... xyz) {
    12:   final var bookReader = new MyFileClass(4);
    13:   MyFileClass movieReader = new MyFileClass(5);
    14:   try (bookReader;
    15:        var tvReader = new MyFileClass(6);
    16:        movieReader){
    17:     System.out.println("Try Block");
    18:   } finally {
    19:   System.out.println("Finally Block");
    20:  }
    21: }
    ```
    - Line 12 declares a `final` variable `bookReader`, while line 13 declares an effectively final variable 
    `movieReader`. Both of these resources can be used in a try-­with-­resources statement.
    We know `movieReader` is effectively final because it is a local variable that is assigned a value 
    only once. The test for effectively final is that if we insert the `final` keyword when the variable 
    is declared, the code still compiles.
    Lines 14 and 16 use the new syntax to declare resources in a try-­with-­resources statement, 
    using just the variable name and separating the resources with a semicolon (`;`).
    - On execution, the code prints the following:
      ```
      Try Block
      Closing: 5
      Closing: 6
      Closing: 4
      Finally Block
      ```
  - If you see a question on the exam that uses a try-­with-­resources 
  statement with a variable not declared in the try clause, make 
  sure it is effectively final. 
    - ```js
      31: var writer = Files.newBufferedWriter(path);
      32:   try (writer) { // DOES NOT COMPILE
      33:     writer.append("Welcome to the zoo!");
      34:   }
      35: writer = null;
      ```
      - The `writer` variable is reassigned on line 35. Since it is not an 
      effectively final variable, it cannot be used in a try-­with-­resources 
      statement on line 32.
  - The other place the exam might try to trick you 
  is accessing a resource after it has been closed. 
    - ```js
      41: var writer = Files.newBufferedWriter(path);
      42: writer.append("This write is permitted but a really bad idea!");
      43: try (writer) {
      44:   writer.append("Welcome to the zoo!");
      45: }
      46: writer.append("This write will fail!"); // IOException
      ```
      - This code compiles but throws an exception on line 46 with the message `Stream
      closed`. 
      While it is possible to write to the resource before the try-­with-­resources statement, it is 
      not afterward.