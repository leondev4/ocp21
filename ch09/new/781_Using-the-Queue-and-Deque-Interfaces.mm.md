---

markmap:
  initialExpandLevel: 1
---
# **Using the `Queue` and `Deque` Interfaces**
- You use a `Queue` when elements are added and removed 
in a specific order. This is a FIFO (first-­in, first-­out) queue.

  - A `Deque` (double-ended queue), often pronounced “deck” extends `Queue`
but is different from a regular queue in that you can insert and remove
 elements from both the front (head) and back (tail).

    - You can envision a double-ended queue as shown in [Figure 9.5](https://1drv.ms/i/c/c83cfca51d5c2032/EflicKnvXVVJn2nCzY-mdCoB4RgHknqzBIoXbHm6NYmfWw?e=QvBcLl).
    Supposing we are using this as a FIFO queue. Rover is first, which
means he was first to arrive. Bella is last, which means she was last 
to arrive and has the longest wait remaining.

- Reviewing [Figure 9.1](https://1drv.ms/i/c/c83cfca51d5c2032/ET55gG4FHyxBjNF6624NagkB_WVC25_O9ZtqIQG493w3Wg?e=WhLYRs). `LinkedList` and `ArrayDeque` both implement 
the `Deque` interface,which inherits `Queue`. You saw `LinkedList` 
earlier in the List section.

  - The main benefit of a `LinkedList` is that it implements both the `List`
  and `Deque` interfaces. The trade-off is that it isn’t as efficient as a
  “pure” queue. You can use the `ArrayDeque` class if you don’t need 
  the `List` methods.

    - Constructors:
      ```java
      ArrayDeque()
      ArrayDeque(int numElements)
      ArrayDeque(Collection<? extends E> c)
      ```

- `Queue` methods
  - They throw an exception when
    something goes wrong
    - **Add to back**
      ```java
      boolean add(E e)
      ```
      **Read from front**
      ```java
      E element()
      ```
      **Get and remove from front**
      ```java
      E remove()
      ```
      - ```java
        Queue<Integer> queue = new LinkedList<>();
        queue.add(10); queue.offer(6); 
        queue.add(4);                       // [10, 6, 4] 
        System.out.println(queue.remove()); // 10 - [6,4]
        System.out.println(queue.poll());   // 6  - [4]
        System.out.println(queue.peek());   // 4  - [4]
        System.out.println(queue.element());// 4  - [4]
        System.out.println(queue);          // [4]
        ```

  - They do not throw an exception
    - **Add to back**
      ```java
      boolean offer(E e)
      ```
      **Read from front**
      ```java
      E peek()
      ```
      **Get and remove from front**
      ```
      E poll()
      ```
      - Constructors:
        ```java
        LinkedList()
        LinkedList(Collection<? extends E> c)
        ```
- Since the `Deque` interface supports double-ended queues, 
it inherits all `Queue` methods and adds more so that it is 
clear if we are working with the front or back of the queue.
  - They throw an exception
    - **Add to front**
      ```java
      void addFirst(E e)
      ```
      **Add to back**
      ```java
      void addLast(E e)
      ```
      **Read from front**
      ```java
      E getFirst()
      ```
      **Read from back**
      ```java
      E getLast()
      ```
      **Get and remove from front**
      ```java
      E removeFirst()
      ```
      **Get and remove from back**
      ```java
      E removeLast()
      ```
      - ```java
        Deque<Integer> deque = new LinkedList<>();
        deque.offerFirst(10); // true    [10]
        deque.offerLast(4);   // true    [10,4]
        deque.peekFirst();    // 10      [10,4]
        deque.pollFirst();    // 10      [4]
        deque.pollLast();     // 4       []
        //both lines do not throw a exception
        deque.pollFirst();    //          null
        deque.peekFirst();    //          null
        ```
  - They do not throw an exception
    - **Add to front**
      ```java
      boolean offerFirst(E e)
      ```
      **Add to back**
      ```java
      boolean offerLast(E e)
      ```
      **Read from front**
      ```java
      E peekFirst()
      ```
      **Read from back**
      ```java
      E peekLast()
      ```
      **Get and remove from front**
      ```java
      E pollFirst()
      ```
      **Get and remove from back**
      ```java
      E pollLast()
      ```
- LIFO (last-in, first-out) queues, which are commonly 
referred to as _stacks_. Picture a stack of plates. You
always add to or remove from the top
  - Using a `Deque` as a stack
    - They throw an exception
      - **Add to the front/top**
        ```java
        void push(E e)
        ```
        **Remove from the front/top**
        ```java
        E pop()
        ```
    - It does not throw an exception
      - **Get top/front element**
        ```java
        E peek()
        ```
    - ```java
      Deque<Integer> stack = new ArrayDeque<>();
      stack.push(10);  //    [10]
      stack.push(4);   //    [4,10]
      stack.peek();    // 4  [4,10] 
      //could be the poll() and 
      //remove() methods
      stack.pop();    // 4   [10]
      stack.pop();    // 10  []
      stack.peek();   // null
      ```
      - When using a `Deque`, it is really important to determine if it is being
      used as a FIFO queue, a LIFO stack, or a double-ended queue. To
      review, a FIFO queue is like a line of people. You get on in the back
      and off in the front. A LIFO stack is like a stack of plates. You put the
      plate on the top and take it off the top. A double-ended queue uses
      both ends.