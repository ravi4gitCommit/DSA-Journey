# Java Notes — Composition, Inheritance, Interfaces & Inner Classes

ये तुम्हारे सभी covered topics के organized notes हैं — simple Hinglish, correct Java syntax, examples और runnable code के साथ। इन्हें GitHub notes में भी रख सकते हो।

## Topic 1. Composition over Inheritance

### 1. Composition क्या है?

Composition में एक class अपने अंदर दूसरी class का object रखती है। इसे has-a relationship कहते हैं।

Example: A `Car` has an `Engine`.

```
class Engine {
    void start() {
        System.out.println("Engine started");
    }
}

class Car {
    private Engine engine = new Engine();

    void startCar() {
        engine.start();
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.startCar();
    }
}
```

Output:

```
Engine started
```

Working:

1. `Engine` class engine का काम करती है।
2. `Car` के पास `Engine` का object है।
3. `startCar()` method, `engine.start()` को call करता है।

### 2. Inheritance क्या है?

Inheritance में एक class दूसरी class की properties और methods inherit करती है। इसे is-a relationship कहते हैं।

```
class Animal {
    void eat() {
        System.out.println("Animal is eating");
    }
}

class Dog extends Animal {
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.eat();
    }
}
```

Output:

```
Animal is eating
```

### 3. Composition vs Inheritance

| Composition                         | Inheritance                         |
| ----------------------------------- | ----------------------------------- |
| Has-a relationship                  | Is-a relationship                   |
| दूसरी class का object रखती है       | `extends` का उपयोग होता है          |
| Components को बदलना आसान हो सकता है | Parent class से जुड़ाव अधिक होता है |
| Example: Car has an Engine          | Example: Dog is an Animal           |

याद रखने की लाइन: जब किसी class को दूसरी class का काम इस्तेमाल करना हो, तो Composition अच्छा विकल्प हो सकता है। Inheritance तब उपयोग करें जब वास्तविक is-a relationship हो।

## Topic 2. SmartPhone using Composition

इस example में `SmartPhone`, `Phone`, `Camera` और `MusicPlayer` की functionality को objects के जरिए इस्तेमाल करता है।

```
class Phone {
    void call() {
        System.out.println("Calling...");
    }
}

class Camera {
    void takePhoto() {
        System.out.println("Photo taken");
    }
}

class MusicPlayer {
    void playMusic() {
        System.out.println("Music playing");
    }
}

class SmartPhone {
    private Phone phone = new Phone();
    private Camera camera = new Camera();
    private MusicPlayer musicPlayer = new MusicPlayer();

    void makeCall() {
        phone.call();
    }

    void takePhoto() {
        camera.takePhoto();
    }

    void playMusic() {
        musicPlayer.playMusic();
    }
}

public class Main {
    public static void main(String[] args) {
        SmartPhone smartphone = new SmartPhone();

        smartphone.makeCall();
        smartphone.takePhoto();
        smartphone.playMusic();
    }
}
```

Output:

```
Calling...
Photo taken
Music playing
```

Main concept: `SmartPhone` को इन तीनों classes से `extends` करने की जरूरत नहीं है। वह उनके objects का उपयोग करता है।

## Topic 3. Interface Basics

### 1. Interface क्या है?

Interface एक contract की तरह होता है। यह बताता है कि implementing class को कौन-से methods provide करने होंगे।

```
interface Animal {
    void eat();
    void sleep();
}

class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("Dog is eating");
    }

    @Override
    public void sleep() {
        System.out.println("Dog is sleeping");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();

        dog.eat();
        dog.sleep();
    }
}
```

Output:

```
Dog is eating
Dog is sleeping
```

Important points:

- Interface को `interface` keyword से बनाते हैं।
- Class interface को `implements` करती है।
- सामान्य abstract methods को implement करने वाले methods `public` होने चाहिए।
- `@Override` बताता है कि हम inherited या interface method implement/override कर रहे हैं।

### 2. Interface Abstract Methods

Interface में बिना body वाला सामान्य method implicit रूप से `public abstract` होता है।

```
interface Animal {
    void eat();

    // Equivalent:
    // public abstract void eat();
}
```

Implementing class को इस method की implementation देनी होती है, जब तक class स्वयं abstract न हो।

### 3. Interface Variables — Constants

Interface के variables implicit रूप से `public static final` होते हैं।

