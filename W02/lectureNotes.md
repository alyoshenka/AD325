# Week 2, 1/13/2022

Projects due Sundays

## Generics

`ArrayList class = new ArrayList();` -> bad! Not type safe

Java Generics = C/C++ Templates

```java
// "E" for Element
// "T" for Type
public class ArrayList<E>{

}
```

### Limitations?
- no primitives -> they are not objects -> use wrappers

- erasure: E is replaced with Object, then cast to given type -> "smoke and mirrors", "crude". Cannot do `new E()` -> no clue how to do it. (But can do E reference). Cannot do `E[10]`. Cannot instantiate objects or arrays of the generic type. Instead, do `(E[])Object[capacity]` -> references are all the same size. This "makes the compiler squirelly" -> 'how do we know the reference types are correct?' Use `SuppressWarnings("unchecked")` above method declaration.


ctrl + d -> duplicate line in IntelliJ

shift + alt + down -> move a highlighted block of code

## Data Mocking

[Handy Site](https://www.mockaroo.com/)

## Records

- kinda like a struct

- holds data

- immutable, default get methods but not set 

- class is created under the hood

- fields in constructor

- no JavaDoc will satisfy

**Try them for P1**

## XLint

Compiler *does*:

- syntax errors

- problems with static types

- checked exceptions (important/common)

But there is more stuff!

- misuse of generics

- unchecked exceptions

- raw types (ArrayList without brackets)

Linter! `-Xlint`

`-Xlint:all` for masochists

`-Xlint:unchecked,rawtypes` actually helpful

`-Xdoclint:all` for documentation

## Project 1 review

data files -> project folder, not src folder