---

markmap:
  initialExpandLevel: 1
---
# **Major Components of Java**
- The Java Development Kit (JDK) contains the minimum
 software you need to do Java development.
- Key commands
  - `javac`: Converts `.java` source files into `.class` bytecode
  - `java`: Executes the program
  - `jar`: Packages files together
  - `javadoc`: Generates documentation
- The `javac` program generates instructions in a special format called _bytecode_ 
that the `java` command can run. Then `java` launches the Java Virtual Machine 
(JVM) before running the code. The JVM knows how to run bytecode.
- **Check Your Version of Java**
  - `javac -version`
    `java -version`
    Both of these commands should include version number 21.