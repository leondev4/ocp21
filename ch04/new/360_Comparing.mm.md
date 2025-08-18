---

markmap:
  initialExpandLevel: 1
---
# **Comparing**
- **Using `equals()`**
While `==` compares object references, `Arrays` includes overloaded versions
 of `equals()` that lets you **check if the arrays are the same size and contain
  the same elements, in the same order**. 
    - For example:
      ```java
      var c1 = new int[]{1} == new int[]{1};                // false
      var c2 = Arrays.equals(new int[]{1}, new int[]{1});   // true
      var c3 = Arrays.equals(new int[]{1}, new int[]{2});   // false
      var c4 = Arrays.equals(new int[]{1}, new int[]{1,2}); // false
      ```
    When comparing elements, it uses `==` for primitive values and `equals()` for
    object values.
- **Using `compare()`**
First you need to learn what the return value means. You do need to 
know the following:
*A **negative number** means the first array is smaller than the second.
*A **zero** means the arrays are equal.
*A **positive number** means the first array is larger than the second.
  - Here’s an example:
    ```java
    var r = Arrays.compare(new int[]{1}, new int[]{2});
    ```
    `r` is a negative number. It should be pretty     intuitive that `1` is 
    smaller than `2`, making the first array smaller.
    - Now that you know how to compare a single value, let’s
  look at how to compare arrays of different lengths:
      * If both arrays are the same length and have the same
      values in each spot in the same order, return zero.
      * If all the elements are the same but the second array
      has extra elements at the end, return a negative
      number.
      * If all the elements are the same, but the first array has
      extra elements at the end, return a positive number.
      * If the first element that differs is smaller in the first
      array, return a negative number.
      * If the first element that differs is larger in the first
      array, return a positive number.
    - Finally, what does smaller mean?
      * `null` is smaller than any other value.
      * For numbers, normal numeric order applies.
      * For strings, one is smaller if it is a prefix of another.
      * For strings/characters, numbers are smaller than letters.
      * For strings/characters, uppercase is smaller than lowercase.
    - Table 4.4 shows examples of these rules in action.
      - **TABLE 4.4** `Arrays.compare()` examples  <br>
        |First <br/>array| Second<br/> array| Result| Reason|
        |-|-|-|-|
        |`new int[]`<br/>`{1, 2}`|`new int[]`<br/>`{1}`|Positive<br/>number|The first element is the<br/>same, but the first array<br/>is longer.|
        |`new int[]`<br/>`{1, 2}`|`new int[]`<br/>`{1,2}`|Zero|Exact match.|
        |`new`<br/>`String[]`<br/>`{"a"}`|`new`<br/>`String[]`<br/>`{"aa"}`|Negative<br/> number|The first element is a<br/>substring of the second.|
        |`new`<br/>`String[]`<br/>`{"a"}`|`new`<br/>`String[]`<br/>`{"A"}`|Positive<br/>number|Uppercase is smaller<br/>than lowercase.|
        |`new`<br/>`String[]`<br/>`{"a"}`|`new`<br/>`String[]`<br/>`{null}`|Positive<br/>number|`null` is smaller than a<br/>letter|
    - Finally, this code does not compile because
     the types are different. When comparing two
      arrays, they must be the same array type.
      ```java
      int []n={1};
      String a[]={"a"};
      //DOES NOT COMPILE
      var r = Arrays.compare(n,a); 
      ```

- **Using `mismatch()`**
If the arrays are equal, `mismatch()` returns `-1`. Otherwise,
 it returns the first index where they differ. Can you figure 
out what these print?
  ```java
  int a1[] = {1}; int [] a2 = {1};
  String s1[]= {"a"}; String []s2={"A"};
  int a3[]={1, 2};
  System.out.println(Arrays.mismatch(a1,a2));
  System.out.println(Arrays.mismatch(s1,s2));
  System.out.println(Arrays.mismatch(a3,a1));
  ```
  - In the first example, the arrays are the same, so the result
  is `-1`. In the second example, the entries at element 0 are
  not equal, so the result is `0`. In the third example, the
  entries at element 0 are equal, so we keep looking. The
  element at index 1 is not equal. Or, more specifically, one
  array has an element at index `1`, and the other does not.
  Therefore, the result is `1`.
    - To make sure you understand the `compare()` and `mismatch()`
    methods, study Table 4.5.
    - **TABLE 4.5** Equality vs. comparison vs. mismatch
      |Method|When arrays contain<br/>the same data| When arrays are<br/>different|
      |-|-|-|
      |`Arrays.equals()`|`true`|`false`|
      |`Arrays.compare()`|`0`|Positive or<br/> negative number|
      |`Arrays.mismatch()`|`-1`|Zero or positive<br/>index|
