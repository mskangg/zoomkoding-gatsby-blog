---
emoji: π
title: μ΄νν°λΈ μλ° νΊμλ³΄κΈ° - μμ΄ν 1. μμ±μ λμ  μ μ  ν©ν°λ¦¬ λ©μλλ₯Ό κ³ λ €νλΌ
date: '2022-09-01 00:23:22'
author: mskangg
tags: java
categories: μ΄νν°λΈ μλ° νΊμλ³΄κΈ°
---

![main](main.png)

## ν΅μ¬ μ λ¦¬

### μ₯μ 

- μ΄λ¦μ κ°μ§ μ μλ€. (λμΌν μκ·Έλμ²μ μμ±μλ₯Ό λκ° κ°μ§ μ μλ€.)

```java
public static Order createForPrime(boolean prime, Product product) {
    Order order = new Order();
    order.prime = prime;
    order.product = product;
    return order;
}

public static Order createForUrgent(boolean urgent, Product product) {
    Order order = new Order();
    order.urgent = urgent;
    order.product = product;
    return order;
}
```

- νΈμΆλ  λλ§λ€ μΈμ€ν΄μ€λ₯Ό μλ‘ μμ±νμ§ μμλ λλ€. (Boolean.valueOf)

```java
public static final Boolean TRUE = new Boolean(true);
public static final Boolean FALSE = new Boolean(false);

public static Boolean valueOf(boolean b) {
    return (b ? TRUE : FALSE);
}

// main
Boolean.valueOf(false);
```

- λ°ν νμμ νμ νμ κ°μ²΄λ₯Ό λ°νν  μ μλ λ₯λ ₯μ΄ μλ€. (μΈν°νμ΄μ€ κΈ°λ° νλ μμν¬, μΈν°νμ΄μ€μ μ μ  λ©μλ)
- μλ ₯ λ§€κ°λ³μμ λ°λΌ λ§€λ² λ€λ₯Έ ν΄λμ€μ κ°μ²΄λ₯Ό λ°νν  μ μλ€. (EnumSet)
- μ μ  ν©ν°λ¦¬ λ©μλλ₯Ό μμ±νλ μμ μλ λ°νν  κ°μ²΄μ ν΄λμ€κ° μ‘΄μ¬νμ§ μμλ λλ€. (μλΉμ€ μ κ³΅μ νλ μμν¬)

```java
public interface HelloService {
    String hello();
}

public class ChineseHelloService implements HelloService {

    @Override
    public String hello() {
        return "Ni Hao";
    }
}

@Bean
public HelloService helloService() {
    return new ChineseHelloService();
}

// main
ServiceLoader<HelloService> loader = ServiceLoader.load(HelloService.class);
Optional<HelloService> helloServiceOptional = loader.findFirst();
helloServiceOptional.ifPresent(h -> {
    System.out.println(h.hello()); // Ni Hao
});
```

### λ¨μ 

- μμμ νλ €λ©΄ publicμ΄λ protected μμ±μκ° νμνλ μ μ  ν©ν°λ¦¬ λ©μλλ§ μ κ³΅νλ©΄ νμ ν΄λμ€λ₯Ό λ§λ€ μ μλ€.
- μ μ  ν©ν°λ¦¬ λ©μλλ νλ‘κ·Έλλ¨Έκ° μ°ΎκΈ° μ΄λ ΅λ€.

## μλ²½ κ³΅λ΅ μμ½

- p9, μ΄κ±° νμμ μΈμ€ν΄νΈκ° νλλ§ λ§λ€μ΄μ§μ λ³΄μ₯νλ€.
- p9, κ°μ κ°μ²΄κ° μμ£Ό μμ²­λλ μν©μ΄λΌλ©΄ νλΌμ΄μ¨μ΄νΈ ν¨ν΄μ μ¬μ©ν  μ μλ€.
- p10, μλ° 8λΆν°λ μΈν°νμ΄μ€κ° μ μ  λ©μλλ₯Ό κ°μ§ μ μλ€λ μ νμ΄ νλ ΈκΈ° λλ¬Έμ μΈμ€ν΄μ€ν λΆκ° λλ° ν΄λμ€λ₯Ό λ μ΄μ κ° λ³λ‘ μλ€.
- p11, μλΉμ€ μ κ³΅μ νλ μμν¬λ₯Ό λ§λλ κ·Όκ°μ΄ λλ€.
- p12, μλΉμ€ μ κ³΅μ μΈν°νμ΄μ€κ° μλ€λ©΄ κ° κ΅¬νμ²΄λ₯Ό μΈμ€ν΄μ€λ‘ λ§λ€ λ λ¦¬νλ μμ μ¬μ©ν΄μΌ νλ€.
- p12, λΈλ¦¬μ§ ν¨ν΄
- p12, μμ‘΄ κ°μ²΄ μ£Όμ νλ μμν¬

## μλ²½ κ³΅λ΅ 1. μ΄κ±° νμ

### Enumeration

