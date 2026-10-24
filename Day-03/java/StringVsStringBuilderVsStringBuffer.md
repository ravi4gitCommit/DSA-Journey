# ☕ Java String vs StringBuilder vs StringBuffer — Complete Notes

Beginner-friendly Hinglish • Corrected code • String Pool • intern() • Multithreading • Performance

## 1. String क्या है?

Java में `String` characters की sequence होती है। इसका उपयोग text store करने के लिए किया जाता है।

Examples:

```
String name = "Java";
String message = "Hello World";
String number = "12345";
```

यहाँ `"Java"`, `"Hello World"` और `"12345"` सभी Strings हैं।

### String की मुख्य बातें

1. `String` Java में एक class है।
2. String characters की sequence represent करती है।
3. `String` immutable होती है, यानी बनने के बाद उसके content को modify नहीं किया जा सकता।
4. String literals को Java String Pool में रखा जा सकता है।
5. `String` को `concat()`, `toUpperCase()` और `replace()` जैसे methods से process कर सकते हैं, लेकिन ये methods मूल String को modify नहीं करते।

### String memory में कहाँ store होती है?

यह समझना जरूरी है कि String object और उसके reference में अंतर होता है।

```
String str = "Hello";
```

- `str` एक reference variable है।
- `"Hello"` String object को represent करता है।
- Reference variable local variable होने पर सामान्यतः stack frame में होता है।
- String object heap memory में होता है।
- यदि String literal है, तो JVM String Pool का उपयोग कर सकती है।

ध्यान दें: String Pool कोई अलग memory area नहीं है; यह heap में मौजूद Strings के लिए एक विशेष pool है।

## 2. String Immutable क्यों होती है?

Immutable का मतलब है कि object बनने के बाद उसका content बदल नहीं सकता।

Example:

```
public class Test {
    public static void main(String[] args) {
        String str1 = "Hello";

        str1.concat(" World");

        System.out.println(str1);
    }
}
```

Output:

```
Hello
```

### ऐसा क्यों हुआ?

`concat()` एक नई String बनाता है, यदि concatenation का result अलग content वाला है। यह original String `"Hello"` को modify नहीं करता।

हमने नई String को किसी variable में store नहीं किया, इसलिए उसे यहाँ access नहीं कर रहे हैं।

सही तरीका:

```
public class Test {
    public static void main(String[] args) {
        String str1 = "Hello";

        String str2 = str1.concat(" World");

        System.out.println(str1);
        System.out.println(str2);
    }
}
```

Output:

```
Hello
Hello World
```

याद रखो:

- `str1` → original String `"Hello"`
- `str2` → नई String `"Hello World"`

एक ही `main()` method में दोनों examples चलाने हों, तो variables के नाम अलग रखो या अलग-अलग examples में run करो। एक scope में `str1` को दोबारा declare नहीं कर सकते।

## 3. String vs StringBuilder vs StringBuffer

इन तीनों का उपयोग characters या text के साथ काम करने के लिए होता है, लेकिन इनका behaviour अलग है।

| Feature              | String                                        | StringBuilder                       | StringBuffer                                         |
| -------------------- | --------------------------------------------- | ----------------------------------- | ---------------------------------------------------- |
| Mutability           | Immutable                                     | Mutable                             | Mutable                                              |
| Content बदल सकता है? | नहीं; नया result बनता है                      | हाँ                                 | हाँ                                                  |
| Thread-safe          | Immutable होने से content में बदलाव नहीं होता | नहीं                                | हाँ, इसके synchronized methods के माध्यम से          |
| Synchronization      | Content mutation नहीं                         | नहीं                                | हाँ, मुख्यतः methods पर                              |
| सामान्य performance  | बार-बार जोड़ने पर inefficient हो सकती है      | Repeated modifications के लिए अच्छा | Synchronization के कारण अतिरिक्त overhead हो सकता है |
| Best use             | Fixed text                                    | Single-threaded modifications       | Shared mutable text, जहाँ synchronization चाहिए      |

### एक आसान example

```
String str = "Java";
str.concat(" Programming");

StringBuilder sb = new StringBuilder("Java");
sb.append(" Programming");

StringBuffer sbf = new StringBuffer("Java");
sbf.append(" Programming");
```