```
interface Animal {
    int MAX_AGE = 150;
}

public class Main {
    public static void main(String[] args) {
        System.out.println(Animal.MAX_AGE);
    }
}
```

Output:

```
150
```

इसका मतलब:

- `public` — accessible जहाँ अनुमति हो।
- `static` — interface के नाम से access कर सकते हैं।
- `final` — value दोबारा assign नहीं कर सकते।

```
System.out.println(Animal.MAX_AGE); // Correct

// Animal.MAX_AGE = 200; // Compile-time error
```

## Topic 4. Multiple Inheritance Through Interfaces

Java में एक class केवल एक direct class को `extends` कर सकती है, लेकिन कई interfaces को `implements` कर सकती है।

```
interface Phone {
    void call();
}

interface Camera {
    void takePhoto();
}

interface MusicPlayer {
    void playMusic();
}

class SmartPhone implements Phone, Camera, MusicPlayer {
    @Override
    public void call() {
        System.out.println("Calling...");
    }

    @Override
    public void takePhoto() {
        System.out.println("Photo taken");
    }

    @Override
    public void playMusic() {
        System.out.println("Music playing");
    }
}

public class Main {
    public static void main(String[] args) {
        SmartPhone phone = new SmartPhone();

        phone.call();
        phone.takePhoto();
        phone.playMusic();
    }
}
```

Syntax याद रखो:

```
class SmartPhone extends Device implements Camera, MusicPlayer {
}
```

यहाँ:

- `extends Device` — एक parent class।
- `implements Camera, MusicPlayer` — multiple interfaces।
- `extends` पहले और `implements` बाद में आता है।

## Topic 5. Static Method in Interface

Interface में `static` method की अपनी implementation होती है। इसे interface के नाम से call करते हैं।

```
interface Animal {
    static void info() {
        System.out.println("Animal interface");
    }
}

class Dog implements Animal {
}

public class Main {
    public static void main(String[] args) {
        Animal.info();
    }
}
```

Output:

```
Animal interface
```

Important:

```
Animal.info(); // Correct

// Dog.info(); // Compile-time error
```

Interface के static methods implementing class को सामान्य inheritance के जरिए नहीं मिलते। इसलिए इन्हें interface के नाम से call करें।

## Topic 6. Default Methods in Interface

Java 8 से interface में `default` methods लिख सकते हैं। इनमें method body होती है, इसलिए implementing class को हमेशा इनकी implementation देना जरूरी नहीं है।

### 6.1 Default Method Without Parameters

```
interface Animal {
    default void run() {
        System.out.println("Animal is running");
    }
}

class Dog implements Animal {
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.run();
    }
}
```

Output:

```
Animal is running
```

### 6.2 Default Method With Parameters

```
interface Calculator {
    default void add(int a, int b) {
        System.out.println(a + b);
    }
}

class MyCalculator implements Calculator {
}

public class Main {
    public static void main(String[] args) {
        MyCalculator calculator = new MyCalculator();
        calculator.add(10, 20);
    }
}
```

Output:

```
30
```

### 6.3 Default Method Overriding

Implementing class अपनी जरूरत के अनुसार default method को override कर सकती है।

```
interface Animal {
    default void run() {
        System.out.println("Animal is running");
    }
}

class Dog implements Animal {
    @Override
    public void run() {
        System.out.println("Dog is running");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.run();
    }
}
```

Output:

```
Dog is running
```

### 6.4 Same Default Method Conflict

अगर दो interfaces में समान signature वाला default method हो, तो implementing class को उसे override करना होगा।

```
interface Animal {
    default void show() {
        System.out.println("Animal show");
    }
}

interface Pet {
    default void show() {
        System.out.println("Pet show");
    }
}

class Dog implements Animal, Pet {
    @Override
    public void show() {
        Animal.super.show();
        Pet.super.show();
        System.out.println("Dog show");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog();
        dog.show();
    }
}
```

Output:

```
Animal show
Pet show
Dog show
```

`Animal.super.show()` और `Pet.super.show()` संबंधित interfaces के default methods को call करते हैं।

## Topic 7. Abstract Class vs Interface

### 7.1 Abstract Class क्या है?

Abstract class को `abstract` keyword से बनाते हैं। इसका object सीधे नहीं बनाया जा सकता। इसमें abstract methods और normal methods दोनों हो सकते हैं।

