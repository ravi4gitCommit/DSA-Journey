# Java Notes — Cloning, Shallow Copy, Deep Copy & Immutability

## Topic 1. Cloning in Java

### 1. Cloning क्या है?

Cloning का मतलब है किसी existing object की एक copy बनाना।

Java में `Object` class का `clone()` method object की copy बनाने के लिए use होता है।

इसके लिए class को `Cloneable` interface implement करना पड़ता है।

### 2. Shallow Copy क्या है?

Shallow copy में नया object बनता है, लेकिन उसके अंदर मौजूद reference variables पुराने object के same referenced objects को point करते हैं।

[Cloneable interface in Java| Scaler Topics](https://images.openai.com/static-rsc-4/8SpOq30J4YXx-vXkCD0xyzO9BFaOmHea1D34TB3Fb96G-sQaLrq7TJ93NCqawOuTPYUAKPzMwraKf2fkhk6XpjK54xQueAgVZFncNoeDUQH5UIMNrbUMutPDV2xKWBnDrCks6xvie12gFqsNMaiG0ppfVmHhZpqJk9QkQJG52o4?purpose=inline)

[scaler.com](https://www.scaler.com/topics/cloneable-interface-in-java/?utm_source=chatgpt.com)

```
class Address {
    String city;

    Address(String city) {
        this.city = city;
    }
}

class Student implements Cloneable {
    String name;
    Address address;

    Student(String name, Address address) {
        this.name = name;
        this.address = address;
    }

    @Override
    public Object clone() throws CloneNotSupportedException {
        return super.clone();
    }
}

public class Main {
    public static void main(String[] args) throws Exception {
        Address address = new Address("Patna");

        Student s1 = new Student("Ravi", address);
        Student s2 = (Student) s1.clone();

        s2.address.city = "Delhi";

        System.out.println(s1.address.city);
        System.out.println(s2.address.city);
    }
}
```

Output:

```
Delhi
Delhi
```

क्यों? दोनों Student objects के `address` references एक ही Address object को point कर रहे हैं।

### 3. Deep Copy क्या है?

Deep copy में नया object और उसके अंदर मौजूद mutable referenced objects की भी अलग-अलग copies बनाई जाती हैं।

[How does clone() method works in Java? - by javinpaul](https://images.openai.com/static-rsc-4/l-QYVE6fMrUAt6pw_QA4bWCFKoPOS7dYTEplX6dHXyVFEoUwlINbO1ZZYGeuWYXHnSpsQTKYTWlqnZd6s6ybjMf0Ec-SKTLrNSDVy9xY7kWzUDRr8Qv9h6Uva-VwOnzcrfRngNLcTsphD633Ne87ZevOWoTbnPf1HYsnVYe5yKI?purpose=inline)

[javarevisited.substack.com](https://javarevisited.substack.com/p/how-does-clone-method-works-in-java?utm_source=chatgpt.com)

```
class Address {
    String city;

    Address(String city) {
        this.city = city;
    }
}

class Student {
    String name;
    Address address;

    Student(String name, Address address) {
        this.name = name;
        this.address = address;
    }

    // Deep copy
    Student deepCopy() {
        Address copiedAddress = new Address(this.address.city);
        return new Student(this.name, copiedAddress);
    }
}

public class Main {
    public static void main(String[] args) {
        Student s1 = new Student("Ravi", new Address("Patna"));

        Student s2 = s1.deepCopy();

        s2.address.city = "Delhi";

        System.out.println(s1.address.city);
        System.out.println(s2.address.city);
    }
}
```

Output:

```
Patna
Delhi
```

क्यों? दोनों Student objects के पास अलग-अलग Address objects हैं।

### 4. Shallow Copy vs Deep Copy

| Shallow Copy                               | Deep Copy                                  |
| ------------------------------------------ | ------------------------------------------ |
| नया outer object बनता है                   | नया outer object बनता है                   |
| Nested objects share होते हैं              | Nested mutable objects की अलग copy होती है |
| Nested changes दोनों को affect कर सकते हैं | Nested changes independent होते हैं        |
| Comparatively आसान                         | Comparatively अधिक काम                     |

याद रखने की लाइन:

- Shallow Copy = Outer object नया, nested references shared.
- Deep Copy = Outer object नया, nested mutable objects भी नए।

## Topic 2. Immutability in Java

### 1. Immutability क्या है?

Immutability का मतलब है कि object बनने के बाद उसकी internal state को change नहीं किया जा सकता।

Example: Java का `String` immutable है।

```
public class Main {
    public static void main(String[] args) {
        String name = "Ravi";

        name.concat(" Sharma");

        System.out.println(name);
    }
}
```

Output:

```
Ravi
```

`concat()` original String को change नहीं करता, बल्कि नया String return करता है।

अगर result store करें:

```
name = name.concat(" Sharma");
System.out.println(name);
```

Output:

```
Ravi Sharma
```

### 2. Immutable Class कैसे बनाते हैं?

Immutable class बनाने के मुख्य rules:

1. Class को `final` बनाएं, ताकि कोई subclass state-changing behavior न जोड़ सके।
2. सभी fields को `private final` बनाएं।
3. Setter methods न बनाएं।
4. Constructor से सभी fields initialize करें।
5. Mutable objects को constructor में defensive copy करें।
6. Mutable objects को getter से सीधे return न करें; उनकी copy return करें।

### 3. Complete Example — Immutable Class

```
final class Student {
    private final int id;
    private final String name;

    Student(int id, String name) {
        this.id = id;
        this.name = name;
    }

    public int getId() {
        return id;
    }

    public String getName() {
        return name;
    }
}

public class Main {
    public static void main(String[] args) {
        Student student = new Student(101, "Ravi");

        System.out.println(student.getId());
        System.out.println(student.getName());
    }
}
```

Output:

```
101
Ravi
```

यह class immutable है क्योंकि:

- Class `final` है।
- Fields `private final` हैं।
- कोई setter नहीं है।
- `String` immutable type है।
- Fields की values constructor में set होती हैं।

## Topic 3. Mutable Object के साथ Immutable Class

अगर class में `Address` जैसा mutable object है, तो केवल `private final` लगाने से class पूरी तरह immutable नहीं बनती।

क्योंकि `final` reference को दूसरा object assign करने से रोकता है, लेकिन referenced object को modify करने से नहीं।

### Complete Example

```
final class Address {
    private String city;

    Address(String city) {
        this.city = city;
    }

    public String getCity() {
        return city;
    }

    public void setCity(String city) {
        this.city = city;
    }
}

final class Student {
    private final int id;
    private final Address address;

    Student(int id, Address address) {
        this.id = id;

        // Defensive copy
        this.address = new Address(address.getCity());
    }

    public int getId() {
        return id;
    }

    public Address getAddress() {
        // Defensive copy
        return new Address(address.getCity());
    }
}

public class Main {
    public static void main(String[] args) {
        Address address = new Address("Patna");

        Student student = new Student(101, address);

        address.setCity("Delhi");

        Address copiedAddress = student.getAddress();
        copiedAddress.setCity("Mumbai");

        System.out.println(student.getAddress().getCity());
    }
}
```

Output:

```
Patna
```

यहाँ क्या हुआ?

1. Constructor ने original Address की अलग copy बनाई।
2. Original Address को बदलने से Student पर असर नहीं हुआ।
3. Getter ने भी Address की अलग copy return की।
4. Returned copy बदलने से Student की internal state नहीं बदली।

इसे Defensive Copying कहते हैं।

## Final Quick Revision

| Concept        | Meaning                                             |
| -------------- | --------------------------------------------------- |
| Cloning        | Existing object की copy बनाना                       |
| Shallow Copy   | Nested references shared रहते हैं                   |
| Deep Copy      | Nested mutable objects की अलग copies                |
| Immutability   | Object की state create होने के बाद change नहीं होती |
| `final` class  | Class को extend नहीं कर सकते                        |
| `final` field  | Field को दोबारा assign नहीं कर सकते                 |
| Defensive Copy | Mutable object की safe copy बनाना                   |

## याद रखने की लाइन

- Cloning: Object की copy बनाना।
- Shallow Copy: Nested object shared।
- Deep Copy: Nested mutable objects अलग।
- Immutable Class: No setters + `private final` fields + defensive copying जहाँ जरूरी हो।

Important: `Cloneable` केवल cloning की अनुमति बताने वाला marker interface है। `Object.clone()` default रूप से shallow copy करता है। Deep copy के लिए nested mutable objects को खुद copy करना पड़ता है।
