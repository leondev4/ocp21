---

markmap:
  initialExpandLevel: 1
---
# **Working with Records**
- A record is a special type of data-oriented class in which the compiler inserts boilerplate 
code for you. (See [Figure 7.6](https://1drv.ms/i/c/c83cfca51d5c2032/EYB95hSHoHVMoRWRKHWJ2IQBuLCNyHySnqc9EFVRqU48zw?e=JfNXzi)).
The compiler inserts useful implementations of the `Object` methods `equals()`, `hashCode()`,
and `toString()`.
  - A `record` implicitly extends `java.lang.Record` class but it is not allowed to have an 
  `extends` clause. A `record` is implicitly `final`.
   If we have an application with hundreds of POJOs, a `record` can save us valuable 
   time.
    - Creating an instance of a `Crane` and printing some fields is easy:
      ```java
      var mommy = new Crane(4, "Cammy");
      System.out.println(mommy.numberEggs()); // 4
      System.out.println(mommy.name()); // Cammy
      ```
      A few things should stand out here. First, we never defined any constructors 
      or methods in our `Crane` declaration. How does the compiler know what to
      do? Behind the scenes, it creates a constructor for you with the parameters 
      in the same order in which they appear in the record declaration.
        - Omitting or changing the type order will lead to compiler errors:
          ```java
          var mommy1 = new Crane("Cammy", 4); // DOES NOT COMPILE
          var mommy2 = new Crane("Cammy");    // DOES NOT COMPILE
          ```
          For each field, it also creates an accessor as the field name, plus a 
          set of parentheses. Finally, records override a number of methods in
          `Object` for you.
            - **Members Automatically Added to Records**
              **Constructor:** A constructor with the parameters in the same order as the record declaration
              **Accessor method:** One accessor for each field
              **`equals()`:** A method to compare two elements that returns true if each field is equal in terms of `equals()`
              **`hashCode()`:** A consistent `hashCode()` method using all of the fields
              **`toString()`:** A `toString()` implementation that prints each field of the record in a convenient, easy-to-read format
                - ```java
                  var father = new Crane(0, "Craig");
                  System.out.println(father); //Crane[numberEggs=0, name=Craig]
                  
                  var copy = new Crane(0, "Craig");
                  System.out.println(copy); //Crane[numberEggs=0, name=Craig]
                  System.out.println(father.equals(copy)); // true
                  System.out.println(father.hashCode() + ", " + copy.hashCode()); // 1007, 1007
                  ```
                    - It is legal to have a record without any fields. It is simply declared with the `record` 
                    keyword and parentheses:
                      ```java
                      public record Crane() {}
                      ```
                      This is not the kind of thing you’d use in your own code, but it could come up on 
                      the exam.
- **Declaring Constructors**
We have two ways to create constructors in records.
  - **The Long Constructor**
  First, we can just declare the constructor the compiler normally inserts automatically, which we refer to as 
  the long constructor (same types and names of parameters).
    ```java
    public record Crane(int numberEggs, String name) {
      public Crane(int numberEggs, String name) {
        if (numberEggs < 0) throw new IllegalArgumentException();
        this.numberEggs = numberEggs;
        this.name = name; //you can use this keyword
      }
    }
    ```
    - The compiler will not insert a constructor if you define one with the same list of parameters in the same order. 
    Since each field is `final`, the constructor must set every field. For example, this record does not compile:
      ```java
      public record Crane(int numberEggs, String name) {
        public Crane(int numberEggs, String name) {} // DOES NOT COMPILE
      }
      ```
      - **Compact Constructors**
        Luckily, the authors of Java added the ability to define a compact constructor for records. A compact constructor 
        is a special type of constructor used for records to process validation and transformations succinctly. It takes no
        parameters and **implicitly sets all fields**. [Figure 7.7](https://1drv.ms/i/c/c83cfca51d5c2032/EXh6DXIoHmZAmtQ31Cz3v1IBTuJidRaB7ocZaqyZWHJmaw?e=9MPQsG) shows an example of a compact constructor. **You cannot 
        use `this` keyword inside it.**
          - Great! Now we can check the values we want, and we don’t have to list all the constructor parameters and trivial
            assignments. **Java will execute the full constructor after the compact constructor.** You should also remember 
            that a **compact constructor is declared without parentheses**, as the exam might try to trick you on this. As 
            shown in Figure 7.7, we can even transform constructor parameters.
            - **Transforming Parameters**
              Compact constructors give you the opportunity to apply transformations to any of the input values. See if you can
              figure out what the following compact constructor does:
              ```java
              public record Crane(int numberEggs, String name) {
                public Crane {
                  if (name == null || name.length() < 1)
                    throw new IllegalArgumentException();
                  name = name.substring(0, 1).toUpperCase() + name.substring(1).toLowerCase();
                }
              }
              ```
              -  It validates the string, then formats it such that only the first letter is capitalized. As before, **Java calls the
              full constructor after the compact constructor but with the modified constructor parameters.**
                  - While compact constructors can modify the constructor parameters, they cannot modify the fields  of the 
                  record. For example, this does not compile:
                    ```java
                    public record Crane(int numberEggs, String name) {
                      public Crane {
                        this.numberEggs = 10; // DOES NOT COMPILE
                      }
                    }
                    ```
                    Removing the `this` reference allows the code to compile, as the constructor parameter is modified instead.
- **Overloaded Constructors**
You can also create overloaded constructors that take a completely different list of 
parameters. They are more closely related to the long-form constructor and don’t 
use any of the syntactical features of compact constructors.
  ```java
  public record Crane(int numberEggs, String name) {
    public Crane(String firstName, String lastName) {
      this(0, firstName + " " + lastName); //must be the first call
    }
  }
  ```
  - The first line of an overloaded constructor must be an explicit call to another constructor via `this()`. 
  If there are no other constructors, the long constructor must be called. Also, unlike compact
  constructors, you can only transform the data on the first line. **After the first line, all of the fields 
  will already be assigned, and the object is immutable.**
      ```java
      public record Crane(int numberEggs, String name) {
        public Crane(int numberEggs, String firstName, String lastName) {
          this(numberEggs + 1, firstName + " " + lastName);
          numberEggs = 10; // NO EFFECT (applies to parameter, not instance field)
          this.numberEggs = 20; // DOES NOT COMPILE (final field)
        }
      }
      ```
      - **Only the long constructor, with fields that match the record declaration, supports setting 
      field values with a `this` reference. Compact and overloaded constructors do not.**
        - You also can’t declare two record constructors that call each other infinitely or as a cycle.
          ```java
          public record Crane(int numberEggs, String name) {
            public Crane(String name) {
              this(1); // DOES NOT COMPILE
            }
            public Crane(int numberEggs) {
              this(""); // DOES NOT COMPILE
            }
          }
          ```
          - **Understanding Record Immutability**
            As you saw, records don’t have setters. Every field is inherently `final` and cannot be modified 
            after it has been written in the constructor. To “modify” a record, you have to make a new 
            object and copy all of the data you want to preserve.
            ```java
            var cousin = new Crane(3, "Jenny");
            var friend = new Crane(cousin.numberEggs(), "Janeice");
            ```
            - Just as interfaces are implicitly `abstract`, **records are also implicitly `final`.** The `final` 
            modifier is optional but assumed.
              ```java
              public final record Crane(int numberEggs, String name) {}
              ```
              **Like enums, that means you can’t extend or inherit a record.**
              ```java
              public record BlueCrane() extends Crane {} // DOES NOT COMPILE
              ```
              - **Also like enums, a record can implement a regular or sealed interface**, provided 
              it implements all of the abstract methods.
                ```java
                public interface Bird {}
                public record Crane(int numberEggs, String name) implements Bird {}
                ```
                - While instance members of a record are `final` (they can be declared only in the 
                definition of record), the `static` members are not required to be. For example, 
                the following defines an immutable record in which a `static` value is updated 
                every time a record is created.
                  ```java
                  public record WhoopingCrane(String name, int position) {
                    private static int counter = 0;
                    public WhoopingCrane(String name) {
                      this(name, counter++);
                    }
                  }
                  ```
- **Using Pattern Matching with Records**
New to Java 21, records have been updated to support pattern matching. Initially, 
you might think this is not actually something new. After all, we could use records
with pattern matching in Java 17. The new feature is really about the members of 
the record, rather than the record itself.
  -  Let’s try an example:
      ```java
      1: record Monkey(String name, int age) {}
      2:
      3: public class Zoo {
      4:   public static void main(String[] args) {
      5:
      6:     Object animal = new Monkey("George", 3);
      7:      //animal instanceof Monkey m
      8:     if(animal instanceof Monkey(String name, int myAge)) {
      9:       System.out.println("Hello " + name);
      10:      System.out.println("Your age is " + myAge);
      11: } } }
      ```
      - Wait, what’s going on in line 8? It looks like we redeclared the declaration of the 
      record. Don’t worry, we didn’t! What we did do, though, is define a pattern that is 
      compatible with the `Monkey` record. We also named two elements, `name` and 
      `myAge` (this is not mandatory). This allows us to use them as local variables on 
      line 9 and 10, without a reference variable.
        - For the exam, you should be aware of the following rules when working with pattern 
        matching and records:
          \* If any field declared in the record is included, then all fields must be included.
          \* The order of fields must be the same as in the record.
          \* The names of the fields do not have to match.
          \* At compile time, the type of the field must be compatible with the type declared in the record.
          \* The pattern may not match at runtime if the record
          supports elements of various types.
            - Working with records and pattern matching has some similarities to casting. For 
            example, the compiler will disallow things that it knows to be invalid. There are 
            some differences, though, that we will get to shortly.
              Quiz time! Given our previous `Monkey` record, which of the following lines of 
              code do not compile?
              ```java
              11: if(animal instanceof Monkey myMonkey) {}
              12: if(animal instanceof Monkey(String n, int a) myMonkey) {}
              13: if(animal instanceof Monkey(String n, long a)) {}
              14: if(animal instanceof Monkey(Object n, int a)) {}
              ```
              - The first example compiles, as this is just simple pattern matching. Line 12 does 
              not compile, though. <strong style="color:green">You can name the record or its fields, but not both</strong>. Line 
              13 also does not compile, as <strong style="color:green">numeric promotion is not supported</strong>. The last 
              line does  compile, as <strong style="color:green">`String` is compatible with `Object`</strong>.
                - **Matching Records**
                  The last two rules for record matching warrant a bit more discussion. Pattern matching for 
                  records include matching both the type of the record the type of each field. Given the five 
                  pattern matching statements, what does the following code print?
                  - ```java
                    1: record Fish(Object type) {}
                    2: public class Veterinarian {
                    3:   public static void main(String[] args) {
                    4:     Fish f1 = new Fish("Nemo");
                    5:     Fish f2 = new Fish(Integer.valueOf(1));
                    6:
                    7:     if(f1 instanceof Fish(Object t)) {
                    8:       System.out.print("Match1-");
                    9:     }
                    10:    if(f1 instanceof Fish(String t)) {
                    11:      System.out.print("Match2-");
                    12:    }
                    13:    if(f1 instanceof Fish(Integer t)) {
                    14:      System.out.print("Match3-");
                    15:    }
                    16:    if(f2 instanceof Fish(String t)) {
                    17:      System.out.print("Match4-");
                    18:    }
                    19:    if(f2 instanceof Fish(Integer x)) {
                    20:      System.out.print("Match5");
                    21:    } } }
                    ```
                    - The first and second pattern matching statements match because `"Nemo"` can be implicitly cast to `Object` 
                    and `String`, respectively. The third statement compiles but does not match, as `"Nemo"` cannot be cast to 
                    `Integer`. Likewise, the fourth statement compiles but does not match, as the numeric value cannot be cast 
                    to `String`. Finally, the last statement matches as the type of both is `Integer`. The code compiles and prints 
                    the following at runtime:
                      ```java
                      Match1-Match2-Match5
                      ```
                    - What happens if we change the declaration of `Fish` to the following?
                      ```java
                      1: record Fish(Integer type) {}
                      ```
                      First off, our `f1` variable declared on line 4 would no longer compile! Assuming we fix the variable declaration, 
                      though, lines 10 and 16 would not compile. The compiler is smart enough to know that no instance of `Fish` 
                      is capable of matching an `Integer` to a `String`.
- **Nesting Record Patterns**
If a record includes other record values as members, then you can optionally pattern match 
the fields within the record. Ready to see how this works? Let’s start with two records.
  ```java
  record Bear(String name, List<String> favoriteThings) {}
  record Couple(Bear a, Bear b) {}
  ```
  - Now, let’s say we define a `Couple` instance within a method.
    ```java
    var c = new Couple(new Bear("Yogi", List.of("PicnicBaskets")), new Bear("Fozzie", List.of("BadJokes")));
    ```
    Which of the following pattern matching statements compile?
    ```java
    if(c instanceof Couple(Bear a, Bear b)) {
      System.out.print(a.name() + " " + b.name());
    }
    if(c instanceof Couple(Bear(String firstName, List<String> f), Bear b)) {
      System.out.print(firstName + " " + b.name());
    }
    if(c instanceof Couple(Bear(String name, List<String> f1), Bear(String name, List<String> f2))) {
      System.out.print(name + " " + name);
    }
    ```
    - The first pattern matching statement compiles and uses `Couple` without expanding the nested `Bear` records. The
      second example expands the first `Bear` record, making `firstName`, `f` and `b` local variables within the pattern matching 
      statement. The third pattern matching statement does not compile. Although you can expand both records, <strong style="color:green">you have 
      to give them distinct names</strong>. We can fix this, though, by expanding the nested types to have unique names.
      ```java
      if(c instanceof Couple(Bear(String n1, List<String> f1), Bear(String n2, List<String> f2))) {
        System.out.print(n1 + " " + n2);
      }
      ```
      - **Matching Records with _`var`_ and Generics**
          You can also use `var` in a pattern matching record. Let’s apply this to our previous examples.
          ```java
          var c = new Couple(new Bear("Yogi", List.of("PicnicBaskets")),
          new Bear("Fozzie", List.of("BadJokes")));
          if (c instanceof Couple(var a, var b)) {
            System.out.print(a.name() + " " + b.name());
          }
          if (c instanceof Couple(Bear(var firstName, List<String> f), var b)) {
            System.out.print(firstName + " " + b.name());
          }
          ```
          - As you can see, you can replace any element reference type with `var`. **When `var` is used for one 
          of the elements of the record, the compiler assumes the type to be the exact match for the 
          type in the record.**
            - Pattern matching generics within records follow the similar rules for overloading generic 
            methods. Let’s try a few examples, though, to see the kinds of things that exam might 
            throw at you. Each of the following compiles without issue:
              ```java
              if(c instanceof Couple(Bear(var n, Object f), var b)) {}
              if(c instanceof Couple(Bear(var n, List f), var b)) {}
              if(c instanceof Couple(Bear(var n, List<?> f), var b)) {}
              if(c instanceof Couple(Bear(var n, List<? extends Object> f), var b)) {}
              if(c instanceof Couple(Bear(var n, ArrayList<String> f), var b)) {}
              ```
              - There are limits, though. For example, the following two examples do not compile:
                ```java
                if(c instanceof Couple(Bear(var n, List<> f), var b)) {}
                if(c instanceof Couple(Bear(var n, List<Object> f), var b)) {}
                ```
                - The first example does not compile because the diamond operator (`<>`) cannot be used for 
                pattern matching (nor overloading generics). The second example does not compile because 
                `List<Object>` is not compatible with `List<String>`. This would also not compile if these types 
                were applied to method parameters of inherited methods, due to type erasure.
- In these examples, remember that `f` is the pattern type, not the original `List<String>`. Given 
this, can  you deduce why this code does not compile?
  ```java
  if(c instanceof Couple(Bear(var n, List f), var b)
        && f.getFirst().toLowerCase().contains("p")) { // DOES NOT COMPILE
    System.out.print("Yummy");
  }
  ```
  - The reference type of `f` is `List`, not `List<String>`, therefore `f.getFirst()` returns an `Object` 
  reference, not a `String` reference. Since `toLowerCase()` is not defined on `Object`, the
  code does not compile. To compile you would either have to explicitly cast it to a `String` 
  or use a different pattern matching type.
    - **Applying Pattern Matching Records to Switch**
      It might not be a surprise that you can use switch with pattern matching and records. The 
      rules are the same as you’ve already learned, we’re just combing the `switch` pattern 
      matching rules you learned about in Chapter 3 with what we covered in this chapter.
      Let’s say we have a `Snake` record as follows:
      ```java
      record Snake(Object data) {}
      ```
      - Next, let’s construct a method that operates on an instance of `Snake`.
        ```java
        long showData(Snake snake) {
          return switch(snake) {
            case Snake(Long hiss)-> hiss + 1;
            case Snake(Integer nagina) -> nagina + 10;
            case Snake(Number crowley) -> crowley.intValue() + 100;
            case Snake(Object kaa) -> -1; //required
          }; //required semicolon
        }
        ```
        - As you might recall, a `default` clause is not required if all types are covered in the pattern  matching 
        expression. Given this code, see if you can follow the output generated by each of these examples:
          ```java
          System.out.println(showData(new Snake(1)));   // 11
          System.out.println(showData(new Snake(2L)));  // 3
          System.out.println(showData(new Snake(3.0))); // 103
          ```
          - Remember, the type matters for any associated `when` clauses. For example, the following does 
          not compile since `kaa` is of type `Object`, which does not have a` doubleValue()` method:
            ```java
            long showData(Snake snake) {
              return switch(snake) {
                case Snake(Object kaa) when kaa.doubleValue() > 0 -> -1;
                default -> 1_000; //(In normal pattern matching is not allowed)
              };
            }
            ```
- **Customizing Records**
Since records are data-oriented, we’ve focused on the features of records you are likely to use. 
Records actually support many of the same features as a class. Here are some of the members 
that records can include and that you should be familiar with for the exam:
  - \* Overloaded and compact constructors
    \* Instance methods including overriding any provided methods (accessors, `equals()`, `hashCode()`, `toString()`)
    \* Nested classes, interfaces, annotations, enums, and records
    - As an illustrative example, the following overrides two instance methods 
    using the optional `@Override` annotation:
      ```java
      public record Crane(int numberEggs, String name) {
        @Override public int numberEggs() { return 10; }
        @Override public String toString() { return name; }
      }
      ```
      - While you can add methods, `static` fields, and other data types, <strong style="color:green">you cannot
      add instance fields outside the record declaration.</strong>
        ```java
        public record Crane(int numberEggs, String name) {
          private static int TYPE = 10;
          public int size; // DOES NOT COMPILE
          private final boolean friendly = true; // DOES NOT COMPILE
        }
        ```
        - <strong style="color:green">Records also do not support instance initializers</strong>. All initialization for the fields of a
        record must happen in a constructor. They do support static initializers, though.
          ```java
          public record Crane(int numberEggs, String name) {
            static { System.out.print("Hello Bird!"); }
            { System.out.print("Goodbye Bird!"); } // DOES NOT COMPILE
            { this.name = "Big"; } // DOES NOT COMPILE
          }
          ```
          In this example, the first initializer compiles because it is `static`, while the second and 
          third do not because they are instance initializers.