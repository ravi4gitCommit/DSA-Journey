Perfect 👍
Ab main tumhare **Inheritance + Method Overriding + Types of Inheritance** ko fully correct, clean aur easy notes me bana raha hoon.

---

# 🚀 Inheritance in Java

## 🔹 Inheritance Kya Hai?

👉 Ek class dusri class ki properties aur methods ko use kare
👉 Code reuse hota hai

Use keyword:

```
extends
```

---

# 🐾 1️⃣ Basic Inheritance Example

---

## Parent Class

```java
public class Animal {

    private String name;
    private int age;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }

    public void sayHello() {
        System.out.println("Animal sound");
    }
}
```

---

## Child Class

```java
public class Dog extends Animal {

    @Override
    public void sayHello() {
        System.out.println("Woof");
    }
}
```

---

## Test Class

```java
public class Test {

    public static void main(String[] args) {

        Dog dog = new Dog();
        dog.sayHello();
    }
}
```

### Output:

```
Woof
```

---

# 🔥 Method Overriding

## 🔹 Definition

Jab child class parent ke method ko same signature ke saath redefine kare → **Method Overriding**

✔ Method name same
✔ Parameters same
✔ Return type same

Example:

```java
@Override
public void sayHello() {
    System.out.println("Woof");
}
```

---

# 🧬 2️⃣ Multilevel Inheritance

GrandParent → Parent → Child

---

## GrandParent

```java
public class GrandParent {

    private String name;

    public void setName(String name) {
        this.name = name;
    }

    public String getName() {
        return name;
    }
}
```

---

## Parent

```java
public class Parent extends GrandParent {

    private int age;

    public void setAge(int age) {
        this.age = age;
    }

    public int getAge() {
        return age;
    }
}
```

---

## Child

```java
public class Child extends Parent {

}
```

---

## Test

```java
public class Test {

    public static void main(String[] args) {

        Child child = new Child();

        child.setName("Ram");
        child.setAge(18);

        System.out.println(child.getName());
        System.out.println(child.getAge());
    }
}
```

✔ Child ko Parent + GrandParent dono ki properties milti hain

---

# 🌳 3️⃣ Hierarchical Inheritance

Ek parent → multiple children

---

## Parent Class

```java
public class Animal {

    public void sayHello() {
        System.out.println("Animal sound");
    }
}
```

---

## Child 1

```java
public class Dog extends Animal {

    public void sayHello() {
        System.out.println("Woof");
    }
}
```

---

## Child 2

```java
public class Cat extends Animal {

    public void sayHello() {
        System.out.println("Meow");
    }
}
```

---

## Test

```java
public class Test {

    public static void main(String[] args) {

        Dog dog = new Dog();
        Cat cat = new Cat();

        dog.sayHello();
        cat.sayHello();
    }
}
```

### Output:

```
Woof
Meow
```

✔ Ek parent se multiple child classes
✔ Isse Hierarchical Inheritance kehte hain

---

# 🎯 Important Rules

✔ Private members direct access nahi hote
✔ Parent method override kar sakte hain
✔ @Override likhna good practice hai
✔ Java me multiple inheritance (class to class) allowed nahi

---

# 🔥 Types of Inheritance in Java

1️⃣ Single Inheritance
2️⃣ Multilevel Inheritance
3️⃣ Hierarchical Inheritance

❌ Multiple Inheritance (class to class) allowed nahi

---

# 💡 Summary

✔ extends → inheritance
✔ Method override → child redefine method
✔ Multilevel → chain inheritance
✔ Hierarchical → one parent many children

---

Ab tum strong ho:

✅ Inheritance
✅ Method Overriding
✅ Multilevel
✅ Hierarchical

---

Next powerful topic:

👉 super keyword
👉 Constructor in inheritance
👉 Polymorphism
👉 Abstract class
👉 Interface

Bolo next kya karein 🚀🔥