- **Using Methods with Varargs**
When an array is passed to your method, there is  another
 way it can look. Here are three examples with a `main()` 
 method:
  ```java
  public static void main(String[] args)
  public static void main(String args[])
  public static void main(String… args) // varargs
  ```
  - The third example uses a syntax called varargs (variable
arguments).For now, all you need to know is that you can
 use a variable defined using varargs as if it were a normal 
 array. For example, `args.length` and `args[0]` are legal.

- **Creating an Array of Arrays**
You can locate them with the type or variable name in the
declaration, just as before:
  ```java
  int[][] vars1; // 2D array
  int vars2 [][]; // 2D array
  int[] vars3[]; // 2D array
  int[] vars4 [], space [][]; // 2D and 3D arrays
  ```
  - The first two examples declare a two-dimensional (2D) array. 
  The third example also declares a 2D array. There’s no good
   reason to use this style other than to confuse readers with
    your code. The final example declares two arrays on the same
     line. Adding up the brackets, we see that the vars4 is a 2D array 
     and space is a 3D array. Again, there’s no reason to use this
style other than to confuse readers of your code.
    - You can specify the size of your array and the array it contains 
    in the declaration if you like:
      ```java
      String [][] rectangle = new String[3][2];
      ```
      The result of this statement is an array rectangle with three
      elements, each of which refers to an array of two elements.
      You can think of the addressable range as `[0][0]` through
      `[2][1]`, but don’t think of it as a structure of addresses like 
      `[0,0]` or `[2,1]`.
      - Now suppose we set one of these values:
        ```java
        rectangle[0][1] = "set";
        ```
        You can visualize the result as shown in [Figure 4.7](https://1drv.ms/i/c/c83cfca51d5c2032/EVHBM8DuQ2lGtlJrxI3EApYBi_Z7wf6rzMV-r63yu8ePrA?e=eAeavH). This
        array is sparsely populated because it has a lot of `null`
        values. You can see that rectangle still points to an array of
        three elements and that we have three arrays of two
        elements. You can also follow the trail from reference to
        the one value pointing to a `String`. You start at index `0` in the
        top array. Then you go to index `1` in the next array.
        - While that array happens to be rectangular in shape, an
        array doesn’t need to be. Consider this one:
          ```java
          int[][] differentSizes = {{1, 4}, {3}, {9,8,7}};
          ```
          We still start with an array of three elements. However,
          this time the elements in the next level are all different
          sizes. One is of length `2`, the next length `1`, and the last
          length `3`. See [Figure 4.8](https://1drv.ms/i/c/c83cfca51d5c2032/EVblEu31DplFqhi3n5HI8DYBSrYAvIaYAUgnkRADfJKeWw?e=pTc6Y6). This time the array is of
          primitives, so they are shown as if they are in the array
          themselves.

          - Another way to create an asymmetric array is to initialize
          just an array’s first dimension and define the size of each
          array component in a separate statement.
            ```java
            int [][] args = new int[2][];
            args[0] = new int[5];
            args[1] = new int[3];
            ```
            This technique reveals what you really get with Java: arrays
            of arrays that, properly managed, could look like a matrix.
- **Using an Array of Arrays**
The most common operation on an array of arrays is to loop
through it. This example prints out a 2D array:
  ```java
  var twoD = new int[3][2];
  for(int i = 0; i < twoD.length; i++) {
    for(int j = 0; j < twoD[i].length; j++)
      System.out.print(twoD[i][j] + " "); //print element
      System.out.println();         //time for a new row
  }
  ```
  - We have two loops here. The first uses index `i` and goes
through the top-level array for `twoD`. The second uses a
different loop variable, `j`. It is important that these be
different variable names so the loops don’t get mixed up.
The inner loop looks at how many elements are in the
second-level array. The inner loop prints the element and
leaves a space for readability. When the inner loop
completes, the outer loop goes to a new line and repeats
the process for the next element.
    - This entire exercise would be easier to read with the
    enhanced `for` loop.
      ```java
      for(int[] inner : twoD) {
      for(int num : inner)
      System.out.print(num + " ");
      System.out.println();
      }
      ```
      We’ll grant you that it isn’t fewer lines, but each line is less
      complex, and there aren’t any loop variables or terminating
      conditions to mix up.