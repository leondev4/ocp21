---

markmap:
  initialExpandLevel: 1
---
# **Inferring the Type with `var`**
- You have the option of using the keyword `var` instead of the 
type when declaring local variables under certain conditions.
  - Here’s an example:
      ```java
      public class Zoo {
        public void whatTypeAmI() {
          var name = "Hello";
          var size = 7;
        }
      }
      ```
      - The formal name of this feature is _local variable type inference_.
- _Local variable_ means just what it sounds like. You can use 
this feature only for local variables.
  - Example:
    ```java
    public class VarKeyword {
      var tricky = "Hello"; // DOES NOT COMPILE
    }
    ```
    - The variable `tricky` is an instance variable. Local variable type 
    inference works with local variables and not instance variables.
- _type inference_ means what it sounds like. When you type
 `var`, you are instructing the compiler to determine the type
  for you.**The compiler looks at the code on the line of 
  the declaration and uses it to infer the type.**
    - Example:
      ```java
      7: public void reassignment() {
      8:   var number = 7;
      9:   number = 4;
      10:  number = "five"; // DOES NOT COMPILE
      11: }
      ```
      - On line 8, the compiler determines that we want an `int` 
      variable. On line 9, we have no trouble assigning a
      different `int` to it. On line 10, Java has a problem. We’ve 
      asked it to assign a `String` to an `int` variable. This is not
      allowed. It is equivalent to typing this:
        ```java
        int number = "five";
        ```
- **When discussing `var`, we are going to assume that a
declaration and initialization statement of the variable
is completed on a single line.**
  - ```java
    public void breakingDeclaration() {
      var silly
          =1;
    }`
    ```
    - This example is valid and does compile, but we consider the
    **declaration and initialization of `silly` to be happening on 
    the same line**.
- Do you think the following compiles?
  -   ```java
      3: public void doesThisCompile(boolean check) {
      4:    var question;
      5:    question = 1;
      6:    var answer;
      7:    if(check){
      8:      answer = 2;
      9:    }else{
      10:      answer = 3;
      11:   }
      12:   System.out.println(answer);
      13: }
      ```
      - The code does not compile. Remember that for local variable type inference, the 
      compiler looks only at the line with the declaration. Since `question` and `answer` 
      are not assigned values on the lines where they are defined, the compiler does not
       know what to make of them. For this reason, both lines 4 and 6 do not compile.
- **Now we know the initial value used to determine the
 type needs to be part of the same statement.** Can you
figure out why these two statements don’t compile?
  - ```java
    4: public void twoTypes() {
    5:   int a, var b = 3; //DOES NOT COMPILE
    6:   var a, b = 3;     //DOES NOT COMPILE
    7:   var n = null;     //DOES NOT COMPILE
    8: }
    ```
    - Line 5 wouldn’t work even if you replaced var with a real type. All the 
    types declared on a single line must be the same type and share the 
    same declaration. We couldn’t write `int a`, `int b = 3;` either.
    Line 6 shows that**you can’t use `var` to define two variables on the
    same line.**
    Line 7 is a single line. The compiler is being asked to infer the type
    of `null`. **The designers of Java decided it would be better not to 
    allow `var` for `null`.**
- Do you see why this does not compile?
  - ```java
    public int addition(var a,var b) {//DOES NOT COMPILE
      return a + b;
    }
    ```
    - In this example, `a` and `b` are method parameters. These are not local
    variables. Be on the lookout for `var` used with constructor parameters, 
    method parameters, or instance variables. Using `var` in one of these
    places is a good exam trick to see if you are paying attention. Remember 
    that `var` is used only for local variable type inference!
- `var` is not a reserved word and allowed to be used as an
identifier. It is considered a reserved type name that  means 
it **cannot be used to define a type, such as a `class`,
`interface`, `record` or `enum`.**
  
  - ```java
    package var;
    public class Var {
      public void var() {
        var var = "var";
      }
      public void Var() {
        Var var = new Var();
      }
    }
    ```
    - Believe it or not, this code does compile. Java is case sensitive, so `Var` doesn’t 
    introduce any conflicts as a class name. Naming a local variable `var` is legal.
    - `var` is a "contextual keyword", then can be used as identifier. So, `var var= "hello";` 
    would actually be a valid statement because the first `var` will be replaced with 
    `String` by the compiler.