```
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
    }

    abstract void eat();

    void sleep() {
        System.out.println(name + " is sleeping");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
    }

    @Override
    void eat() {
        System.out.println(name + " is eating");
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Tommy");

        dog.eat();
        dog.sleep();
    }
}
```

Output:

```
Tommy is eating
Tommy is sleeping
```

### 7.2 Constructor और Instance Variable

Abstract class में constructor और instance variables हो सकते हैं।

```
abstract class Animal {
    String name;

    Animal(String name) {
        this.name = name;
        System.out.println("Animal constructor called");
    }
}

class Dog extends Animal {
    Dog(String name) {
        super(name);
        System.out.println("Dog constructor called");
    }

    void display() {
        System.out.println("Dog name: " + name);
    }
}

public class Main {
    public static void main(String[] args) {
        Dog dog = new Dog("Tommy");
        dog.display();
    }
}
```

Output:

```
Animal constructor called
Dog constructor called
Dog name: Tommy
```

समझो:

- `super(name)` parent class के constructor को call करता है।
- Parent constructor पहले execute होता है।
- Interface में constructor नहीं होता।
- Interface के fields implicit रूप से `public static final` होते हैं।

### 7.3 Abstract Class + Multiple Interfaces

```
abstract class Device {
    String brand;

    Device(String brand) {
        this.brand = brand;
    }

    void showBrand() {
        System.out.println("Brand: " + brand);
    }
}

interface Camera {
    void takePhoto();
}

interface MusicPlayer {
    void playMusic();
}

class SmartPhone extends Device implements Camera, MusicPlayer {
    SmartPhone(String brand) {
        super(brand);
    }

    @Override
    public void takePhoto() {
        System.out.println("Photo taken");
    }

    @Override
    public void playMusic() {
        System.out.println("Music playing");
    }
}

public class Main {
    public static void main(String[] args) {
        SmartPhone phone = new SmartPhone("Samsung");

        phone.showBrand();
        phone.takePhoto();
        phone.playMusic();
    }
}
```

Output:

```
Brand: Samsung
Photo taken
Music playing
```

### 7.4 Comparison Table

| Feature             | Abstract Class           | Interface                                 |
| ------------------- | ------------------------ | ----------------------------------------- |
| Keyword             | `abstract class`         | `interface`                               |
| Object directly     | नहीं बना सकते            | नहीं बना सकते                             |
| Constructor         | हाँ                      | नहीं                                      |
| Instance variables  | हाँ                      | सामान्यतः नहीं; fields constants होते हैं |
| Abstract methods    | हाँ                      | हाँ                                       |
| Implemented methods | हाँ                      | `default`, `static` आदि                   |
| Inheritance         | एक class extend          | Multiple interfaces implement             |
| Use                 | Shared state और behavior | Contract / capability                     |

याद रखने की लाइन: Abstract class shared state और implementation के लिए; interface contract और अलग-अलग capabilities के लिए।

## Topic 8. `main()` Method Inside Interface

Interface के अंदर भी `static main()` method लिख सकते हैं। अगर interface में valid `main()` method है, तो उसे सीधे launch किया जा सकता है।

File name: `Animal.java`

```
interface Animal {
    void eat();

    static void main(String[] args) {
        System.out.println("Main method inside Interface");

        Dog dog = new Dog();
        dog.eat();
    }
}

class Dog implements Animal {
    @Override
    public void eat() {
        System.out.println("Dog is eating");
    }
}
```

Run:

```
javac Animal.java
java Animal
```

Output:

```
Main method inside Interface
Dog is eating
```

`main()` static है, इसलिए इसे चलाने के लिए `Animal` का object बनाने की जरूरत नहीं है। `Dog` का object उसके instance method `eat()` को call करने के लिए बनाया जाता है।

## Topic 9. Inner Classes

Inner class वह class है जो दूसरी class के अंदर define होती है। Java में member inner, static nested, local और anonymous classes जैसे प्रकार होते हैं।

### 9.1 Member Inner Class

Non-static class को दूसरी class के अंदर define करने पर वह member inner class कहलाती है।

