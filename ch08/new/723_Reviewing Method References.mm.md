# **Method references**
- **TABLE 8.3** Method references
  |Type|Before ::|After ::|Example|
  |-|-|-|-|
  |`static` methods|Class name|Method name|`Math::random`|
  |Instance methods on a<br/>particular object|Instance variable name|Method name|`str::startsWith`|
  |Instance methods on a<br/>parameter|Class name|Method name|`String::isEmpty`|
  |Constructor|Class name|`new`|`String::new`|
  - The second, uses variables declared outside the lambda expression
  - To Change _Instance method on particular object_ to _Instance method
   on a parameter_ **only add 1 parameter** to the functional interface.
  - ```java
    String str = "hello";
    Predicate<String> pred = str::startsWith;
          //s -> str.startsWith(s);
    System.out.println(pred.test("he"));
            
    BiPredicate<String,String> biPre = String::startsWith;
                    //(s,pref)->s.startsWith(pref);
    System.out.println(biPre.test(str, "he"));
    ```
  - Third use variables declared as arguments in the lambda expression