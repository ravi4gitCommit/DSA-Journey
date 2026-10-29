Perfect 👍
Ab main tumhare **Polymorphism (Compile Time + Run Time)** ko fully correct, clean aur easy flow me bana raha hoon.

---

# 🚀 Polymorphism in Java

## 🔹 Polymorphism Kya Hai?

👉 **Poly = Many**
👉 **Morphism = Forms**

Matlab:

✔ Ek method → alag-alag form me kaam kare
✔ Same method name → different behavior

---

# 🔥 Polymorphism Ke 2 Types

1️⃣ Compile Time Polymorphism
2️⃣ Run Time Polymorphism

---

# 🟢 1️⃣ Compile Time Polymorphism

👉 Method Overloading se achieve hota hai
👉 Decision compile time pe hota hai

---

## Example:

```java
class Cal {

    public int add(int a, int b) {
        return a + b;
    }

    public int add(int a, int b, int c) {
        return a + b + c;
    }

    public double add(double a, double b) {
        return a + b;
    }
}
```

---

## Main Class

```java
public class Main {

    public static void main(String[] args) {

        Cal calc = new Cal();

        System.out.println(calc.add(5, 10));
        System.out.println(calc.add(5, 10, 5));
        System.out.println(calc.add(5.5, 10.5));
    }
}
```

---

## Output:

```
15
20
16.0
```

---

## 🔹 Why Compile Time?

✔ Compiler decide karta hai kaunsa method call hoga
✔ Method signature ke base par

---

# 🔵 2️⃣ Run Time Polymorphism

👉 Method Overriding se achieve hota hai
👉 Decision run time pe hota hai
👉 Dynamic Method Dispatch

---

## Parent Class

```java
class Animal {

    public void sound() {
        System.out.println("Animal sound");
    }
}
```

---

## Child Class

```java
class Dog extends Animal {

    @Override
    public void sound() {
        System.out.println("Woof");
    }
}
```

---

## Test Class

```java
public class Test {

    public static void main(String[] args) {

        Animal obj = new Dog();   // Upcasting

        obj.sound();
    }
}
```

---

## Output:

```
Woof
```

---

# 🔥 Dynamic Method Dispatch

```java
Animal obj = new Dog();
```

✔ Parent reference
✔ Child object

Method call runtime pe decide hota hai
Isse kehte hain → **Dynamic Method Dispatch**

---

# 🎯 Important Differences

| Compile Time          | Run Time          |
| --------------------- | ----------------- |
| Method Overloading    | Method Overriding |
| Same class            | Parent + Child    |
| Decision compile time | Decision run time |
| Fast                  | Slightly slower   |

---

# 🔥 Simple Line

✔ Method Overloading = Compile Time Polymorphism
✔ Method Overriding = Run Time Polymorphism
✔ Upcasting + Overriding = Dynamic Polymorphism

---

# 💡 Real Life Example

Remote control:

✔ Same button
✔ TV me alag kaam
✔ AC me alag kaam

Ye hi Polymorphism hai.

---

Ab tum strong ho:

✅ Method Overloading
✅ Method Overriding
✅ Compile Time
✅ Run Time
✅ Dynamic Method Dispatch
✅ Upcasting

---

Next topic:

👉 super keyword
👉 final keyword
👉 Abstract class
👉 Interface
👉 Real Interview OOPS Questions

Bolo next kya karein 🚀🔥
