# Week 2 Textbook Notes

## Chapter 10, ArrayLists

arrayList: array, but with variable length

stored internally as an array, maintains fast access

```java
import java.util.ArrayList;
```

```java
ArrayList<E> list = new ArrayList<>();
list.add(stuff);
```

don't have to worry about `toString()`

here's a scanner example:
```java
// removes duplicates from a list
Scanner input = new Scanner(new File("names.txt"));
ArrayList<String> list = new ArrayList<>();
while (input.hasNext()) {
    String name = input.next();
    if (!list.contains(name)) {
    list.add(name);
    }
}
System.out.println("list = " + list);
```

uses `equals` method for comparing

```java
// create an ArrayList of names of days of the week (shorter)
ArrayList<String> dayNames = new ArrayList<>(
    List.of("Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"));
 ```

for-each loops:

```java
for (<type> <name>: <structure>) {
    <statement>;
    <statement>;
    ...
    <statement>;
}

// cannot modify while iterating

int sum = 0;
for (String s: list) {
    sum += s.length();
}
System.out.println("Total of lengths = " + sum);

// different approach, same result
int sum = list.stream().mapToInt(String::length).sum();
System.out.println("Total of lengths = " + sum);
```

ArrayLists cannot be declared with primitive types, so must use wrapper classes

But Java converts between them, sometimes -> "Boxing"

Boxing:
An automatic conversion from primitive data to a
wrapped object of the appropriate type (e.g., an int
boxed to form an Integer ).

There's also Unboxing

Classes must implement `Comparable` interface

```java
public interface Comparable<T> {
    // -: <
    // 0: =
    // +: >
    public int compareTo(T other);
}

public class CalendarDate implements Comparable<CalendarDate> {
    ...
}
```

## Chapter 15, Implementing a Collection Class

```java
public class ArrayIntList {
    public static final int DEFAULT_CAPACITY = 100;

    private int[] elementData;
    private int size;

    // Objects of this class can be used to iterate over an
    // ArrayIntList and remove values from the list.
    import java.util.NoSuchElementException;

    // inner class, can access methods and fields of the outer class
    // do not declare ArrayListIterator<E>...
    private class ArrayIntListIterator implements Iterator<E> {
        private ArrayIntList list; // list to iterate over
        private int position; // current list position
        private boolean removeOK; // okay to remove now?

        // post: constructs an iterator for the given list
        public ArrayIntListIterator(ArrayIntList list) {
            this.list = list;
            position = 0;
            removeOK = false;
        }

        // post: returns true if there are more elements left
        public boolean hasNext() {
            return position < list.size();
        }

        // pre : hasNext() (throws NoSuchElementException if not)
        // post: returns the next element in the iteration
        public int next() {
            if (!hasNext()) {
                throw new NoSuchElementException();
            }
            int result = list.get(position);
            position++;
            removeOK = true;
            return result;
        }

        // pre : next() has been called without a call on remove
        // (throws IllegalStateException if not)
        // post: removes the last element returned by the iterator
        public void remove() {
            if (!removeOK) {
                throw new IllegalStateException();
            }
            list.remove(position – 1);
            position––;
            removeOK = false;
        }
    }
    
    
    public ArrayIntList() {
        // calling other constructor
        this(DEFAULT_CAPACITY);
    }

    public ArrayIntList(int capacity) {
        elementData = new int[capacity];
        // initialized to 0 by default
        size = 0;
    }

    public void add(int value) {
        elementData[size] = value;
        size++;
    }

    public int size() {
        return size;
    }

    public int get(int index) {
        return elementData[index];
    }

    public String toString() {
        if (size == 0) {
            return "[]";
        } else {
            String result = "[" + elementData[0];
            for (int i = 1; i < size; i++) {
                result += ", " + elementData[i];
            }
            result += "]";
            return result;
        }
    }

    public int indexOf(int value) {
        for (int i = 0; i < size; i++) {
            if (elementData[i] == value) {
                return i;
            }
        }
        return –1;
    }

    public void remove(int index) {
        for (int i = index; i < size – 1; i++) {
            elementData[i] = elementData[i + 1];
        }
        size––;
        // don't need to overwrite

        // BUT! if this were generic you would.
        // mark as null so GC cleans
    }

    public void add(int index, int value) {
        for (int i = size; i >= index + 1; i––) {
            elementData[i] = elementData[i – 1];
        }
        elementData[index] = value;
        size++;
    }

    public ArrayIntListIterator iterator() {
        return new ArrayIntListIterator(this);
    }


    // make it longer, if needed
    public void ensureCapacity(int capacity) {
        if (capacity > elementData.length) {
            int newCapacity = elementData.length * 2 + 1;
            if (capacity > newCapacity) {
                newCapacity = capacity;
            }
            elementData = Arrays.copyOf(elementData, newCapacity);
        }
    }
}
```

```java
if(badArgument){
    throw new IllegalArgumentException("message");
}
```

also can use `IllegalStateException` or `IndexOutOfBoundsException` and many others

In Java, `private` fields are "private to the class." So list1 can access list2.size in its member functions.

Iterators:

`hasNext()`: returns true if there are more elements to be
examined

`next()`: returns the next element from the list and
advances the position of the iterator by one

`remove()`: removes the element most recently returned by
next()

casting with a generic

`elementData = new E[capacity];` -> `elementData = (E[]) new Object[capacity];`

suppress warning about generic array (annotation)

```java
@SuppressWarnings("unchecked")
public ArrayList(int capacity) {
    ...
}
```

## Chapter 16, Linked Lists

