# Week 3 Textbook Notes

## Chapter 14, Stacks and Queues

stack -> LIFO

queue -> FIFO

```java
public class StackExample {
    public static void main(String[] args) {
        String[] data = {"to", "be", "or", "not", "to",
        "be"};
        Stack<String> s = new Stack<>();

        for (String str : data) {
            s.push(str);
        }

        System.out.println("stack = " + s);
        System.out.println("size = " + s.size());
        System.out.println("peek = " + s.peek());
        while (!s.isEmpty()) {
            System.out.print(s.pop() + " ");
        }
        System.out.println();
    }
}
```

It produces the following output:

```java
stack = [to, be, or, not, to, be]
size = 6
peek = be
// notice this is backwards
be to not or be to
```

enqueue -> add

dequeue -> remove

```java
// variable type uses interface, constructor uses implementation
Queue<Integer> q = new LinkedList<>();
```

```java
// transfer data from a stack to a queue
public static void queueToStack(Queue<Integer> q, Stack<Integer> s) {
    while (!q.isEmpty()) {
        s.push(q.remove());
    }
}
```

### Sum of a Queue, without destroying data

```java
// Best version that re-adds elements to the queue passed in
// so that it does not destroy the contents of the queue.
public static int sum(Queue<Integer> q) {
    int sum = 0;
    for (int i = 0; i < q.size(); i++) {
        int n = q.remove();
        sum += n;
        q.add(n);
    }
    return sum;
}
```

### Tokenizer

```java
import java.util.*;

// This class breaks up a string into a sequence of tokens using
// both whitespace and a list of special characters that are each
// considered tokens. The special characters in this case are
// used to tokenize an arithmetic expression. For example the
// expression:
// 2*3.8/(4.95−7.8)
// would be tokenized as 2 * 3.8 / ( 4.95 − 7.8 ) even though it
// has no whitespace to separate these tokens. 
public class StringSplitter {
    private Queue<Character> characters;
    private String token;

    public static final String SPECIAL_CHARACTERS = "()+−*/^";

    public StringSplitter(String line) {
        characters = new LinkedList<>();
        for (int i = 0; i < line.length(); i++) {
            characters.add(line.charAt(i));
        }
        findNextToken();
    }

    // post: Returns true if there is another token
    public boolean hasNext() {
        return token != null;
    }

    // pre : there is another token to return (throws
    // NoSuchElementException if not)
    // post: returns and consumes the next token
    public String next() {
        checkToken();
        String result = token;
        findNextToken();
        return result;
    }

    // pre : there is another token to return (throws
    // NoSuchElementException if not)
    // post: returns the next token without consuming it
    public String peek() {
        checkToken();
        return token;
    }

    // post: finds the next token, if any
    private void findNextToken() {
        while (!characters.isEmpty() &&
        Character.isWhitespace(characters.peek())) {
            characters.remove();
        }
        if (characters.isEmpty()) {
            token = null;
        } else {
            token = "" + characters.remove();
            if (!SPECIAL_CHARACTERS.contains(token)) {
                boolean done = false;
                while (!characters.isEmpty() && !done) {
                    char ch = characters.peek();
                    if (Character.isWhitespace(ch) ||
                    SPECIAL_CHARACTERS.indexOf(ch) >= 0)
                    {
                        done = true;
                    } else {
                        token = token +
                        characters.remove();
                    }
                }
            }
        }
    }

    // post: throws an exception if there is no token left
    private void checkToken() {
        if (!hasNext()) {
            throw new NoSuchElementException();
        }
    }
}

// implementation
StringSplitter data = new StringSplitter(line);
Stack<String> symbols = new Stack<>();
Stack<Double> values = new Stack<>();
boolean error = false;
while (!error && data.hasNext()) {
String next = data.next();
    if (next.equals(")")) { // process )        
        if (symbols.size() < 2 || symbols.peek().equals("(")) {
            error = true;
        } else {
            String operator = symbols.pop();
            if (!symbols.peek().equals("(")) {
                error = true;
            } else {
                symbols.pop(); // to remove the "("
                double op2 = values.pop();
                double op1 = values.pop();
                double value = evaluate(operator, op1, op2);
                values.push(value);
            }
        }
    } else if ("(+−*/^".contains(next)) {
        symbols.push(next);
    } else { // it should be a number
        values.push(Double.parseDouble(next));
    }
}
if (error || values.size() != 1 || !symbols.isEmpty()) {
    System.out.println("illegal expression");
} else {
    System.out.println(values.pop());
}
```

```java
// This program prompts for fully parenthesized arithmetic
// expressions and it evalues each expression. It uses two
// stacks to evaluate the expressions.

import java.util.*;

public class Evaluator {
    public static void main(String[] args) {
        System.out.println("This program evaluates fully");
        System.out.println("parenthesized expressions with the");
        System.out.println("operators +, −, *, /, and ^");
        System.out.println();
        Scanner console = new Scanner(System.in);
        System.out.print("expression (return to quit)? ");
        String line = console.nextLine().trim();
        while (line.length() > 0) {
            evaluate(line);
            System.out.print("expression (return to quit)? ");
            line = console.nextLine().trim();
        }
    }

    // pre : line contains a fully parenthesized expression
    // post: prints the value of the expression or an error
    // message if the expression is not legal
    public static void evaluate(String line) {
        StringSplitter data = new StringSplitter(line);
        Stack<String> symbols = new Stack<>();
        Stack<Double> values = new Stack<>();
        boolean error = false;
        while (!error && data.hasNext()) {
            String next = data.next();
            if (next.equals(")")) {
                if (symbols.size() < 2 ||
                symbols.peek().equals("(")) {
                error = true;
                } else {
                    String operator = symbols.pop();
                    if (!symbols.peek().equals("(")) {
                        error = true;
                    } else {
                        symbols.pop(); // to remove the "("
                        double op2 = values.pop();
                        double op1 = values.pop();
                        double value = evaluate(operator, op1, op2);
                        values.push(value);
                    }
                }
            } else if ("(+−*/^".contains(next)) {
                symbols.push(next);
            } else { // it should be a number
                values.push(Double.parseDouble(next));
            }
        }
        if (error || values.size() != 1 || !symbols.isEmpty()) {
            System.out.println("illegal expression");
        } else {
            System.out.println(values.pop());
        }
    }

    // pre : operator is one of +, −, *, /, or ^
    // post: returns the result of applying the given operator to
    // the given operands
    public static double evaluate(String operator, double operand1, double operand2) {
        if (operator.equals("+")) {
            return operand1 + operand2;
        } else if (operator.equals("−")) {
            return operand1 – operand2;
        } else if (operator.equals("*")) {
            return operand1 * operand2;
        } else if (operator.equals("/")) {
            return operand1 / operand2;
        } else if (operator.equals("^")) {
            return Math.pow(operand1, operand2);
        } else {
            throw new RuntimeException("illegal operator " + operator);
        }
    }
}
```