```
class Car {
    private String model;
    private boolean isEngineOn = false;

    Car(String model) {
        this.model = model;
    }

    class Engine {
        void start() {
            if (!isEngineOn) {
                isEngineOn = true;
                System.out.println(model + " engine started");
            } else {
                System.out.println(model + " engine is already on");
            }
        }

        void stop() {
            if (isEngineOn) {
                isEngineOn = false;
                System.out.println(model + " engine stopped");
            } else {
                System.out.println(model + " engine is already off");
            }
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car("Tata");

        Car.Engine engine = car.new Engine();

        engine.start();
        engine.start();
        engine.stop();
    }
}
```

Output:

```
Tata engine started
Tata engine is already on
Tata engine stopped
```

Important:

- `Engine` non-static class है।
- यह outer class के private members को access कर सकती है।
- इसका object बनाने के लिए `Car` का object चाहिए।

```
Car.Engine engine = car.new Engine();
```

### 9.2 Static Nested Class

दूसरी class के अंदर `static` keyword के साथ बनाई गई class को static nested class कहते हैं।

```
class Car {
    static class Engine {
        void start() {
            System.out.println("Engine started");
        }
    }
}

public class Main {
    public static void main(String[] args) {
        Car.Engine engine = new Car.Engine();
        engine.start();
    }
}
```

Output:

```
Engine started
```

Important:

- Outer class का object जरूरी नहीं है।
- इसका object `new Car.Engine()` से बनता है।
- यह outer class के non-static members को सीधे access नहीं कर सकती; उनके लिए outer object चाहिए।

### 9.3 Local Inner Class

Method के अंदर define की गई class को local inner class कहते हैं।

```
class Car {
    void startCar() {
        class Engine {
            void start() {
                System.out.println("Car engine started");
            }
        }

        Engine engine = new Engine();
        engine.start();
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.startCar();
    }
}
```

Output:

```
Car engine started
```

`Engine` class केवल `startCar()` method के scope में उपलब्ध है।

### 9.4 Anonymous Inner Class

Anonymous inner class एक ऐसी class है जिसका explicit नाम नहीं होता। इसका उपयोग अक्सर किसी abstract class या interface की implementation तुरंत देने के लिए किया जाता है।

```
abstract class Engine {
    abstract void start();
}

class Car {
    void startCar() {
        Engine engine = new Engine() {
            @Override
            void start() {
                System.out.println("Car engine started");
            }
        };

        engine.start();
    }
}

public class Main {
    public static void main(String[] args) {
        Car car = new Car();
        car.startCar();
    }
}
```

Output:

```
Car engine started
```

Working:

1. `Engine` abstract class है।
2. `new Engine() { ... }` anonymous subclass define और instantiate करता है।
3. उस subclass में `start()` implement होता है।
4. `engine.start()` implementation को call करता है।

## Topic 10. Inner Classes — Quick Comparison

| Type            | कहाँ define होती है?        | Object बनाने का तरीका             |
| --------------- | --------------------------- | --------------------------------- |
| Member Inner    | Class के अंदर, non-static   | `car.new Engine()`                |
| Static Nested   | Class के अंदर, `static`     | `new Car.Engine()`                |
| Local Inner     | Method के अंदर              | उसी method के अंदर `new Engine()` |
| Anonymous Inner | बिना explicit class name के | `new Engine() { ... }`            |

## Final Do-or-Die Revision Map

1\. Composition vs Inheritance

Composition = has-a · Inheritance = is-a · `extends` और objects का सही use।

2\. SmartPhone Practical

Phone, Camera और MusicPlayer की functionality को Composition से use करना।

3\. Interfaces

Abstract methods, `public static final` constants, multiple interfaces, static methods, default methods, overriding और conflicts।

4\. Abstract Class vs Interface

Constructors, instance variables, `super()`, एक class inheritance और multiple interfaces।

5\. Inner Classes

Member Inner, Static Nested, Local Inner और Anonymous Inner Class।

## सभी Java programs कैसे run करें?

हर code example को अलग `Main.java` file में रखकर run करें, जब तक example में `Animal.java` जैसा अलग filename न दिया गया हो।

```
javac Main.java
java Main
```

Interface वाले उदाहरण के लिए:

```
javac Animal.java
java Animal
```

ध्यान रखें: एक ही file में सभी examples को एक साथ paste न करें, क्योंकि कई examples में अलग-अलग `Main` classes हैं।

Final memory line: `extends` से inheritance, `implements` से interface contract, Composition से दूसरे objects की functionality और Inner Class से एक class के अंदर class define होती है।
