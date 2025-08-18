# **`Map` methods**
- ```java
    void clear()
    ```
    - Removes all keys and values from map.
- ```java
  boolean containsKey(Object key)
  ```
  - Returns whether key is in map.
- ```java
  boolean containsValue( Object value)
  ```
  - Returns whether value is in map.
- ```java
  Set<Map.Entry<K,V>> entrySet()
  ```
  - Returns `Set` of key/value pairs.
- ```java
  void forEach(BiConsumer<K, V> action)
  ```
  - Loops through each key/value pair.
- ```java
  V get(Object key)
  ```
  - Returns value mapped by key or `null` if none is mapped.
- ```java
  V getOrDefault(Object key, V defaultValue)
  ```
  - Returns value mapped by key or default value if none is mapped.
- ```java
  boolean isEmpty()
  ```
  - Returns whether map is empty.
- ```java
  Set<K> keySet()
  ```
  - Returns set of all keys.
- ```java
  V merge(K key, V value, BiFunction<V, V, V> func)
  ```
  - Sets value if key not set. Runs function if key is set, 
  to determine new value. Removes if value is `null`.
- ```java
  V put(K key, V value)
  ```
  - Adds or replaces key/value pair. Returns previous value or `null`.
- ```java
  V putIfAbsent(K key, V value)
  ```
  - Adds value if key not present and returns `null`. 
  Otherwise, returns existing value.
- ```java
  V remove(Object key)
  ```
  - Removes and returns value mapped to key. Returns `null` if none.
- ```java
  V replace(K key, V value)
  ```
  - Replaces value for given key if key is set. Returns original value or `null` if none.
- ```java
  void replaceAll(BiFunction<K, V, V> func)
  ```
  - Replaces each value with results of function.
- ```java
  int size()
  ```
  - Returns number of entries (key/value pairs) in map.
- ```java
  Collection<V> values()
  ```
  - Returns `Collection` of all values.
