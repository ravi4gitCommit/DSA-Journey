Perfect 👍
Ab main tumhare **Student class, Encapsulation, Getter-Setter, BankAccount example** ko fully correct, clean aur easy notes me bana raha hoon.

---

# 🎓 1️⃣ Class, Field & Local Variable

## 🔹 Example:

```java
public class Student {

    // Properties / Fields / Instance Variables
    String name;

    public static void main(String[] args) {

        String name = "Ram";   // Local variable
    }
}
```

---

## 🔹 Difference

| Type              | Kaha declare hota hai | Scope                    |
| ----------------- | --------------------- | ------------------------ |
| Instance Variable | Class ke andar        | Object ke through access |
| Local Variable    | Method ke andar       | Sirf us method tak       |

---

# 🎓 2️⃣ Proper Student Class

```java
public class Student {

    String name;
    int rollNo;
    int age;
}
```

---

## 🔹 Object Create Karna

```java
public class Test {

    public static void main(String[] args) {

        Student student = new Student();

        student.name = "Ram";
        student.rollNo = 1;

        System.out.println(student.name);
    }
}
```

---

# 🔐 3️⃣ Encapsulation

## 🔹 Problem

Agar koi directly age ko negative set kare?

```java
student.age = -15;  // Wrong
```

---

## 🔹 Solution: Encapsulation

✔ Variables ko **private** karo
✔ Getter & Setter use karo

---

## ✅ Encapsulated Student Class

```java
public class Student {

    private String name;
    private int rollNo;
    private int age;

    public void setAge(int age) {

        if(age < 0) {
            this.age = 0;
        } else {
            this.age = age;
        }
    }

    public int getAge() {
        return this.age;
    }
}
```

---

## 🔹 Use

```java
Student student = new Student();

student.setAge(-15);   // Invalid → 0
System.out.println(student.getAge());
```

---

# 🏦 4️⃣ BankAccount Example (Real Encapsulation)

---

## ✅ BankAccount Class

```java
public class BankAccount {

    private long accountNo;
    private double balance;

    public void setAccountNo(long accountNo) {
        this.accountNo = accountNo;
    }

    public long getAccountNo() {
        return accountNo;
    }

    public double getBalance() {
        return balance;
    }

    public void deposit(double amount) {

        if(amount > 0) {
            balance += amount;
            System.out.println("Deposit: " + amount);
        } else {
            System.out.println("Invalid Deposit Amount");
        }
    }

    public void withdraw(double amount) {

        if(amount > 0 && amount <= balance) {
            balance -= amount;
            System.out.println("Withdraw: " + amount);
        } else {
            System.out.println("Invalid Withdrawal");
        }
    }
}
```

---

# 🔹 Test Class

```java
public class Test {

    public static void main(String[] args) {

        BankAccount bankAccount = new BankAccount();

        bankAccount.setAccountNo(12345);

        bankAccount.deposit(-10);   // Invalid
        bankAccount.withdraw(10);   // Invalid

        bankAccount.deposit(100);   // Valid
        bankAccount.withdraw(10);   // Valid

        System.out.println("Balance: " + bankAccount.getBalance());
    }
}
```

---

# 🔥 Expected Output

```
Invalid Deposit Amount
Invalid Withdrawal
Deposit: 100.0
Withdraw: 10.0
Balance: 90.0
```

---

# 🎯 Important OOPS Concepts

✔ Field = Instance variable
✔ Local variable = Method ke andar
✔ Encapsulation = Data hiding
✔ Private → Secure
✔ Getter → Read
✔ Setter → Modify
✔ Validation logic → Setter ke andar

---

# 💡 Real Life Meaning

Encapsulation = Bank locker

✔ Bahar se direct access nahi
✔ Sirf allowed method se access

---

Ab tum samajh gaye:

✅ Instance vs Local variable
✅ Encapsulation
✅ Getter/Setter
✅ Validation
✅ Real OOPS example

---

Next powerful topic:

👉 Constructor
👉 this keyword
👉 Static keyword
👉 Polymorphism
👉 Abstraction

Bolo next kya karein 🚀🔥
