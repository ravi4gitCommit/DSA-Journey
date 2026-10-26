final keyword

Perfect 👍
Ab main tumhe **final keyword** ke clean, simple aur perfect notes bana kar de raha hoon.

---

# 🔐 Final Keyword in Java

## 🔹 final Kya Hai?

👉 `final` keyword ka matlab hota hai:

**“Change nahi ho sakta”**

Iska use 3 jagah hota hai:

1. Final Variable
2. Final Method
3. Final Class

---

# 🔥 1️⃣ Final Variable

👉 Final variable ki value change nahi ho sakti.

---

## ✅ Example

```java
public class Test {

    final int age = 18;

    public void change() {
        // age = 20; ❌ Error
    }
}
```

✔ Ek baar value assign ho gayi
✔ Dubara change nahi kar sakte

---

## 🔹 Final Variable Without Initialization

```java
public class Student {

    final int id;

    public Student(int id) {
        this.id = id;
    }
}
```

✔ Constructor ke andar initialize kar sakte hain
✔ Isko blank final variable kehte hain

---

# 🔥 2️⃣ Final Method

👉 Final method override nahi ho sakta.

---

## ✅ Example

```java
class Animal {

    final void sound() {
        System.out.println("Animal sound");
    }
}

class Dog extends Animal {

    // void sound() { } ❌ Error
}
```

✔ Child class method override nahi kar sakti

---

# 🔥 3️⃣ Final Class

👉 Final class inherit nahi ho sakti.

---

## ✅ Example

```java
final class Animal {

}

// class Dog extends Animal { } ❌ Error
```

✔ Koi class isko extend nahi kar sakti

---

# 🎯 Important Example

Java me:

```java
public final class String {
}
```

`String` class final hai
Isliye koi usko extend nahi kar sakta

---

# 🔥 Final with Reference Variable

```java
final Student s = new Student();
```

✔ Object change nahi kar sakte

```
s = new Student(); ❌ Error
```

✔ But object ke andar ke data change kar sakte ho

```
s.name = "Ram"; ✅
```

---

# 🔥 Final vs Finally vs Finalize

| Keyword    | Meaning                    |
| ---------- | -------------------------- |
| final      | constant / restrict change |
| finally    | exception block            |
| finalize() | garbage collector method   |

---

# 💡 Real Life Example

Final variable = Aadhar number
✔ Ek baar assign
✔ Change nahi ho sakta

Final class = RBI rules
✔ Koi modify nahi kar sakta

---

# 🔥 Summary

✔ Final variable → value change nahi
✔ Final method → override nahi
✔ Final class → inherit nahi
✔ Final reference → object change nahi

---

Ab tum strong ho:

✅ Final variable
✅ Final method
✅ Final class
✅ Important rules

---

Next topic:

👉 this vs super
👉 Interface
👉 Abstract vs Interface
👉 Encapsulation
👉 Full OOPS Revision

Bolo next kya karein 🚀🔥
