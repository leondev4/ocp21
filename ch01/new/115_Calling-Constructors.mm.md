---

markmap:
  initialExpandLevel: 1
---
# **Calling Constructors**
- To create an instance of a class, all you have to do is write `new`
before the class name and add parentheses after it.
    - ```java
      Park p =new Park();
      ```
      - First you declare the type that you’ll be creating (`Park`) and give the
      variable a name (`p`). This gives Java a place to store a reference to
      the object. Then you write `new Park()` to actually create the object.
- `Park()` looks like a method since it is followed by parentheses.
It’s called a _constructor_, which is a special type of method that
creates a new object. You can define your own constructor.
    - ```java
      public class Chick {
      public Chick(){
          System.out.println("in constructor");
        }
      }
      ```
      - There are two key points to note about the constructor: the name
       of the constructor matches the name of the class, and there’s no
        return type. 
- You may see a method like this on the exam:
  - ```java
    public class Chick {
      public void Chick() { }// NOT A CONSTRUCTOR
    }
    ```
    - When you see a method name beginning with a capital letter and
    having a return type. It is not a constructor since there’s a return
    type. It’s a regular method that does compile but will not be called
    when you write `new Chick()`.
- The purpose of a constructor is to initialize fields, although you
can put any code in there. Another way to initialize fields is to
 do so directly on the line on which they’re declared. This example
  shows both approaches:
  - ```java
    public class Chicken {
      int numEggs = 12; //initialize on line
      String name;
      public Chicken() {
        name = "Duke"; //initialize in constructor
      }
    }
    ```
- **Reading and Writing Member Fields**
  - It’s possible to read and write instance variables directly from the
caller.
    - ```java
      public class Swan {
        int numberEggs;                          //instance variable
        public static void main(String[] args) {
          Swan mother = new Swan();
          mother.numberEggs = 1;                 //set variable
          System.out.println(mother.numberEggs); //read variable
        }
      }
      ```
      - The “caller” in this case is the `main()` method, which could be in the
same class or in another class. This class sets `numberEggs` to `1` and
then reads `numberEggs` directly to print it out.
  - You can even read values of already initialized fields on a line
initializing a new field.
    - ```java
      1: public class Name{
      2:   String first = "Theodore";
      3:   String last = "Moose";
      4:   String full = first + last;
      5: }
      ```
      - Lines 2 and 3 both write to fields. Line 4 both reads and writes data. 
      It reads the fields `first` and `last`. It then writes the field `full`.