इन statements के बाद:

- `str` की value `"Java"` ही है।
- `sb` का content `"Java Programming"` है।
- `sbf` का content `"Java Programming"` है।

याद रखने की लाइन:

- `String` → Immutable
- `StringBuilder` → Mutable, generally faster, not thread-safe
- `StringBuffer` → Mutable, synchronized methods

# 4. StringBuilder — Complete Notes

## 4.1 StringBuilder क्या है?

`StringBuilder` Java की एक mutable character sequence class है। इसका मतलब है कि एक बार object बनने के बाद उसके content में बदलाव किया जा सकता है।

यह तब उपयोगी है जब loop में बार-बार String जोड़नी हो।

Example:

```
StringBuilder sb = new StringBuilder();

sb.append("Hello");
sb.append(" ");
sb.append("Java");

System.out.println(sb);
```

Output:

```
Hello Java
```

यहाँ एक ही `StringBuilder` object का content बदलता है। हर बार नया `StringBuilder` बनाने की जरूरत नहीं पड़ती।

## 4.2 StringBuilder के सभी जरूरी methods

नीचे तुम्हारे सभी methods रखे गए हैं और उनके working examples दिए गए हैं।

### 1. `append()`

Existing content के अंत में text जोड़ता है।

```
StringBuilder sb = new StringBuilder("Hello");
sb.append(" Java");

System.out.println(sb);
```

Output:

```
Hello Java
```

### 2. `insert()`

दिए गए index पर text insert करता है।

```
StringBuilder sb = new StringBuilder("Hello");
sb.insert(1, "Java");

System.out.println(sb);
```

Output:

```
HJavaello
```

Index `1` पर insertion हुआ, इसलिए original characters आगे shift हो गए।

### 3. `replace()`

एक range के characters को दूसरे text से replace करता है।

```
StringBuilder sb = new StringBuilder("Hello");
sb.replace(1, 3, "XY");

System.out.println(sb);
```

Output:

```
HXYlo
```

Important: `replace(start, end, str)` में `start` inclusive और `end` exclusive होता है।

इसलिए index `1` और `2` के characters replace हुए; index `3` वाला character शामिल नहीं हुआ।

### 4. `delete()`

दिए गए range के characters delete करता है।

```
StringBuilder sb = new StringBuilder("Hello");
sb.delete(1, 3);

System.out.println(sb);
```

Output:

```
Hlo
```

Index `1` और `2` के characters delete हुए।

### 5. `reverse()`

Characters का order उल्टा करता है।

```
StringBuilder sb = new StringBuilder("Java");
sb.reverse();

System.out.println(sb);
```

Output:

```
avaJ
```

### 6. `setCharAt()`

किसी specific index के character को बदलता है।

```
StringBuilder sb = new StringBuilder("Hello");
sb.setCharAt(0, 'h');

System.out.println(sb);
```

Output:

```
hello
```

ध्यान दें: Character के लिए single quotes `'h'` उपयोग होते हैं, जबकि String के लिए double quotes `"hello"`।

### 7. `trimToSize()`

`StringBuilder` की internal capacity को उसकी current length के अनुसार कम करने का प्रयास करता है।

```
StringBuilder sb = new StringBuilder(100);
sb.append("Java");

System.out.println(sb.length());

sb.trimToSize();

System.out.println(sb.length());
```

Output:

```
4
4
```

`trimToSize()` capacity से संबंधित है, length से नहीं। इसलिए दोनों outputs `4` हैं।

### 8. `length()`

वर्तमान character sequence में कितने characters हैं, यह बताता है।

```
StringBuilder sb = new StringBuilder("Java");

System.out.println(sb.length());
```

Output:

```
4
```

### 9. `substring()`

दिए गए range के characters की एक नई `String` return करता है। यह मूल `StringBuilder` को modify नहीं करता।

```
StringBuilder sb = new StringBuilder("Hello");

String result = sb.substring(1, 3);

System.out.println(result);
System.out.println(sb);
```

Output:

```
el
Hello
```

यहाँ भी `start` inclusive और `end` exclusive है।

