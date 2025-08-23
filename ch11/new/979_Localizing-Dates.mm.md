# **Loading Properties with<br/> Resource Bundles**
- A _resource bundle_ contains the locale-­specific objects to be used by a program. It is like 
a map with keys and values. The resource bundle is commonly stored in a properties file.
A _properties file_ is a text file in a specific format with key/value pairs.
- Resource bundles
  - Will let us easily translate our application to multiple
   locales or even support multiple locales at once
  - Each line representing a different key/value. The key and
   value are separated by an equal sign (`=`) or colon (`:`). 
  - **If we don’t have a country-specific resource 
  bundle, Java will use a language-­specific one.**
# **Creating a Resource Bundle**
- ```
  //Zoo_en.properties
  hello=Hello
  open=The zoo is open
  
  //Zoo_fr.properties
  hello=Bonjour
  open=Le zoo est ouvert
  ```
  - The filenames match the name of our resource bundle,
   `Zoo`. They are then followed by an underscore (`_`),
    target locale, and `.properties` file extension.
- ```js
  10: public static void printWelcomeMessage(Locale locale) {
  11:   var rb = ResourceBundle.getBundle("Zoo", locale);
  12:   System.out.println(rb.getString("hello")
  13:   + ", " + rb.getString("open"));
  14: }
  15: public static void main(String[] args) {
  16:   var us =Locale.of("en", "US");//fall Zoo_en
  17:   var france =Locale.of("fr", "FR");//fall Zoo_fr
  18:   printWelcomeMessage(us); // Hello, The zoo is open
  19:   printWelcomeMessage(france); // Bonjour, Le zoo est ouvert
  20: }
  ```
- Since a resource bundle contains key/value pairs, you can even loop 
through them to list all of the pairs. The `ResourceBundle` class provides 
a `keySet()` method to get a set of all keys.

  ```js
  var us = Locale.of("en", "US");
  ResourceBundle rb = ResourceBundle.getBundle("Zoo", us);
  rb.keySet().stream()
    .map(k -­> k + ": " + rb.getString(k))
    .forEach(System.out::println);
  ```
  - This example goes through all of the keys. It maps each key to a 
  `String` with both the key and the value before printing everything.
    ```
    hello: Hello
    open: The zoo is open
    ```
# **Picking a Resource Bundle (file <br/>with .properties extension)**
-  Two methods for obtaining a resource bundle
    - ```
      ResourceBundle.getBundle("name");//uses default locale
      ```
    - ```
      ResourceBundle.getBundle("name", locale);
      ```
- Picking a resource bundle for `fr_FR` 
with default locale `en_US`
  - `Zoo_fr_FR.properties`
    - Requested locale
  - `Zoo_fr.properties`
    - Language we requested with no country
  - `Zoo_en_US.properties`
    - Default locale
  - `Zoo_en.properties`
    - Default locale’s language with no country
  - `Zoo.properties`
    - No locale at all—­default bundle
  - If still not found, throw
`MissingResource­Exception`
    - No locale or default bundle available
  - Or learn these steps:
    - 1. Look for the resource bundle for the requested locale, 
    followed by the one for the default locale.
    - 2. For each locale, check the language/country, followed 
    by just the language.
    - 3. Use the default resource bundle if no matching locale 
    can be found.
- What is the maximum number of files that Java would need to consider in order to 
find the appropriate resource bundle with the following code?

  ```
  Locale.setDefault(Locale.of("hi"));
  ResourceBundle rb = ResourceBundle.getBundle("Zoo",Locale.of("en"));
  ```
    The answer is three. They are listed here:
        1. `Zoo_en.properties`
        2. `Zoo_hi.properties`
        3. `Zoo.properties`
    The requested locale is `en`, so we start with that. Since the en locale does not contain
    a country, we move on to the default locale, `hi`. Again, there’s no country, so we end 
    with the default bundle.