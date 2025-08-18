---

markmap:
  initialExpandLevel: 1
---
# **Creating a New Package**
- Up to now, all the code we’ve written in this chapter has been in
the _default package_. This is a special unnamed package that you
should use only for throwaway code.
  - The directory structure on your computer is related to the package name.
- Suppose we have these two classes in the `/tmp` directory:
  - ```java
    package packagea;
    public class ClassA{}
    //--------------------
    package packageb;
    import packagea.ClassA;
    public class ClassB{
      public static void main(String[] args) {
        ClassA a;
        System.out.println("Got it");
      }
    }
    ```
    - Running from `/tmp` works because both `packagea` 
    and `packageb` are underneath it.
- **Compiling and Running Code with Packages**
  - The first step is to create the two files from the previous section.
  For Windows, see [Table 1.1](https://1drv.ms/i/c/c83cfca51d5c2032/Eb7BEhhvg8ROnMICYP-ZrHMBbgHBFRq4y7JF1rH_1uO_RQ?e=rkJrUm).
    - 1. Create first class.
          - `/tmp/packagea/ClassA.java`
    - 2. Create second class.
          - `/tmp/packageb/ClassB.java`
    - 3. Go to directory.
          - `cd /tmp`
  - To compile:
`javac packagea/ClassA.java packageb/ClassB.java`
    - Two new files will be created:
      - `packagea/ClassA.class and packageb/ClassB.class`
    - You can use an asterisk (*) to specify that you’d like to include all Java files
    in a directory. This is convenient when you have a lot of files in a package. 
    `javac packagea/*.java packageb/*.java`
    You cannot use a wildcard to include subdirectories.
  - You can run it:
`java packageb.ClassB`
    - You’ll see `Got it` printed. You might have noticed that we typed
    `ClassB` rather than `ClassB.class`. As discussed earlier,  **you don’t 
    pass the extension when running a program**.
      - The following shows where the `.class` files 
      were created in the directory structure.`
        ```
        tmp
        ├── packagea
        │   ├── ClassA.class
        │   └── ClassA.java
        └── packageb
            ├── ClassB.class
            └── ClassB.java`
        ```
- **Compiling to Another Directory**
  - By default, the `javac` command places the compiled classes in 
  the same directory as the source code. It also provides an option 
  to place the class files into a different directory. The `-d` option
  specifies this target directory.
    - Java options are case sensitive. This means you cannot pass `-D` instead of `-d`.
      - Delete the `ClassA.class` and `ClassB.class` files.
  - To compile:
`javac -d classes packagea/ClassA.java packageb/ClassB.java`
    - The command creates the `classes` directory if it does not exist.
  - The following shows this new structure`
    ```
    tmp
    ├── classes
    │   ├── packagea
    │   │   └── ClassA.class
    │   └── packageb
    │       └── ClassB.class
    ├── packagea
    │   └── ClassA.java
    └── packageb
        └── ClassB.java`
    ```
    - To run the program, you specify the classpath so Java knows where
    to find the classes. There are three options you can use.
      ```java
      java -cp classes packageb.ClassB
      java -classpath classes packageb.ClassB
      java --class-path classes packageb.ClassB
      ```
    - Notice that the last one requires two dashes (`--`), while the first two
    require one dash (`-`)
- **Important `javac` options**
  - ```
    -cp <classpath>
    -classpath <classpath>
    --class-path <classpath>
    ```
    - Location of classes needed to compile the program
  - `-d <dir>`
    - Directory in which to place generated class files
- **Important `java` options**
  - ```
    -cp <classpath>
    -classpath <classpath>
    --class-path <classpath>
    ```
    - Location of classes needed to run the program