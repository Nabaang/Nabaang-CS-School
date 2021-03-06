# Thread safety

wiki μ°Έκ³ 

<aside>
π‘ λ©ν° μ€λ λ νλ‘κ·Έλλ°μμ μΌλ°μ μΌλ‘ μ΄λ€ ν¨μλ λ³μ, νΉμ κ°μ²΄κ° μ¬λ¬ μ€λ λλ‘ λΆν° λμμ μ κ·Όμ΄ μ΄λ£¨μ΄μ Έλ νλ‘κ·Έλ¨μ μ€νμ λ¬Έμ κ° μμμ λ»νλ€.
μ¦, νλμ ν¨μκ° ν μ€λ λλ‘λΆν° νΈμΆλμ΄ μ€ν μ€μΌ λ, λ€λ₯Έ μ€λ λκ° κ·Έ ν¨μλ₯Ό νΈμΆνμ¬ λμμ ν¨κ» μ€ν λλλΌλ κ° μ€λ λμμμ ν¨μμ μν κ²°κ³Όκ° μ¬λ°λ‘ λμ€λ κ²μΌλ‘ μ μ

</aside>

![Untitled](./6.png)

```python
a = 3
b = 2
print(a + b)
```

μ΄μ κ°μ΄ κ°λ¨ν μ°μ°μ κ²½μ°, μ¬λ¬ μ€λ λκ° λμμ μ²λ¦¬νλλΌλ thread-safety νκ² μ²λ¦¬ λ  μ μμ.

```python
d[key] + value
d[key] = value
```

μμ μ½λλ thread-safety νμ§ μμ. μ‘°νμ λμ μ¬μ΄μ μ€λ λ μ νμ΄ μΌμ΄λ  μ μκΈ° λλ¬Έ.

μ€ν μμ­

# Thread-safety μ§ν€κΈ° μν λ°©λ²

1. Re-entrancy

   - μ΄λ€ ν¨μκ° ν μ€λ λμ μν΄ νΈμΆλμ΄ μ€ν μ€μΌ λ, λ€λ₯Έ μ€λ λκ° κ·Έ ν¨μλ₯Ό νΈμΆνλλΌλ κ·Έ κ²°κ³Όκ° κ°κ°μκ² μ¬λ°λ‘ μ£Όμ΄μ ΈμΌ ν¨.
   - μ€λ λλΌλ¦¬ λλ¦½μ μΌ μ μκ² μ½λλ₯Ό μ‘°μ¬ν μ§λΌ!

2. Thread-local storage

   - κ³΅μ  μμμ μ¬μ©μ μ΅λν μ€μ¬ κ°κ°μ μ€λ λμμλ§ μ κ·Ό κ°λ₯ν μ μ₯μ λ€μ μ¬μ©ν¨μΌλ‘μ¨ λμ μ κ·Όμ λ§μ.
   - μ΄ λ°©μμ λκΈ°ν λ°©λ²κ³Ό κ΄λ ¨λμ΄ μκ³ , λν κ³΅μ  μνλ₯Ό νΌν  μ μμλ μ¬μ©νλ λ°©μ
   - κΈλ‘λ³ λ³μ λ±μ μ¬μ©μ μ§μν΄λΌ!

3. Mutual exclusion

   - κ³΅μ  μμμ κΌ­ μ¬μ©ν΄μΌ ν  κ²½μ° ν΄λΉ μμμ μ κ·Όμ μΈλ§ν¬μ΄, lock λ±μΌλ‘ ν΅μ 

4. Atomic operations

   - κ³΅μ  μμμ μ κ·Όν  λ μμ μ°μ°μ μ΄μ©νκ±°λ βμμμ βμΌλ‘ μ μλ μ κ·Ό λ°©λ²μ μ¬μ©ν¨μΌλ‘ μ¨ μνΈ λ°°μ λ₯Ό κ΅¬νν  μ μμ.
     - `+=` μ°μ°μ κ²½μ° ν μ€μ μ½λλλΌλ `+` μ°μ° νμ `=` μ°μ°μ νκΈ° λλ¬Έμ μμ μ μ΄λΌκ³  νκΈ° μ΄λ €μ.(virtual instruction μμ€μμ λ³΄λλΌλ μμμ μ΄λΌκ³  λ³΄κΈ° μ΄λ €μ.)
   - atomic νλ€λ κ²μ λ©ν° μ€λ λ νκ²½μμ λ°μ΄ν°κ° λ°λμ λ³κ²½ μ κ³Ό νμ μν©μμλ§ μ κ·Όνλ κ²μ λ³΄μ₯. μ¦, λ°μ΄ν°μ λ³κ²½μ΄ μ΄λ£¨μ΄μ§κ³  μλ μκ°μλ μ κ·Όμ΄ λΆκ°λ₯.

5. Immutable Object

   κ°μ²΄ μμ± μ΄νμ κ°μ λ³κ²½ ν  μ μλλ‘ λ§λ¬.

