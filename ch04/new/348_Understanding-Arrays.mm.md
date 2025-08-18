---

markmap:
  initialExpandLevel: 1
---
# **Understanding Arrays**
- An array is an area of memory on the heap with space for a
designated number of elements
    ```java
    char[] letters;
    ```
    - Keep in mind that `letters` is a reference variable and not a
    primitive. The `char` type is a primitive. But `char` is what goes
    into the array and not the type of the array itself. The array
    itself is of type `char[]`. You can mentally read the brackets
    (`[]`) as “array.”
        - An array is an ordered list and can contain
duplicates.
- **Creating an Array of Primitives**
The [Figure 4.3](https://1drv.ms/i/c/c83cfca51d5c2032/Ed4CH0WjwN9NqjsUK3OJAW4BfKitQC50FkChGIpz7a6yVQ?e=bHI9nb)  shows the most common way to create an array.
It specifies the type of the array (`int`) and the size (`3`). The
brackets tell you this is an array.
    - When you use this form to instantiate an array, all elements
    are set to the default value for that type. The default value of
     an `int` is `0`. Since `numbers` is a reference variable, it points
    to the array object, as shown in [Figure 4.4](https://1drv.ms/i/c/c83cfca51d5c2032/EdpwXex4-9pPsLWkmcZD-9kBiHTtq0sC7grumbVYHuKYWA?e=L1xG1z) . As you can see,
     the default value for all the elements is `0`. Also, the indexes 
     start with `0` and count up, just as they did for a `String`.
        - Another way to create an array is to specify all the elements 
        it should start out with.
            ```java
            int[] moreNumbers = new int[] {42, 55, 99};
            ```
            In this example, we also create an int array of size `3`. This
            time, we specify the initial values of those three elements
            instead of using the defaults. [Figure 4.5](https://1drv.ms/i/c/c83cfca51d5c2032/Ea0HOvCqIjNKj4UV0WGISTcBAi3Z-4oCGpBvWbuApBWs8g?e=dBUAsZ) shows what this
            array looks like.
            - The array type is specified on the left side of the equal sign.
             Since the initial values ​​are also specified, Java already knows
              the type and size.             As a shortcut, Java lets you write this:
                ```java
                int[] moreNumbers = {42, 55, 99};
                ```
                This approach is called an anonymous array. It is anonymous 
                because you don’t specify the type and size.
                - Finally, you can type the `[]` before or after the name, and
                adding a space is optional. All five of these statements do
                 the exact same thing:
                    ```java
                    int[] numAnimals;
                    int [] numAnimals2;
                    int []numAnimals3;
                    int numAnimals4[];
                    int numAnimals5 [];
                    ```
                    - **Multiple “Arrays” in Declarations**
                    What types of reference variables do you think the following
                     code creates?
                        ```java
                        int[] ids, types;
                        ```
                        The correct answer is two variables of type `int[]`. This
                        seems logical enough. After all, `int a, b;` created two `int`
                        variables. What about this example?
                        ```java
                        int ids[], types;
                        ```
                        - All we did was move the brackets, but it changed the behavior. 
                        This time we get one variable of type `int[]` and one variable of
                         type `int`. Java sees this line of code and thinks something like
                          this: “They want two variables of type `int`. The first one is called
                           `ids[]`. This one is an `int[]` called `ids`. The second one is just 
                           called `types`. No brackets,                          so it is a regular integer.”
- **Creating an Array with Reference Variables**
You can choose any Java type to be the type of the array. Let’s take a look at a
 built-in type with `String`:
    ```java
    String[] bugs = { "cricket", "beetle", "ladybug" };
    String[] alias = bugs;
    String[] anotherArray = { "cricket", "beetle", "ladybug" };
    System.out.println(bugs.equals(alias)); // true
    System.out.println(bugs.equals(anotherArray)); // false
    System.out.println(bugs.toString()); // [Ljava.lang.String;@160bc7c0
    ```
    - We can call `equals()` because an array is an object. The first
    test with `alias` returns `true` because of reference equality.
    Why does the second equality test return `false`? The `equals()`
    method on arrays does not look at the elements of the array.
    The second print statement is even more interesting. What on 
    Earth is `[Ljava.lang.String;@160bc7c0`? You don’t have to know
     this for the exam, but `[L` means it is an array,
    `java.lang.String` is the reference type, and `160bc7c0` is the hash 
    code.
        - Java provides a method that prints an array nicely:
        `Arrays.toString(bugs)` would print `[cricket, beetle,
        ladybug]`.
        - We can see our `bugs` array represented in memory in [Figure
        4.6](https://1drv.ms/i/c/c83cfca51d5c2032/EQZs7NK-ebxAibs0AjMgaMEBc00CdhfMt96_smdpkUK9aQ?e=pvq6MF). Make sure you understand this figure. The array does not
         allocate space for the `String` objects. Instead, it allocates space
          for a reference to where the objects are really stored.
            - As a quick review, what do you think this array points to?
                ```java
                public class Names {
                  String names[];
                }
                ```
                The answer is `null`. The code never instantiated the array,
                so it is just a reference variable to `null`. Let’s try that again:
                what do you think this array                 points to?
                ```java
                public class Names {
                  String names[] = new String[2];
                }
                ```
                - It is an array because it has brackets. It is an array of type
                `String` since that is the type mentioned in the declaration. It
                has two elements because the length is `2`. Each of those
                two slots currently is `null` but has the potential to point to a
                `String` object.
                    -  You can use casting with arrays too:
                        ```java
                        3: String[] strings = { "stringValue" };
                        4: Object[] objects = strings;
                        5: String[] againStrings = (String[]) objects;
                        6: againStrings[0] = new StringBuilder(); // DOES NOT COMPILE
                        7: objects[0] = new StringBuilder(); // Careful!
                        ```
                        Line 3 creates an array of type `String`. Line 4 doesn’t require a cast because
                        `Object` is a broader type than `String`. On line 5, a cast is needed because 
                        we are moving to a more specific type. Line 6 doesn’t compile because a
                        `String[]` allows only `String` objects, and `StringBuilder` is not a `String`.

                        - Line 7 is where this gets interesting. From the point of view of the compiler,
                        this is just fine. A `StringBuilder` object can clearly go in an `Object[]`. The 
                        problem is that we don’t actually                          have an `Object[]`. We have a `String[]` 
                        referred to from an `Object[]` variable. At runtime, the code throws an
                        `ArrayStoreException`. You don’t need to memorize the name of this exception, 
                        but you do need to know that this line will compile and throw an exception.
- **Using an Array**
Now that you know how to create an array, let’s try accessing one:
    ```java
    4: String[] mammals = {"monkey", "chimp", "donkey"};
    5: System.out.println(mammals.length);  // 3
    6: System.out.println(mammals[0]);      // monkey
    7: System.out.println(mammals[1]);      // chimp
    8: System.out.println(mammals[2]);      // donkey
    ```
    - Line 4 declares and initializes the array. Line 5 tells us how many 
    elements the array can hold. The rest of the code prints the array. 
    Notice that elements are indexed starting with `0`. This should be 
    familiar from `String` and `StringBuilder`, which also start counting 
    with `0`. Those classes also counted `length` as the number of
     elements. Note that there are no parentheses after length since it
      is not a method (**in `String` is method and in array is property**).
        - Watch out for compiler errors like the following on the exam!
            ```java
            4: String[] mammals = {"monkey", "chimp", "donkey"};
            5: System.out.println(mammals.length()); // DOES NOT COMPILE
            ```
            To make sure you understand how `length` works, what do you think this prints?
            ```java
            4: var birds = new String[6];
            5: System.out.println(birds.length);
            ```
            The answer is `6`. Even though all six elements of the array are `null`, there are 
            still six of them. The `length` attribute does not consider what is in the array; it 
            considers only how many slots have been allocated.
            - It is very common to use a loop when reading from or
            writing to an array. This loop sets each element of 
            `numbers` to five higher than the current index:
                ```java
                5: var numbers = new int[10];
                6: for (int i = 0; i < numbers.length; i++)
                7:   numbers[i] = i + 5;
                8: for(int n : numbers)
                9:   System.out.println(n);
                ```
                - Line 5 simply instantiates an array with `10` slots. Line 6 is a
                `for` loop that uses an extremely common pattern. It starts at
                index `0`, which is where an array begins as well. It keeps
                going, one at a time, until it hits the end of the array. Line 7
                sets the current element of `numbers` to the index of the
                element plus 5. Lines 8 and 9 print the numbers in the array,
                 using the for-each loop.
                    - For our array of size 10, Can you tell why each of these throws an
                    `ArrayIndexOutOfBoundsException`?
                        ```java
                        3: var numbers = new int[10];
                        4: numbers[10] = 3;
                        5:
                        6: numbers[numbers.length] = 5;
                        7:
                        8: for (int i = 0; i <= numbers.length; i++)
                        9:   numbers[i] = i + 5;
                        ```
                        - The first one is trying to see whether you know that indexes
                        start with `0`. Since we have 10 elements in our array, this
                        means only `numbers[0]` through `numbers[9]` are valid. The
                        second example assumes you are clever enough to know
                        that `10` is invalid and disguises it by using the length field.
                        However, the `length` is always one more than the maximum
                        valid index. Finally, the for loop incorrectly uses `<=` instead
                        of `<`, which is also a way of referring to that tenth index.
- **Sorting**
Java provides a sort method—or rather, a bunch of sort methods. 
You can pass almost any array to `Arrays.sort()`.
`Arrays` requires an import. To use it, you must have either of the
following two statements in your class:
    ```java
    import java.util.*; // import whole package 
                        // including  Arrays
    import java.util.Arrays; // import just Arrays
    ```
    There is one exception, although it doesn’t come up often on the
     exam. You can write `java.util.Arrays` every time it is used in the 
     class instead of specifying it as an import.
    
    - Remember that if you are shown a code snippet, you can
    assume the necessary imports are there. This simple
    example sorts three numbers:
        ```java
        int[] numbers = { 6, 9, 1 };
        Arrays.sort(numbers);
        for (int i = 0; i < numbers.length; i++)
           System.out.print(numbers[i] + " ");
        ```
        - The result is `1 6 9`, as you should expect it to be. Notice that
        we looped through the output to print the values in the array. 
        Just printing the array variable directly would give the annoying
        hash of `[I@2bd9c3e7`. Alternatively, we could have printed
        `Arrays.toString(numbers)` instead of using the loop. That 
        would have output `[1, 6, 9]`.
            - Try this again with `String` types:
                ```java
                String[] strings = { "10", "9", "100" };
                Arrays.sort(strings);
                for (String s : strings)
                System.out.print(s + " ");
                ```
                This time the result might not be what you expect. This code
                 outputs `10 100 9`. The problem is that `String` sorts in alphabetic
                  order, and `1` sorts before `9`. (Numbers sort before letters, and
                   uppercase sorts before lowercase.)
                - You can use **7Up**, the soda, to help remember the order.
                Numbers (7) sort first, followed by uppercase (U), and
                then lowercase (p).
- **Searching**
Java also provides a convenient way to search, **but only if
the array is already sorted.** Table 4.3 covers the rules for
binary search.
    - **TABLE 4.3** Binary search rules
        |Scenario|Result|
        |-|-|
        |Target element<br/> found in sorted<br/>array|Index of match|
        |Target element<br>not found in<br/>sorted array|Negative value showing one smaller<br/>than the negative of the index, where a<br/>match needs to be inserted to preserve<br/>sorted order (like `~` operator)|
        |Unsorted array|A surprise; this result is undefined|
        - Let’s try these rules with an example:
        
            ```java
            3: int[] numbers = {2,4,6,8};
            4: System.out.println(Arrays.binarySearch(numbers, 2)); // 0
            5: System.out.println(Arrays.binarySearch(numbers, 4)); // 1
            6: System.out.println(Arrays.binarySearch(numbers, 1)); // -1
            7: System.out.println(Arrays.binarySearch(numbers, 3)); // -2
            8: System.out.println(Arrays.binarySearch(numbers, 9)); // -5
            ```
            Take note of the fact that line 3 is a sorted array. If it wasn’t, we couldn’t 
            apply either of the other rules. Line 4 searches for the index of `2`. The 
            answer is index `0`. Line 5 searches for the index of `4`, which is 1.
            - Line 6 searches for the index of `1`. Although `1` isn’t in the
            list, the search can determine that it should be inserted at
            element 0 to preserve the sorted order. Since 0 already
            means something for array indexes, Java needs to subtract
            1 to give us the answer of `–1`. Line 7 is similar. Although `3`
            isn’t in the list, it would need to be inserted at element 1 to
            preserve the sorted order. We negate and subtract 1 for
            consistency, getting `–1–1`, also known as `–2`. Finally, line 8
            wants to tell us that 9 should be inserted at index `4`. We
            again negate and subtract 1, getting `–4–1`, also known as `–5`.
                - What do you think happens in this example?
                    ```java
                    5: int[] numbers = new int[] {3,2,1};
                    6: System.out.println(Arrays.binarySearch(numbers, 2));
                    7: System.out.println(Arrays.binarySearch(numbers, 3));
                    ```
                    Note that on line 5, the array isn’t sorted. As soon as you see the
                     array isn’t sorted, look for an answer choice about unpredictable 
                     output.
                    On the exam, you need to know what a binary search returns in
                     various scenarios.