---

markmap:
  initialExpandLevel: 1
---
# Applying Access Modifiers
- `private`: Only accessible within the same class.
Package access: `private` plus other members of the
same package. Sometimes referred to as package-
private or default access.
`protected`: Package access plus access within subclasses.
`public`: `protected` plus classes in the other packages.
- **Private Access**
Only code in the same class can call `private` methods or access `private` fields.
First, take a look at [Figure 5.2](https://1drv.ms/i/c/c83cfca51d5c2032/Ec1WWEfWDVlGq3sI1c6zIrEBZGOOLNOkbCEX1EK8IeM1cA?e=ks01es). It shows the classes you’ll use to explore private and package
access. The big boxes are the names of the packages. The smaller boxes inside them are the
classes in each package.
  - This is perfectly legal code because everything is one class:
    ```java
    1: package pond.duck;
    2: public class FatherDuck {
    3:   private String noise = "quack";
    4:   private void quack() {
    5:     System.out.print(noise); // private access is ok
    6:   }
    7: }
    ```
    - `FatherDuck` declares a `private` method `quack()` and uses `private`
    instance variable `noise` on line 5.
    Now we add another class:
      ```java
      1: package pond.duck;
      2: public class BadDuckling {
      3:   public void makeNoise() {
      4:     var duck = new FatherDuck();
      5:     duck.quack();                 // DOES NOT COMPILE
      6:     System.out.print(duck.noise); // DOES NOT COMPILE
      7:   }
      8: }
      ```
      - `BadDuckling` is trying to access an instance variable and a method it has no business
      touching. On line 5, it tries to access a `private` method in another class. On line 6, it
      tries to access a `private` instance variable in another class. Both generate compiler
      errors. Bad duckling!
      Luckily, **you know that accessing `private` members of other classes is not allowed**, 
      and you need to use a different type of access.
        - **NOTE**
        In the previous example, `FatherDuck` and `BadDuckling` are in separate
        files, but what if they were declared in the same file? Even then, the code
        would still not compile as Java prevents access outside the class.
- **Package Access**
`MotherDuck` allows classes in the same package to access her members. 
When there is no access modifier, Java assumes package access.
  ```java
  package pond.duck;
  public class MotherDuck {
    String noise = "quack";
    void quack() {
      System.out.print(noise); // package access is ok
    }
  }
  ```
    - `MotherDuck` can refer to `noise` and call `quack()`. After all, members in the same class are
    certainly in the same package. The big difference is that `MotherDuck` lets other classes
    in the same package access members, whereas `FatherDuck` doesn’t (due to being private).
    `GoodDuckling` has a much better experience than `BadDuckling`:
      ```java
      package pond.duck;
      public class GoodDuckling {
        public void makeNoise() {
          var duck = new MotherDuck();
          duck.quack();                 // package access is ok
          System.out.print(duck.noise); // package access is ok
        }
      }
      ```
      - Notice that all the classes covered so far are in the same package,
      `pond.duck`. This allows package access to work.
      - The cygnet sees the ducklings learning to quack and decides to learn 
      from `MotherDuck` as well.
        ```java
        package pond.swan;            // other package
        import pond.duck.MotherDuck;  // import another package
        public class BadCygnet {
          public void makeNoise() {
            var duck = new MotherDuck();
            duck.quack();                 // DOES NOT COMPILE
            System.out.print(duck.noise); // DOES NOT COMPILE
          }
        }
        ```
        - `MotherDuck` only allows lessons to other ducks by restricting access to the
        `pond.duck` package. Poor little `BadCygnet` is in the `pond.swan` package, 
        and the code doesn’t compile. **Remember that when there is no access 
        modifier on a member, only classes in the same package can access 
        the member.**
- **Protected Access**
Protected access allows everything that package access does, and more. The `protected`
access modifier adds the ability to access members of a parent class. In the following
example, the "child" `ClownFish` class is a subclass of the "parent" `Fish` class, using the
`extends` keyword to connect them:
  ```java
  public class Fish {}

  public class ClownFish extends Fish {}
  ```
  - By extending a class, the subclass gains access to all `protected` and `public` members
  of the parent class, as if they were declared in the subclass. If the two classes are in the
  same package, then the subclass also gains access to all package members.
  [Figure 5.3](https://1drv.ms/i/c/c83cfca51d5c2032/EbV_yHBHQ0tEt7FWND7G15cBeK5mfRbTY1TbhzjEGFuEAA?e=SorkuI) shows the many classes we create in this section.
    - First, create a `Bird` class and give `protected` access to its members:
      ```java
      package pond.shore;
      public class Bird {
        protected String text = "floating";
        protected void floatInWater() {
          System.out.print(text); // protected access is ok
        }
      }
      ```
      - Next, we create a subclass:
        ```java
        package pond.goose;                 // Different package than Bird
        import pond.shore.Bird;
        public class Gosling extends Bird { // Gosling is a subclass of Bird
          public void swim() {
            floatInWater();         // protected access is ok
            System.out.print(text); // protected access is ok
          }
          public static void main(String[] args) {
            new Gosling().swim();
          }
        }
        ```
        - `Gosling` extends the `Bird` class. Extending means creating a subclass that has
         access to any `protected` or `public` members of the parent class. Running this
        program prints `floating` twice: once from calling `floatInWater()`, and once 
        from the print statement in `swim()`. Since `Gosling` is a subclass of `Bird`, it can 
        access these members even though it is in a different package.
          - Remember that `protected` also gives us access to everything that package access does. 
          This means a class in the same package as `Bird` can access its protected members.
              ```java
              package pond.shore;              // Same package as Bird
              public class BirdWatcher {
                public void watchBird() {
                  Bird bird = new Bird();
                  bird.floatInWater();         // protected access is ok
                  System.out.print(bird.text); // protected access is ok
                }
              }
            ```
             - Since `Bird` and `BirdWatcher` are in the same package, `BirdWatcher` can access
              package members of the `bird` variable. The definition of `protected` allows access to
              ­subclasses and classes in the same package. This example uses the same package.
                - Now let’s try the same thing from a different package:
                  ```java
                  package pond.inland; // Different package than Bird
                  import pond.shore.Bird;
                  public class BirdWatcherFromAfar { // Not a subclass of Bird
                    public void watchBird() {
                      Bird bird = new Bird();
                      bird.floatInWater(); // DOES NOT COMPILE
                      System.out.print(bird.text); // DOES NOT COMPILE
                    }
                  }
                  ```
                  - `BirdWatcherFromAfar` is not in the same package as `Bird`, and it doesn’t inherit from
                  `Bird`. This means it is not allowed to access `protected` members of `Bird`.
                  Got that? Subclasses and classes in the same package are the only ones allowed 
                  to access `protected` members.
- There is one gotcha for `protected` access. Consider this class:
  ```java
  1:  package pond.swan; // Different package than Bird
  2:  import pond.shore.Bird;
  3:  public class Swan extends Bird { // Swan is a subclass of Bird
  4:    public void swim() {
  5:      floatInWater();         // protected access is ok
  6:      System.out.print(text); // protected access is ok
  7:    }
  8:    public void helpOtherSwanSwim() {
  9:      Swan other = new Swan();
  10:     other.floatInWater();         // subclass access to superclass
  11:     System.out.print(other.text); // subclass access to superclass
  12:   }
  13:   public void helpOtherBirdSwim() {
  14:     Bird other = new Bird();
  15:     other.floatInWater();           // DOES NOT COMPILE
  16:     System.out.print(other.text);   // DOES NOT COMPILE
  17:   }
  18: }
  ```
  - `Swan` is not in the same package as `Bird` but does extend it—­which implies it has access
to the `protected` members of `Bird` since it is a subclass. And it does. Lines 5 and 6 refer
to `protected` members via inheriting them.
Lines 10 and 11 also successfully use `protected` members of `Bird`. This is allowed because
these lines refer to a `Swan` object. `Swan` inherits from `Bird`, so this is okay. It is sort of a 
two-phase check. The `Swan` class is allowed to use `protected` members of `Bird`, and we are
referring to a `Swan` object. Granted, it is a `Swan` object created on line 9 rather than an
inherited one, but it is still a `Swan` object.
  - Lines 15 and 16 do not compile. Wait a minute. They are almost exactly the same as lines
  10 and 11! There’s one key difference. This time a `Bird` reference is used rather than inher-
  itance. It is created on line 14. `Bird` is in a different package, and this code isn’t inheriting
  from `Bird`, so it doesn’t get to use protected members. Say what, now? We just got through
  saying repeatedly that `Swan` inherits from `Bird`. And it does. However, the variable reference
  isn’t a `Swan`. The code just happens to be in the `Swan` class.

    - Looking at it a different way, the `protected` rules apply under two scenarios:
    1.- A member is used without referring to a variable. This is the case on lines 5 and 6. In
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;this case, we are taking advantage of inheritance, and `protected` access is allowed.
    2.- A member is used through a variable. This is the case on lines 10, 11, 15, and 16. In this
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;case, the rules for the reference type of the variable are what matter. If it is a subclass,
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;`protected` access is allowed. This works for references to the same class or a subclass.
      - We’re going to try this again to make sure you understand what is going on. 
      Can you figure out why these examples don’t compile?
        ```java
        package pond.goose;
        import pond.shore.Bird;
        public class Goose extends Bird {
          public void helpGooseSwim() {
            Goose other = new Goose();
            other.floatInWater();
            System.out.print(other.text);
          }
          public void helpOtherGooseSwim() {
            Bird other = new Goose();
            other.floatInWater();         // DOES NOT COMPILE
            System.out.print(other.text); // DOES NOT COMPILE
          }
        }
        ```
        - The first method is fine. In fact, it is equivalent to the `Swan` example. `Goose` extends `Bird`. 
        Since we are in the `Goose` subclass and referring to a `Goose` reference, it can access
        `protected` members. The second method is a problem. Although the object happens to be a
        `Goose`, it is stored in a `Bird` reference. We are not allowed to refer to members of the `Bird`
        class since we are not in the same package and the reference type of `other` is not a sub-
        class of `Goose`.
          - What about this one?
            ```java
            package pond.duck;
            import pond.goose.Goose;
            public class GooseWatcher {
              public void watch() {
                Goose goose = new Goose();
                goose.floatInWater(); // DOES NOT COMPILE
                }
            }
            ```
            - This code doesn’t compile because we are not in the `goose` object. The `floatInWater()`
            method is declared in `Bird`. `GooseWatcher` is not in the same package as `Bird`, nor does
            it extend `Bird`. `Goose` extends `Bird`. That only lets `Goose` refer to `floatInWater()`, not
            callers of `Goose`.
- **Public Access**
`public` means anyone can access the member from anywhere.
** The Java module system redefines “anywhere,” and it
becomes possible to restrict access to `public` code
outside a module. 
  - Let’s create a class that has `public` members:
    ```java
    package pond.duck;
    public class DuckTeacher {
      public String name = "helpful";
      public void swim() {
        System.out.print(name); // public access is ok
      }
    }
    ```
    - `DuckTeacher` allows access to any class that wants it. Now we
    can try it:
      ```java
      package pond.goose;
      import pond.duck.DuckTeacher;
      public class LostDuckling {
        public void swim() {
          var teacher = new DuckTeacher();
          teacher.swim(); //allowed
          System.out.print("Thanks " + teacher.name); //allowed
        }
      }
      ```
      - `LostDuckling` is able to refer to `swim()` and `name` on `DuckTeacher`
      because they are `public`.
        - **Reviewing Access Modifiers**
        Make sure you know why everything in **Table 5.4** is `true`.<br/>
          - **TABLE 5.4** A method in ______ can access a ______ member.
          <br/>
              ||`private`|package|`protected`|`public`|
              |-|-|-|-|-|
              |the same class|Yes|Yes|Yes|Yes|
              |another class in the same<br/> package|No|Yes|Yes|Yes|
              |a subclass in a different <br/>package |No|No|Yes|Yes|
              |an unrelated class in a<br/>different package|No|No|No|Yes|

