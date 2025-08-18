---

markmap:
  initialExpandLevel: 1
---
# **Using Method References**
- Method references are another way to make the code easier 
to read, such as simply mentioning the name of the method.
  - Consider the following functional interface:
    ```java
    public interface LearnToSpeak {
      void speak(String sound);
    }
    ```
    - Using lambdas
      ```java
      LearnToSpeak learner=s -­> System.out.println(s);
      ```
    - Usig method reference
      ```java
      LearnToSpeak learner=System.out::println;
      ```
    - The **`::`** operator tells Java to call the `println()` method later.
- **Calling `static` Methods**
  - We use a functional interface that converts a `double` to a `long`:
    ```java
    interface Converter {
      long round(double num);
    }
    ```
  - ```java
    14: Converter methodRef = Math::round;
    15: Converter lambda = x -> Math.round(x);
    16:
    17: System.out.println(methodRef.round(100.1)); // 100
    ```
    - On line 14, we reference a method with one parameter, and Java knows that it’s like a lambda with one 
    parameter. Additionally, Java knows to pass that parameter to the method.
    Wait a minute. You might be aware that the `round()` method is overloaded—­it can take a `double` or a 
    `float` and returns `long` or `int`. How does Java know that we want to call the version with a `double`? 
    With both lambdas and method references, Java infers information from the _context_. In this case, we 
    said that we were declaring a `Converter`, which has a method taking a `double` parameter. **Java looks
    for a method that matches that description. If it can’t find it or finds multiple matches, then
    the compiler will report an error. The latter is sometimes called an _ambiguous_ type error.**
- **Calling Instance Methods 
  on a Particular Object**
  - We have a functional interface that checks if a 
  `String` starts with a specified value:
    ```java
    interface StringStart {
      boolean beginningCheck(String prefix);
    }
    ```
  - ```java
    18: var str = "Zoo"; //effectively final or final
    19: StringStart methodRef = str::startsWith;
    20: StringStart lambda = s -> str.startsWith(s);
    21:
    22: System.out.println(methodRef.beginningCheck("A")); // false
    ```
    - Line 19 shows that we want to call `str.startsWith()` and 
    pass a single parameter to be supplied at runtime
  - In this example, we create a functional interface with a method 
  that doesn’t take any parameters but returns a value:
    ```java
    interface StringChecker {
      boolean check();
    }
    ```
  - We implement it by checking if the `String` is empty:
    ```java
    18: var str = "";
    19: StringChecker methodRef = str::isEmpty;
    20: StringChecker lambda = () -> str.isEmpty();
    21:
    22: System.out.print(methodRef.check()); // true
    ```
    - Since the method on `String` is an instance method, we call 
    the method reference on an instance of the `String` class.
  - While all method references can be turned into lambdas, the opposite 
  is not always `true`. For example, consider this code:

    ```java
    var str = "";
    StringChecker lambda = () -> str.startsWith("Zoo");
    ```
    - How might we write this as a method reference? You might try one of the following:

      ```java
      StringChecker methodReference = str::startsWith; // DOES NOT COMPILE

      StringChecker methodReference = str::startsWith("Zoo"); // DOES NOT COMPILE
      ```
    - Neither of these works! While we can pass the `str` as part of the method reference, there’s 
    no way to pass the `"Zoo"` parameter with it. Therefore, it is not possible to write this lambda
     as a method reference.
- **Calling Instance Methods on a Parameter**
  - ```
    interface StringParameterChecker {
      boolean check(String text);
    }
    ```
    We can implement this functional interface as follows:
    ```java
    23: StringParameterChecker methodRef = String::isEmpty;
    24: StringParameterChecker lambda = s->s.isEmpty();
    25:
    26: System.out.println(methodRef.check("Zoo")); // false
    ```
    - Line 23 says the method that we want to call is declared in `String`. It looks like a `static` method, but it 
    isn’t. Instead, Java knows that `isEmpty()` is an instance method that does not take any parameters. 
    Java uses the parameter supplied at runtime as the instance on which the method is called.
    Compare lines 23 and 24 with lines 19 and 20 of our instance example. They look similar, although 
    one references a local variable named `str`, while the other only references the functional interface 
    parameters.
  - ```java
    interface StringTwoParameterChecker {
      boolean check(String text, String prefix);
    }
    ```
    Pay attention to the parameter order when reading the implementation:
    ```java
    26: StringTwoParameterChecker methodRef = String::startsWith;
    27: StringTwoParameterChecker lambda = (s, p)-> s.startsWith(p);
    28:
    29: System.out.println(methodRef.check("Zoo", "A")); // false
    ```
    - Since the functional interface takes two parameters, Java has to figure out what they
    represent. The first one will always be the instance of the object for instance methods. Any
    others are to be method parameters.
    Remember that line 26 may look like a `static` method, but it is really a method reference 
    declaring that the instance of the object will be specified later. Line 27 shows some of the 
    power of a method reference. We were able to replace two lambda parameters this time.
- **Calling Constructors**
  - A constructor reference is a special type of method reference that uses `new`
   instead of a method and instantiates an object.
   - ```java
     interface EmptyStringCreator {
       String create();
     }
     ```
     To call this, we use `new` as if it were a method name:
     ```java
     30: EmptyStringCreator methodRef = String::new;
     31: EmptyStringCreator lambda = ()-> new String();
     32:
     33: var myString = methodRef.create();
     34: System.out.println(myString.equals("Snake")); // false
     ```
    - ```
      interface StringCopier {
        String copy(String value);
      }
      ```
      In the implementation, notice that line 32 in the following example has 
      the same method reference as line 30 in the previous example:
      ```java
      32: StringCopier methodRef = String::new;
      33: StringCopier lambda = x->new String(x);
      34:
      35: var myString = methodRef.copy("Zebra");
      36: System.out.println(myString.equals("Zebra")); // true
      ```
      - This means you can’t always determine which method can be called by looking at the
      method reference. Instead, you have to look at the context to see what parameters are 
      used and if there is a return type. In this example, Java sees that we are passing a 
      `String` parameter and calls the constructor of `String` that takes such a parameter.