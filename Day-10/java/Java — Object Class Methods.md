# Java — Object Class Methods

Java में हर class directly या indirectly `Object` class को inherit करती है। इसलिए हर Java object को `Object` class के methods मिलते हैं।

इसमें तीन important methods हैं:

1. `equals()`
2. `hashCode()`
3. `toString()`

## 1. `toString()` — Object Representation

`toString()` object की information को String format में return करता है।

```
class Student {
    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return id + " " + name;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student(101, "Ravi");

        System.out.println(s1);
    }
}
```

Output:

```
101 Ravi
```

- `System.out.println(s1)` internally `s1.toString()` call करता है।
- Default `toString()` class name और hash-based representation देता है, readable object data नहीं।

## 2. `equals()` — Object Comparison

`equals()` दो objects की equality check करता है।

- Default `equals()` reference comparison करता है।
- अगर objects का data compare करना है, तो इसे override करते हैं।

```
class Student {
    int id;

    Student(int id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;

        if (!(obj instanceof Student)) return false;

        Student s = (Student) obj;
        return this.id == s.id;
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student(101);
        Student s2 = new Student(101);

        System.out.println(s1.equals(s2));
    }
}
```

Output:

```
true
```

दोनों अलग objects हैं, लेकिन उनका `id` समान है।

## 3. `hashCode()` — Hash Value

`hashCode()` object का integer hash value return करता है।

इसका उपयोग `HashMap` और `HashSet` जैसी hash-based collections करती हैं।

```
@Override
public int hashCode() {
    return Integer.hashCode(id);
}
```

जब `equals()` override करते हैं, तो `hashCode()` भी override करना चाहिए।

## 4. `equals()` and `hashCode()` Contract

Very Important

Java के दोनों methods के बीच ये rules हैं:

1. अगर `a.equals(b)` `true` है, तो दोनों का `hashCode()` समान होना चाहिए।
2. अगर दोनों का `hashCode()` समान है, तो जरूरी नहीं कि `equals()` भी `true` हो।
3. अगर equality में उपयोग होने वाला data नहीं बदला, तो बार-बार `hashCode()` call करने पर same result आना चाहिए।

| equals() | hashCode() | Rule         |
| -------- | ---------- | ------------ |
| `true`   | Same       | Must be same |
| `false`  | Same       | Possible     |
| `false`  | Different  | Possible     |

याद रखने की लाइन:

Equal objects → Same hashCode.

Same hashCode → Objects जरूरी नहीं कि equal हों।

## 5. Complete Example — All Three Methods

```
import java.util.Objects;

class Student {
    int id;
    String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    @Override
    public String toString() {
        return id + " " + name;
    }

    @Override
    public boolean equals(Object obj) {
        if (this == obj) return true;
        if (!(obj instanceof Student)) return false;

        Student s = (Student) obj;
        return id == s.id && Objects.equals(name, s.name);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student(101, "Ravi");
        Student s2 = new Student(101, "Ravi");

        System.out.println(s1);
        System.out.println(s1.equals(s2));
        System.out.println(s1.hashCode() == s2.hashCode());
    }
}
```

Output:

```
101 Ravi
true
true
```

## Final Quick Revision

| Method       | Purpose                        |
| ------------ | ------------------------------ |
| `toString()` | Object की readable information |
| `equals()`   | Object equality comparison     |
| `hashCode()` | Integer hash value             |

Golden Rule: `equals()` override किया है तो `hashCode()` भी override करो, ताकि `HashMap` और `HashSet` सही तरीके से काम करें।
