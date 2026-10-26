Perfect 👍
Ab main tumhare **OOPS + Methods** wale notes ko fully correct, simple, clean aur proper flow me bana raha hoon.

---

# 🚀 OOPS (Object Oriented Programming)

## 🔹 OOPS Kya Hai?

OOPS ek programming technique hai jisme hum:

👉 **Class aur Object** ke through program banate hain
👉 Real world ko code me represent karte hain

---

# 🔹 1️⃣ Class & Object

## ✅ Class

Class ek blueprint hoti hai.

Example: Car ka design

---

## ✅ Object

Object class ka real instance hota hai.

Example: Blue car, Red car

---

## 🔹 Example: Car Class

```java
public class Car {

    String color;
    String brand;
    String model;
    private int year;
    int speed;

    public void accelerate(int increment) {
        speed += increment;
    }

    public void brake(int decrement) {
        speed -= decrement;
        if(speed < 0) {
            speed = 0;
        }
    }
}
```

---

## 🔹 Using Object in Main Method

```java
public class Test {

    public static void main(String[] args) {

        Car car = new Car();

        car.color = "Blue";
        car.accelerate(10);

        System.out.println(car.speed);
    }
}
```

---

# 🔹 2️⃣ Encapsulation

Encapsulation ka matlab:

👉 Data + Methods ko ek single unit (class) me bandhna

Example:

```java
private int year;
```

✔ Private variable ko directly access nahi kar sakte
✔ Isse data secure hota hai

---

# 🔹 3️⃣ Inheritance

Inheritance ka matlab:

👉 Ek class dusri class ki properties use kare

---

## Example:

```java
public class Animal {

    public void makeSound() {
        System.out.println("Some sound");
    }
}
```

```java
public class Cat extends Animal {

    String breed;
}
```

✔ `extends` keyword use hota hai
✔ Cat ko Animal ki properties mil gayi

---

# 🔹 4️⃣ For-each Loop Example

```java
int[] arr = {1, 2, 3, 4};
int res = 0;

for(int i : arr) {
    res += i;
}
```

---

# 🔹 5️⃣ Methods in Java

## ✅ Main Method

```java
public static void main(String[] args) {

}
```

---

# 🔹 6️⃣ Method with Return Type

## ✅ Sum of Array

```java
public static void main(String[] args) {

    int[] a = {3, 4, 3, 3, 3, 4, 56, 67};

    System.out.println(sumOfArray(a));
}
```

```java
public static int sumOfArray(int[] arr) {

    int res = 0;

    for(int i : arr) {
        res += i;
    }

    return res;
}
```

---

# 🔹 7️⃣ String Method Example

```java
private static String upperFun(String str) {

    return str.trim().toUpperCase();
}
```

Use:

```java
String res = upperFun("   ram   ");
System.out.println(res);
```

Output:

```
RAM
```

---

# 🔹 8️⃣ Void Method

Void method value return nahi karta.

```java
private static void sum(int a, int b) {

    System.out.println(a + b);
}
```

Use:

```java
sum(3, 7);
```

---

# 🔥 Important Concepts Summary

✔ Class → blueprint
✔ Object → real instance
✔ Encapsulation → data hiding
✔ Inheritance → reuse code
✔ Method → reusable block
✔ void → no return
✔ return type → value return karta hai

---

# 🎯 OOPS Pillars (Important for Interview)

1. Encapsulation
2. Inheritance
3. Polymorphism
4. Abstraction

---

Ab tum:

✅ Class bana sakte ho
✅ Object use kar sakte ho
✅ Method likh sakte ho
✅ Inheritance samajh gaye

---

Next level topic:

👉 Constructor
👉 Polymorphism
👉 Abstraction
👉 Getter & Setter
👉 Real OOPS Interview Questions

Bolo next kya karna hai 🔥🚀