## 4.3 Method Chaining क्या है?

जब एक method का returned object तुरंत अगले method पर call किया जाता है, तो इसे method chaining कहते हैं।

तुम्हारा original example:

```
sb.append("2").replace(1, 2, "ok").reverse();
```

इसे सही तरह समझने के लिए:

```
public class Test {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();

        sb.append("2")
          .replace(1, 2, "ok")
          .reverse();

        System.out.println(sb);
    }
}
```

Output:

```
2
```

### Step-by-step working

1. `append("2")` → `"2"`
2. `replace(1, 2, "ok")` → कोई बदलाव नहीं, क्योंकि length `1` है और index `1` पर कोई character नहीं है। Empty range पर `"ok"` insert हो जाता है, इसलिए content `"2ok"` बनता है।
3. `reverse()` → `"ko2"`

सही output वास्तव में:

```
ko2
```

यहाँ हर method `StringBuilder` object को return करता है, जिससे अगला method उसी object पर call हो सकता है।

### तुम्हारे सभी methods का combined example

```
public class Test {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();

        sb.append("Hello");
        sb.insert(1, "Java");
        sb.replace(1, 3, "World");
        sb.delete(1, 3);
        sb.reverse();
        sb.setCharAt(0, 'h');
        sb.trimToSize();

        System.out.println("Final text: " + sb);
        System.out.println("Length: " + sb.length());

        String part = sb.substring(1, 3);
        System.out.println("Substring: " + part);
    }
}
```

यह code सभी methods को एक ही runnable example में दिखाता है। अलग-अलग methods को सीखने के लिए ऊपर वाले individual examples अधिक आसान हैं।

# 5. StringBuffer — Complete Notes

## 5.1 StringBuffer क्या है?

`StringBuffer` एक mutable character sequence class है। इसका content modify किया जा सकता है और इसके मुख्य methods synchronized होते हैं।

इसलिए multiple threads जब एक ही `StringBuffer` object पर methods call करते हैं, तो उन method calls के बीच synchronization मिलता है।

Example:

```
public class Test {
    public static void main(String[] args) {
        StringBuffer sbf = new StringBuffer("Java");

        sbf.append(" Programming");

        System.out.println(sbf);
    }
}
```

Output:

```
Java Programming
```

`StringBuffer` में भी `append()`, `insert()`, `replace()`, `delete()`, `reverse()`, `setCharAt()`, `length()` और `substring()` जैसे methods होते हैं।

## 5.2 Thread-safe का क्या मतलब है?

Thread-safe का मतलब है कि concurrent access की स्थिति में class या operation shared data को निर्धारित तरीके से सुरक्षित रख सके।

`StringBuffer` के synchronized methods एक समय में उसी object के संबंधित synchronized method execution को दूसरे thread से रोकते हैं।

लेकिन ध्यान दें: Thread-safe methods होने का मतलब यह नहीं कि किसी भी multi-step operation का पूरा sequence automatically atomic हो जाएगा। कई methods के बीच साझा logic हो, तो अतिरिक्त synchronization की जरूरत पड़ सकती है।

# 6. Multithreading — StringBuffer Example

तुम्हारे code में `sb` और `sbf` के नाम mix हो गए थे। `task` class को भी `StringBuffer` वाला object मिलना चाहिए। नीचे पूरा corrected program है।

```
class Task extends Thread {
    private final StringBuffer sb;

    public Task(StringBuffer sb) {
        this.sb = sb;
    }

    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            sb.append("a");
        }
    }
}

public class Test {
    public static void main(String[] args)
            throws InterruptedException {

        StringBuffer sb = new StringBuffer();

        Task t1 = new Task(sb);
        Task t2 = new Task(sb);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Final length: " + sb.length());
    }
}
```

Output:

```
Final length: 2000
```

### Working

1. `StringBuffer sb` एक shared object है।
2. `t1` और `t2` दोनों उसी object को access करते हैं।
3. हर thread loop को 1000 बार चलाता है।
4. हर iteration में `"a"` append होता है।
5. `start()` दोनों threads को execution के लिए शुरू करता है।
6. `join()` main thread को दोनों threads complete होने तक wait कराता है।
7. उसके बाद length `2000` होती है।