- μμ λͺ©λ‘μ λ΄μ μ μλ λ°μ΄ν° νμ.
- νΉμ ν λ³μκ° κ°μ§ μ μλ κ°μ μ νν  μ μλ€. νμ-μΈμ΄νν° (Type-Safety)λ₯Ό λ³΄μ₯ν  μ μλ€. μ±κΈν€ ν¨ν΄μ κ΅¬νν  λ μ¬μ©νκΈ°λ νλ€.
- μ§λ¬Έ1) νΉμ  enum νμμ΄ κ°μ§ μ μλ λͺ¨λ  κ°μ μννλ©° μΆλ ₯νλΌ.

```java
Arrays.stream(OrderStatus.values())
      .forEach(orderStatus1 -> System.out.println("orderStatus1 = " + orderStatus1));
```

- μ§λ¬Έ2) enumμ μλ°μ ν΄λμ€μ²λΌ μμ±μ, λ©μλ, νλλ₯Ό κ°μ§ μ μλκ°?

```java
public enum OrderStatus {
    PREPARING(0), SHIPPED(1), DELIVERING(2), DELIVERED(3);

    private int number;

    OrderStatus(int number) {
        this.number = number;
    }
}
```

- μ§λ¬Έ3) enumμ κ°μ == μ°μ°μλ‘ λμΌμ±μ λΉκ΅ν  μ μλκ°?

```java
Order order = new Order();
if (order.orderStatus == OrderStatus.DELIVERED) { // enum
    System.out.println("order = " + order);
}
```

- μ§λ¬Έ4) enumμ keyλ‘ μ¬μ©νλ Mapμ μ μνμΈμ.

```java
EnumMap<OrderStatus, String> enumMap = new EnumMap(OrderStatus.class);
enumMap.put(OrderStatus.PREPARING, "PRE");
enumMap.put(OrderStatus.DELIVERING, "DEL");
enumMap.put(OrderStatus.SHIPPED, "SHIPP");
enumMap.put(OrderStatus.DELIVERED, "DELIVERED");

for (Map.Entry<OrderStatus, String> orderStatusStringEntry : enumMap.entrySet()) {
    System.out.println("orderStatusStringEntry.getKey() = " + orderStatusStringEntry.getKey());
    System.out.println("orderStatusStringEntry.getValue() = " + orderStatusStringEntry.getValue());
}
```

- μ§λ¬Έ5) enumμ λ΄κ³  μλ Setμ λ§λ€μ΄ λ³΄μΈμ.

```java
EnumSet<OrderStatus> allOfOrderStatuses = EnumSet.allOf(OrderStatus.class);
System.out.println("allOfOrderStatuses = " + allOfOrderStatuses);

EnumSet<OrderStatus> noneOfOrderStatuses = EnumSet.noneOf(OrderStatus.class);
System.out.println("noneOfOrderStatuses = " + noneOfOrderStatuses);

EnumSet<OrderStatus> ofOrderStatuses = EnumSet.of(OrderStatus.SHIPPED, OrderStatus.PREPARING);
System.out.println("ofOrderStatuses = " + ofOrderStatuses);
```

### HashMap vs EnumMap

- HashMapμ keyλ₯Ό bucketμ μ μ₯νκ³  κ° bucketμ΄ linked listλ₯Ό μ°Έμ‘° νκ³  μμ. (LinkedListμλ hash(key)κ° κ°μ elementκ° λ€μ΄κ°)
- EnumMapμ κ²½μ° keyλ‘ μ¬μ©ν  κ°μ΄ μ νλμ΄ μμΌλ―λ‘, κ·Έ κ°―μλ§νΌ κΈΈμ΄λ₯Ό κ°μ§ arrayλ₯Ό μ μΈν¨. ν΄λΉ indexμ κ°μ λ£μΌλ©΄ λ¨.

### HashSet vs EnumSet

- HashSetμ HashMapκ³Ό κ°μλ° mapμ valueκ° μλ€ μλ€λ₯Ό νννλ μ§μμ κ°μ κ°μ΄ λ€μ΄κ°.
- EnumSetμ κ°μ΄ μλ€ μλ€λ§ νμνλ©΄ λλκΉ EnumMapμ²λΌ arrayλ‘ κ΅¬ννμ§ μκ³  10101011 κ°μ bit vectorλ‘ κ΅¬νμ΄ κ°λ₯.

## μλ²½ κ³΅λ΅ 2. νλΌμ΄μ¨μ΄νΈ ν¨ν΄

### Flyweight (κ°λ²Όμ΄ μ²΄κΈ)

- κ°μ²΄λ₯Ό κ°λ³κ² λ§λ€μ΄ λ©λͺ¨λ¦¬ μ¬μ©μ μ€μ΄λ ν¨ν΄.
- μμ£Ό λ³νλ μμ±(λλ μΈμ μΈ μμ±, extrinsit)κ³Ό λ³νμ§ μλ μμ±(λλ λ΄μ μΈ μμ±, intrinsit)μ λΆλ¦¬νκ³  μ¬μ¬μ©νμ¬ λ©λͺ¨λ¦¬ μ¬μ©μ μ€μΌ μ μλ€.

