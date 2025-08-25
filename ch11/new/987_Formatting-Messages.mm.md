---

markmap:
  initialExpandLevel: 2
---
# **Formatting Messages**
- The convention is to use a number inside braces such as `{0}`, `{1}`, etc. The 
number indicates the order in which the parameters will be passed. Although 
resource bundles don’t support this directly, the `MessageFormat` class does.
- For example, suppose that we had this property defined:
  ```
  helloByName=Hello, {0} and {1}
  ```

  We can read in the value normally. After that, we can run it through the
  `MessageFormat` class to substitute the parameters. The second parameter 
  to `format()` is a vararg, allowing you to specify any number of input values.
  Suppose we have a resource bundle `rb`:

  ```js
  String format = rb.getString("helloByName");
  System.out.print(MessageFormat.format(format, "Tammy", "Henry"));
  ```

  This will print the following:
  ```
  Hello, Tammy and Henry
  ```
# **Using the `Properties` Class**
- `Properties` class functions like the `HashMap`.  It 
uses `String` values for the keys and values
- ```
  import java.util.Properties;
  public class ZooOptions {
    public static void main(String[] args) {
      var props = new Properties();
      props.setProperty("name", "Our zoo");
      props.setProperty("open", "10am");
    }
  }
  ```
  - The `Properties` class is commonly used in handling values that may not exist.

    ```js
    System.out.println(props.getProperty("camel"));        //null
    System.out.println(props.getProperty("camel", "Bob")); //Bob
    ```
  - If a key were passed that actually existed, both statements would print it. This is 
  commonly referred to as providing a default, or a backup value, for a missing key.
- The `Properties` class also includes a `get()` method, but only `getProperty()` allows 
for a default value. For example, the following call is invalid since `get()` takes only 
a single parameter:

  ```js
  props.get("open");  //10am

  props.get("open", "The zoo will be open soon");// DOES NOT COMPILE
  ```