यहाँ `join()` क्यों जरूरी है?

अगर `join()` नहीं होता, तो main thread दोनों workers के complete होने से पहले length print कर सकता था। तब output `2000` से कम भी हो सकता था।

# 7. Multithreading — StringBuilder Example

अब उसी program में `StringBuffer` की जगह `StringBuilder` का उपयोग करते हैं।

```
class Task extends Thread {
    private final StringBuilder sb;

    public Task(StringBuilder sb) {
        this.sb = sb;
    }

    @Override
    public void run() {
        for (int i = 0; i < 1000; i++) {
            sb.append("a");
        }
    }
}

public class Test {
    public static void main(String[] args)
            throws InterruptedException {

        StringBuilder sb = new StringBuilder();

        Task t1 = new Task(sb);
        Task t2 = new Task(sb);

        t1.start();
        t2.start();

        t1.join();
        t2.join();

        System.out.println("Final length: " + sb.length());
    }
}
```

### Expected vs guaranteed output

इस code में final length `2000` guaranteed नहीं है।

कारण:

- दोनों threads एक ही mutable object modify कर रहे हैं।
- `StringBuilder` के methods synchronized नहीं हैं।
- Concurrent modifications के कारण updates lost हो सकते हैं या internal state में inconsistency हो सकती है।

कभी-कभी यह program `2000` print कर सकता है, लेकिन इससे यह साबित नहीं होता कि code thread-safe है।

Correct practice: अगर shared mutable character sequence को multiple threads से modify करना है, तो `StringBuffer` या external synchronization का उपयोग करो। यदि केवल एक thread modify कर रहा है, तो सामान्यतः `StringBuilder` बेहतर विकल्प है।

# 8. StringBuilder vs StringBuffer — Performance Benchmark

तुम्हारे notes में `System.nanoTime()` वाला benchmark था। इसमें variables की declarations, loops और averages का calculation अधूरा था। नीचे complete code है।

इस benchmark में दोनों classes को same starting content और same number of append operations दिए गए हैं।

```
public class Test {
    public static void main(String[] args) {
        int iterations = 1_000_000;
        int repetitions = 10;

        long totalSBTime = 0;
        long totalSBFTime = 0;

        // Warm-up: JVM को code execute करने का मौका देना
        for (int i = 0; i < 3; i++) {
            runBuilder(iterations / 10);
            runBuffer(iterations / 10);
        }

        // StringBuilder benchmark
        for (int i = 0; i < repetitions; i++) {
            long startTime = System.nanoTime();

            runBuilder(iterations);

            long endTime = System.nanoTime();
            totalSBTime += (endTime - startTime);
        }

        // StringBuffer benchmark
        for (int i = 0; i < repetitions; i++) {
            long startTime = System.nanoTime();

            runBuffer(iterations);

            long endTime = System.nanoTime();
            totalSBFTime += (endTime - startTime);
        }

        System.out.println(
            "Average StringBuilder time: "
            + (totalSBTime / repetitions) + " ns"
        );

        System.out.println(
            "Average StringBuffer time: "
            + (totalSBFTime / repetitions) + " ns"
        );
    }

    static void runBuilder(int iterations) {
        StringBuilder sb = new StringBuilder("java");

        for (int i = 0; i < iterations; i++) {
            sb.append("programming");
        }
    }

    static void runBuffer(int iterations) {
        StringBuffer sb = new StringBuffer("java");

        for (int i = 0; i < iterations; i++) {
            sb.append("programming");
        }
    }
}
```

### Code का explanation

- `System.nanoTime()` elapsed time मापने के लिए इस्तेमाल होता है।
- `startTime` operation शुरू होने का समय रखता है।
- `endTime` operation पूरा होने का समय रखता है।
- `endTime - startTime` elapsed nanoseconds देता है।
- `totalSBTime` दस StringBuilder runs का total रखता है।
- `totalSBFTime` दस StringBuffer runs का total रखता है।
- Total को `repetitions` से divide करने पर average time मिलता है।

### कौन तेज है?

