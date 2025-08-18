---

markmap:
  initialExpandLevel: 1
---
# **Applying Scope to Classes**
- The rule for instance variables: they are available as soon as they are defined and last for the entire 
lifetime of the object itself. The rule for class, aka static, variables: they go into scope when declared 
like the other variable types. However, they stay in scope for the entire life of the program.
- Try to figure out the type of the four variables and when they
go into and out of scope.
  - ```java
    1: public class Mouse{
    2:    final static int MAX_LENGTH=5;
    3:    int length;
    4:    public void grow(int inches) {
    5:      if (length < MAX_LENGTH) {
    6:        int newSize=length + inches;
    7:        length=newSize;
    8:      }
    9:    }
    10: }
    ```
    - In this class, we have one class variable, `MAX_LENGTH`; one instance
    variable, `length`; and two local variables, `inches` and `newSize`. The
    `MAX_LENGTH` variable is a class variable because it has the `static`
    keyword in its declaration. In this case, `MAX_LENGTH` goes into scope
    on line 2 where it is declared. It stays in scope until the program ends.
    - Next, `length` goes into scope on line 3 where it is declared. It stays
    in scope as long as this `Mouse` object exists. `inches` goes into scope
    where it is declared on line 4. It goes out of scope at the end of the
    method on line 9. `newSize` goes into scope where it is declared on
    line 6. Since it is defined inside the `if` statement block, it goes out
    of scope when that block ends on line 8.
- **Reviewing Scope**
  - **Local variables:** In scope from declaration to the end of the block
    **Method parameters:** In scope for the duration of the method
    **Instance variables:** In scope from declaration until the object is eligible for garbage collection
    **Class variables:** In scope from declaration until the program ends