## Thread-safety Data Types

java λΌμ΄λΈλ¬λ¦¬μλ λͺλͺ Thread-safeμΈ λ°μ΄ν° μ νμ΄ μμ.

> [StringBuffer is]Β AΒ thread-safe, mutable sequence of characters. A string buffer is like a String, but can be modified. At any point in time it contains some particular sequence of characters, but the length and content of the sequence can be changed through certain method calls.
>
>
> String buffers areΒ **safe for use by multiple threads.**Β The methods areΒ **synchronized**Β where necessary so that all the operations on any particular instance behave as if they occur in some serial order that is consistent with the order of the method calls made by each of the individual threads involved.

> [StringBuilder is]Β A mutable sequence of characters. This class provides an API compatible with StringBuffer, butΒ with no guarantee of synchronization. This class is designed for use as a drop-in replacement for StringBuffer in places where the string buffer was beingΒ used by a single threadΒ (as is generally the case). Where possible, it is recommended that this class be used in preference to StringBuffer as it will be faster under most implementations.

### μμ½νμλ©΄,

`StringBuffer`λ λ©ν° μ€λ λ νκ²½μμ μ¬μ©νκΈ° μ’μΌλ©°, λκΈ°νκ° λκΈ° λλ¬Έμ thread-safeνλ€. κ·Έλ¬λ `StringBuilder` λ μ±κΈ μ€λ λ νκ²½μμ μ¬μ©λ  μ μμΌλ©° λκΈ°νκ° μ΄λ£¨μ΄μ§μ§ μλλ€. 

μ¬κΈ°μ Thread-safe Data Typeμ `StringBuffer` λΌκ³  ν  μ μκ² λ€.

`Vector`μΒ `Hashtable`μ μ μΈν Java Collection Interface λλΆλΆμ μ±κΈ μ€λ λ νκ²½μμ μ¬μ©ν  μ μλλ‘ μ€κ³λμ΄μμ΄Β `List`,Β `Set`,Β `Map`,Β `ArrayList`,Β `HashSet`,Β `HashMap`Β λ±μ thread-safeνμ§ μλ€.

λ©ν° μ€λ λ© νκ²½μ μν΄ μλ°μμλΒ `Collections.synchronizedXXX()`Β λ©μλλ₯Ό μ κ³΅νλ€.

```java
List<String> list = Collections.synchronizedList(new ArrayList<String>()); 
private static Map<Integer,Boolean> cache 
								= Collections.synchronizedMap(new HashMap<>());
```

- μμ κ°μ΄ μ μΈνλ©΄ thread-safeν λ¦¬μ€νΈλ₯Ό λ§λ€ μ μμ.

- λκΈ°νλ Collectionμ μ€λ λ μμ μ lockμ΄ κ±Έλ¦¬κΈ° λλ¬Έμ μ€λ λκ° λ³λ ¬μ μΌλ‘ μμλ€μ μ²λ¦¬ ν  μ μμ.

- μ΄ λΆλΆ κ°μ νκΈ° μν΄ `java.util.concurrent` ν¨ν€μ§μμ λΆλΆ lockμ μ¬μ©νλ ConcurrentXXX κ° μ κ³΅λ¨.

  ```java
  Map<K,V> map = new ConcurrentHashMap<K,V>(); 
  Queue<E> queue = new ConcurrentQueue<E>();
  ```

  - `ConcurrentHashMap`μ μ½κΈ° μμμλ μ¬λ¬ μ€λ λκ° λμμ μ κ·Όν  μ μμ§λ§, μ°κΈ° μμμλ νΉμ  μΈκ·Έλ¨ΌνΈ or λ²ν·μ λν lockμ μ¬μ©ν¨.
    - λ°μ΄ν°λ₯Ό μ μ ν μΈκ·Έλ¨ΌνΈλ‘ λλμ΄ lockμ ν λΉ.
    - κ°μ μΈκ·Έλ¨ΌνΈλ§ μλλΌλ©΄ lockμ κΈ°λ€λ¦΄ νμκ° μμ.
    - μ¬λ¬ μ€λ λμμ `ConcurrentHashMap` κ°μ²΄μ λμμ λ°μ΄ν°λ₯Ό μ½μ, μ°Έμ‘°νλλΌλ κ·Έ λ°μ΄ν°κ° λ€λ₯Έ μΈκ·Έλ¨ΌνΈμ μμΉνλ©΄ μλ‘ lockμ μ»κΈ° μν΄ κ²½μνμ§ μμ.

[[μ΄μμ²΄μ ] μ€λ λ μμ  : Thread-safety (C++κ³Ό JAVA)](https://eun-jeong.tistory.com/21)

[Reading 20: Thread Safety](http://web.mit.edu/6.005/www/fa15/classes/20-thread-safety/)

[HashMap, Hashtable, ConcurrentHashMap λκΈ°ν μ²λ¦¬ λ°©μ](https://tomining.tistory.com/169)