आमतौर पर single-threaded append operations में `StringBuilder` तेज हो सकता है, क्योंकि इसमें synchronization overhead नहीं होता। लेकिन वास्तविक परिणाम JVM, hardware, warm-up, garbage collection और workload पर निर्भर करते हैं।

यह छोटा benchmark learning के लिए है, formal performance measurement के लिए नहीं। अधिक reliable benchmarking के लिए Java Microbenchmark Harness (JMH) का उपयोग किया जाता है।

# 9. String Pool — Complete Notes

## 9.1 String Pool क्या है?

String Pool, JVM heap में मौजूद एक विशेष mechanism है जो समान String literals को reuse करने में मदद करता है।

इससे duplicate String objects बनाने की जरूरत कम हो सकती है और memory बच सकती है।

Example:

```
public class Test {
    public static void main(String[] args) {
        String s1 = "Java";
        String s2 = "Java";

        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

Output:

```
true
true
```

### ऐसा क्यों हुआ?

```
String s1 = "Java";
String s2 = "Java";
```

दोनों literals का content एक जैसा है। JVM String Pool में मौजूद pooled String object को reuse करती है।

इसलिए `s1` और `s2` समान object को refer करते हैं।

- `==` references की identity compare करता है।
- `.equals()` String content compare करता है।

इस example में दोनों `true` हैं।

## 9.2 String Pool vs `new String()`

```
public class Test {
    public static void main(String[] args) {
        String s1 = "Java";
        String s2 = new String("Java");

        System.out.println(s1 == s2);
        System.out.println(s1.equals(s2));
    }
}
```

Output:

```
false
true
```

### ऐसा क्यों?

- `s1` pooled literal को refer करता है।
- `new String("Java")` एक अलग String object बनाता है।
- दोनों objects का content समान है, इसलिए `.equals()` `true` देता है।
- दोनों references अलग objects को refer करते हैं, इसलिए `==` `false` देता है।

Important: `new String("Java")` का उपयोग सामान्यतः अनावश्यक होता है, यदि केवल `"Java"` की String चाहिए।

## 9.3 String Pool के मुख्य फायदे

1. समान literals के objects reuse हो सकते हैं।
2. Duplicate String objects की संख्या कम हो सकती है।
3. Memory utilization बेहतर हो सकता है।
4. String literals का reuse Java को memory-efficient बनाने में मदद करता है।

## 9.4 String Pool में Strings कब जाती हैं?

String literals automatically intern किए जाते हैं।

```
String a = "Hello";
```

यह literal pooled String mechanism का उपयोग करता है।

लेकिन:

```
String b = new String("Hello");
```

यह एक अलग String object बनाता है। इसका अर्थ यह नहीं कि `"Hello"` literal pool में मौजूद नहीं है; literal स्वयं pooled हो सकता है और `new String()` अलग object बनाता है।

# 10. `intern()` Method — Complete Notes

## 10.1 `intern()` क्या है?

`intern()` Java की `String` class का method है। यह String Pool में canonical String reference देता है।

अगर उसी content की String पहले से pool में है, तो उसका pooled reference return होता है। अन्यथा उस content की canonical String को pool में जोड़ा जाता है और उसका reference return होता है।

Example:

```
public class Test {
    public static void main(String[] args) {
        String s1 = new String("Java");
        String s2 = s1.intern();

        String s3 = "Java";

        System.out.println(s1 == s2);
        System.out.println(s2 == s3);
        System.out.println(s1.equals(s2));
    }
}
```

Output:

```
false
true
true
```

### Step-by-step working

1. `s1` एक अलग String object को refer करता है।
2. `s1.intern()` pooled canonical reference return करता है।
3. `s2` उस pooled reference को store करता है।
4. `s3` उसी pooled literal `"Java"` को refer करता है।
5. इसलिए `s2 == s3` true होता है।
6. `s1 == s2` false है, क्योंकि वे अलग objects हैं।
7. `s1.equals(s2)` true है, क्योंकि दोनों का content `"Java"` है।

## 10.2 `intern()` का एक और example

```
public class Test {
    public static void main(String[] args) {
        String s1 = new String("Hello").intern();
        String s2 = "Hello";

        System.out.println(s1 == s2);
    }
}
```

Output:

```
true
```

दोनों references pooled canonical String को refer करते हैं।

## 10.3 `==`, `.equals()` और `intern()` का अंतर

| Expression      | क्या check करता है?                                    |
| --------------- | ------------------------------------------------------ |
| `s1 == s2`      | दोनों references same object को refer करते हैं या नहीं |
| `s1.equals(s2)` | दोनों Strings का content समान है या नहीं               |
| `s1.intern()`   | String Pool से canonical reference देता है             |

### एक जरूरी बात

`intern()` original String को mutate नहीं करता। यह एक reference return करता है। उस reference को variable में store करना जरूरी है, यदि आगे उपयोग करना हो।

```
String s1 = new String("Java");
String s2 = s1.intern();
```

यहाँ `s1` का reference अपने आप नहीं बदलता।

# 11. String, StringBuilder और StringBuffer की Memory

तीनों में text represent होता है, लेकिन उनके objects और references को समझना जरूरी है।

```
String str = "Java";
StringBuilder builder = new StringBuilder("Java");
StringBuffer buffer = new StringBuffer("Java");
```

सामान्य mental model:

- `str` reference → String object
- `builder` reference → StringBuilder object
- `buffer` reference → StringBuffer object

ये objects heap memory में रहते हैं। Local reference variables सामान्यतः method के stack frame में रहते हैं।

String Pool केवल String literals के लिए विशेष pooling mechanism है। `StringBuilder` और `StringBuffer` के objects String Pool में नहीं रखे जाते।

आंतरिक रूप से इन mutable classes में character data रखने के लिए storage होता है। आधुनिक Java implementations में String data और mutable builders की internal representation implementation details पर निर्भर करती है; इन्हें हमेशा सीधे `char[]` ही समझना जरूरी नहीं है।

# 12. Common Mistakes — Revision

| Mistake                                             | Correct concept                                     |
| --------------------------------------------------- | --------------------------------------------------- |
| `str.concat(" World");` के बाद `str` बदल जाएगा      | String immutable है                                 |
| `StringBuilder` thread-safe है                      | नहीं, यह synchronized नहीं है                       |
| `StringBuffer` का हर multi-step operation atomic है | जरूरी नहीं                                          |
| `==` content compare करता है                        | यह reference identity compare करता है               |
| `.equals()` references compare करता है              | String content compare करता है                      |
| `substring(1, 3)` में index 3 शामिल है              | नहीं, end index exclusive है                        |
| `trimToSize()` length बदलता है                      | यह capacity adjust करता है                          |
| `join()` String जोड़ता है                           | `Thread.join()` thread completion का इंतजार करता है |
| `intern()` original String बदल देता है              | यह canonical reference return करता है               |
| String Pool heap से अलग memory area है              | String Pool heap में होता है                        |

# 13. Final Revision Map

## याद रखने वाली मुख्य बातें

String

- Immutable
- Literal String Pool का उपयोग कर सकता है
- `concat()` का result store करना पड़ सकता है

StringBuilder

- Mutable
- Thread-safe नहीं
- Single-threaded repeated modifications के लिए अच्छा
- Methods: `append`, `insert`, `replace`, `delete`, `reverse`, `setCharAt`, `trimToSize`, `length`, `substring`

StringBuffer

- Mutable
- Synchronized methods
- Shared mutable text के लिए उपयोगी
- Synchronization का overhead हो सकता है

String Pool और `intern()`

- समान literals pooled object reuse कर सकते हैं
- `new String()` अलग object बनाता है
- `==` reference identity compare करता है
- `.equals()` content compare करता है
- `intern()` canonical pooled reference देता है

## दो दिन की study strategy

Day 1: String immutability, `StringBuilder` के सभी methods, method chaining और `StringBuffer` के basics को code run करके समझो।

Day 2: दोनों multithreading examples, performance benchmark, String Pool, `intern()` और `==` vs `.equals()` के outputs खुद predict करके check करो।

सबसे जरूरी बात: हर code को copy-paste करके run करने के बाद output देखो। केवल definition याद करने के बजाय यह समझो कि कौन-सा object बदल रहा है और कौन-सा reference उसी object को point कर रहा है।