![flyweight-factory](flyweight-factory.png)

## μλ²½ κ³΅λ΅ 3. μΈν°νμ΄μ€μ μ μ  λ©μλ

### μλ° 8κ³Ό 9μμ μ£Όμ μΈν°νμ΄μ€μ λ³ν

κΈ°λ³Έ λ©μλ(default method)μ μ μ  λ©μλλ₯Ό κ°μ§ μ μλ€.

- κΈ°λ³Έ λ©μλ
  - μΈν°νμ΄μ€μμ λ©μλ μ μΈ λΏ μλλΌ, κΈ°λ³Έμ μΈ κ΅¬νμ²΄κΉμ§ μ κ³΅ν  μ μλ€.
  - κΈ°μ‘΄μ μΈν°νμ΄μ€λ₯Ό κ΅¬ννλ ν΄λμ€μ μλ‘μ΄ κΈ°λ₯μ μΆκ°ν  μ μλ€.
- μ μ  λ©μλ
  - μλ° 9λΆν° private static λ©μλλ κ°μ§ μ μλ€.
  - λ¨, private νλλ μμ§λ μ μΈν  μ μλ€.
- μ§λ¬Έ1) λ΄λ¦Όμ°¨μμΌλ‘ μ λ ¬νλ `Comparator`λ₯Ό λ§λ€κ³  `List<Integer>`λ₯Ό μ λ ¬νλΌ.

```java
List<Integer> numbers = new ArrayList();
numbers.add(10);
numbers.add(100);
numbers.add(20);
numbers.add(44);
numbers.add(3);

Comparator<Integer> desc = (o1, o2) -> o2 - o1;
numbers.sort(desc);
```

- μ§λ¬Έ2) μ§λ¬Έ1μμ λ§λ  `Comparator`λ₯Ό μ¬μ©ν΄μ μ€λ¦μ°¨μμΌλ‘ μ λ ¬νλΌ.

```java
numbers.sort(desc.reversed());
```

## μλ²½ κ³΅λ΅ 4. μλΉμ€ μ κ³΅μ νλ μμν¬

### νμ₯ κ°λ₯ν μ νλ¦¬μΌμ΄μμ λ§λλ λ°©λ²

- μ£Όμ κ΅¬μ± μμ
  - μλΉμ€ μ κ³΅μ μΈν°νμ΄μ€ (SPI)μ μλΉμ€ μ κ³΅μ (μλΉμ€ κ΅¬νμ²΄)
  - μλΉμ€ μ κ³΅μ λ±λ‘ API (μλΉμ€ μΈν°νμ΄μ€μ κ΅¬νμ²΄λ₯Ό λ±λ‘νλ λ°©λ²)
  - μλΉμ€ μ κ·Ό API (μλΉμ€μ ν΄λΌμ΄μΈνΈκ° μλΉμ€ μΈν°νμ΄μ€μ μΈμ€ν΄μ€λ₯Ό κ°μ Έμ¬ λ μ¬μ©νλ API)
- λ€μν λ³ν
  - λΈλ¦Ώμ§ ν¨ν΄
  - μμ‘΄ κ°μ²΄ μ£Όμ νλ μμν¬
  - java.util.ServiceLoader
    - [https://docs.oracle.com/javase/tutorial/sound/SPI-intro.html](https://docs.oracle.com/javase/tutorial/sound/SPI-intro.html)
    - [https://docs.oracle.com/javase/tutorial/ext/basics/spi.html](https://docs.oracle.com/javase/tutorial/ext/basics/spi.html)

## μλ²½ κ³΅λ΅ 5. λ¦¬νλ μ

### reflection

- ν΄λμ€λ‘λλ₯Ό ν΅ν΄ μ½μ΄μ¨ ν΄λμ€ μ λ³΄(κ±°μΈμ λ°μ¬λ μ λ³΄)λ₯Ό μ¬μ©νλ κΈ°μ 
- λ¦¬νλ μμ μ¬μ©ν΄ ν΄λμ€λ₯Ό μ½μ΄μ€κ±°λ, μΈμ€ν΄μ€λ₯Ό λ§λ€κ±°λ, λ©μλλ₯Ό μ€ννκ±°λ, νλμ κ°μ κ°μ Έμ€κ±°λ λ³κ²½νλ κ²μ΄ κ°λ₯νλ€.
- μΈμ  μ¬μ©ν κΉ?
  - νΉμ  μ λΈνμ΄μμ΄ λΆμ΄μλ νλ λλ λ©μλ μ½μ΄μ€κΈ° (JUnit, Spring)
  - νΉμ  μ΄λ¦ ν¨ν΄μ ν΄λΉνλ λ©μλ λͺ©λ‘ κ°μ Έμ νΈμΆνκΈ° (getter, setter)
- [https://docs.oracle.com/javase/tutorial/reflect/](https://docs.oracle.com/javase/tutorial/reflect/)

```toc
```
