# ☕ Java Varargs (Variable Arguments)

## 1. Varargs क्या है?

Java में Varargs का मतलब Variable Arguments होता है। इसकी मदद से हम किसी method में एक ही parameter के माध्यम से zero, one या multiple arguments pass कर सकते हैं।

इसमें `...` (three dots) का उपयोग होता है।

### Example

```
public class Main {
    static void sum(int... numbers) {
        int total = 0;

        for (int num : numbers) {
            total += num;
        }

        System.out.println("Sum = " + total);
    }

    public static void main(String[] args) {
        sum();
        sum(10);
        sum(10, 20);
        sum(10, 20, 30);
    }
}
```

Output:

```
Sum = 0
Sum = 10
Sum = 30
Sum = 60
```

### Working कैसे होती है?

- `int... numbers` का अर्थ है कि method zero या अधिक `int` values accept कर सकता है।
- Method के अंदर `numbers` को array की तरह इस्तेमाल किया जाता है।
- `for-each` loop से सभी values को access कर सकते हैं।

## 2. Varargs का Syntax

```
returnType methodName(dataType... variableName) {
    // Method body
}
```

Example:

```
static void printNames(String... names) {
    for (String name : names) {
        System.out.println(name);
    }
}
```

Call:

```
printNames("Ravi", "Aman", "Rahul");
```

Output:

```
Ravi
Aman
Rahul
```

## 3. Varargs के जरूरी Rules

1. एक method में अधिकतम एक Varargs parameter हो सकता है।
2. Varargs parameter method के parameter list में last में होना चाहिए।
3. Varargs में zero, one या multiple values pass कर सकते हैं।
4. Method के अंदर Varargs को array की तरह access किया जाता है।

Correct:

```
static void display(String message, int... numbers) {
    System.out.println(message);
}
```

Incorrect:

```
// Compile-time error
static void display(int... numbers, String message) {
}
```

## 4. Varargs vs Array

| Varargs                                | Array                                                   |
| -------------------------------------- | ------------------------------------------------------- |
| `int... nums`                          | `int[] nums`                                            |
| Values सीधे pass कर सकते हैं           | Array pass कर सकते हैं                                  |
| Zero या multiple arguments दे सकते हैं | Array बनाकर या existing array pass करके values देते हैं |

Example:

```
static void show(int... nums) {
    System.out.println(nums.length);
}

public static void main(String[] args) {
    show(10, 20, 30);       // Output: 3
    show(new int[]{1, 2});  // Output: 2
    show();                 // Output: 0
}
```

## 5. एक Important Interview Point

Varargs के साथ fixed parameters भी रख सकते हैं, लेकिन Varargs को सबसे आखिर में लिखना जरूरी है।

```
static void print(int id, String... subjects) {
    System.out.println("ID: " + id);

    for (String subject : subjects) {
        System.out.println(subject);
    }
}
```

Call:

```
print(101, "Java", "DSA", "SQL");
```

याद रखने की लाइन: Java Varargs (`...`) method को variable number of arguments accept करने देता है, और method के अंदर यह array की तरह काम करता है।
