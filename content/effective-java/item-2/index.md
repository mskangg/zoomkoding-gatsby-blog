---
emoji: π
title: μ΄νν°λΈ μλ° νΊμλ³΄κΈ° - μμ΄ν 2. μμ±μμ λ§€κ°λ³μκ° λ§λ€λ©΄ λΉλλ₯Ό κ³ λ €νλΌ
date: '2022-09-02 00:12:02'
author: mskangg
tags: java
categories: μ΄νν°λΈ μλ° νΊμλ³΄κΈ°
---

![main](../item-1//main.png)

## μ μ  ν©ν λ¦¬μ μμ±μμ μ νμ  λ§€κ°λ³μκ° λ§μ λ

### λμ1: μ μΈ΅μ  μμ±μ ν¨ν΄ λλ μμ±μ μ²΄μ΄λ

- λ§€κ°λ³μκ° λμ΄λλ©΄ ν΄λΌμ΄μΈνΈ μ½λλ₯Ό μμ±νκ±°λ μ½κΈ° μ΄λ ΅λ€.

```java
// μ½λ 2-1 μ μΈ΅μ  μμ±μ ν¨ν΄ - νμ₯νκΈ° μ΄λ ΅λ€! (14~15μͺ½)
public class NutritionFacts {
    private final int servingSize;  // (mL, 1ν μ κ³΅λ)     νμ
    private final int servings;     // (ν, μ΄ nν μ κ³΅λ)   νμ
    private final int calories;     // (1ν μ κ³΅λλΉ)       μ ν
    private final int fat;          // (g/1ν μ κ³΅λ)       μ ν
    private final int sodium;       // (mg/1ν μ κ³΅λ)      μ ν
    private final int carbohydrate; // (g/1ν μ κ³΅λ)       μ ν

    public NutritionFacts(int servingSize, int servings) {
        this(servingSize, servings, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories) {
        this(servingSize, servings, calories, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat) {
        this(servingSize, servings, calories, fat, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium) {
        this(servingSize, servings, calories, fat, sodium, 0);
    }

    public NutritionFacts(int servingSize, int servings, int calories, int fat, int sodium, int carbohydrate) {
        this.servingSize = servingSize;
        this.servings = servings;
        this.calories = calories;
        this.fat = fat;
        this.sodium = sodium;
        this.carbohydrate = carbohydrate;
    }

    public static void main(String[] args) {
        NutritionFacts cocaCola = new NutritionFacts(10, 10);
    }
}
```

### λμ2: μλ°λΉμ¦ ν¨ν΄

- μμ ν κ°μ²΄λ₯Ό λ§λ€λ €λ©΄ λ©μλλ₯Ό μ¬λ¬λ² νΈμΆν΄μΌ νλ€. (μΌκ΄μ±μ΄ λ¬΄λμ§ μνκ° λ  μλ μλ€.)
- ν΄λμ€λ₯Ό λΆλ³μΌλ‘ λ§λ€ μ μλ€.

```java
// μ½λ 2-2 μλ°λΉμ¦ ν¨ν΄ - μΌκ΄μ±μ΄ κΉ¨μ§κ³ , λΆλ³μΌλ‘ λ§λ€ μ μλ€. (16μͺ½)
public class NutritionFacts {
    // νλ (κΈ°λ³Έκ°μ΄ μλ€λ©΄) κΈ°λ³Έκ°μΌλ‘ μ΄κΈ°νλλ€.
    private int servingSize = -1; // νμ; κΈ°λ³Έκ° μμ
    private int servings = -1; // νμ; κΈ°λ³Έκ° μμ
    private int calories = 0;
    private int fat = 0;
    private int sodium = 0;
    private int carbohydrate = 0;
    private boolean healthy;

    public NutritionFacts() {
    }

    public void setServingSize(int servingSize) {
        this.servingSize = servingSize;
    }

    public void setServings(int servings) {
        this.servings = servings;
    }

    public void setCalories(int calories) {
        this.calories = calories;
    }

    public void setFat(int fat) {
        this.fat = fat;
    }

    public void setSodium(int sodium) {
        this.sodium = sodium;
    }

    public void setCarbohydrate(int carbohydrate) {
        this.carbohydrate = carbohydrate;
    }

    public void setHealthy(boolean healthy) {
        this.healthy = healthy;
    }

    public static void main(String[] args) {
        NutritionFacts cocaCola = new NutritionFacts();
        cocaCola.setServingSize(240);
        cocaCola.setServings(8);
        cocaCola.setCalories(100);
        cocaCola.setSodium(35);
        cocaCola.setCarbohydrate(27);
    }
}
```

### κΆμ₯νλ λ°©λ²: λΉλ ν¨ν΄1

```java
// μ½λ 2-3 λΉλ ν¨ν΄ - μ μΈ΅μ  μμ±μ ν¨ν΄κ³Ό μλ°λΉμ¦ ν¨ν΄μ μ₯μ λ§ μ·¨νλ€. (17~18μͺ½)
public class NutritionFacts {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static void main(String[] args) {
        NutritionFacts cocaCola = new Builder(240, 8)
                .calories(100)
                .sodium(35)
                .carbohydrate(27).build();
    }

    public static class Builder {
        // νμ λ§€κ°λ³μ
        private final int servingSize;
        private final int servings;

        // μ ν λ§€κ°λ³μ - κΈ°λ³Έκ°μΌλ‘ μ΄κΈ°ννλ€.
        private int calories = 0;
        private int fat = 0;
        private int sodium = 0;
        private int carbohydrate = 0;

        public Builder(int servingSize, int servings) {
            this.servingSize = servingSize;
            this.servings = servings;
        }

        public Builder calories(int val) {
            calories = val;
            return this;
        }

        public Builder fat(int val) {
            fat = val;
            return this;
        }

        public Builder sodium(int val) {
            sodium = val;
            return this;
        }

        public Builder carbohydrate(int val) {
            carbohydrate = val;
            return this;
        }

        public NutritionFacts build() {
            return new NutritionFacts(this);
        }
    }

    private NutritionFacts(Builder builder) {
        servingSize = builder.servingSize;
        servings = builder.servings;
        calories = builder.calories;
        fat = builder.fat;
        sodium = builder.sodium;
        carbohydrate = builder.carbohydrate;
    }
}
```

- μ μΈ΅μ  μμ±μλ³΄λ€ ν΄λΌμ΄μΈνΈ μ½λλ₯Ό μ½κ³  μ°κΈ°κ° ν¨μ¬ κ°κ²°νκ³ , μλ°λΉμ¦ λ³΄λ€ ν¨μ¬ μμ νλ€.
- νλ£¨μΈνΈ API λλ λ©μλ μ²΄μ΄λμ νλ€. (thisλ₯Ό λ¦¬ν΄νκΈ° λλ¬Έ)

### κΆμ₯νλ λ°©λ²: λΉλ ν¨ν΄2 (κ³μΈ΅μ  ν΄λμ€)

```java
// μ½λ 2-4 κ³μΈ΅μ μΌλ‘ μ€κ³λ ν΄λμ€μ μ μ΄μΈλ¦¬λ λΉλ ν¨ν΄ (19μͺ½)
// μ°Έκ³ : μ¬κΈ°μ μ¬μ©ν 'μλ?¬λ μ΄νΈν μν νμ(simulated self-type)' κ΄μ©κ΅¬λ λΉλλΏ μλλΌ μμμ μ λμ μΈ κ³μΈ΅κ΅¬μ‘°λ₯Ό νμ©νλ€.
public abstract class Pizza {
    public enum Topping {HAM, MUSHROOM, ONION, PEPPER, SAUSAGE}

    final Set<Topping> toppings;

    abstract static class Builder<T extends Builder<T>> {
        EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);

        public T addTopping(Topping topping) {
            toppings.add(Objects.requireNonNull(topping));
            return self();
        }

        abstract Pizza build();

        // νμ ν΄λμ€λ μ΄ λ©μλλ₯Ό μ¬μ μ(overriding)νμ¬ "this"λ₯Ό λ°ννλλ‘ ν΄μΌ νλ€.
        protected abstract T self();
    }

    Pizza(Builder<?> builder) {
        toppings = builder.toppings.clone(); // μμ΄ν 50 μ°Έμ‘°
    }
}

// μ½λ 2-6 μΉΌμ΄λ€ νΌμ - κ³μΈ΅μ  λΉλλ₯Ό νμ©ν νμ ν΄λμ€ (20~21μͺ½)
public class Calzone extends Pizza {
    private final boolean sauceInside;

    public static class Builder extends Pizza.Builder<Builder> {
        private boolean sauceInside = false; // κΈ°λ³Έκ°

        public Builder sauceInside() {
            sauceInside = true;
            return this;
        }

        @Override
        public Calzone build() {
            return new Calzone(this);
        }

        @Override
        protected Builder self() {
            return this;
        }
    }

    private Calzone(Builder builder) {
        super(builder);
        sauceInside = builder.sauceInside;
    }

    @Override
    public String toString() {
        return String.format("%sλ‘ ν νν μΉΌμ΄λ€ νΌμ (μμ€λ %sμ)",
                toppings, sauceInside ? "μ" : "λ°κΉ₯");
    }
}

// κ³μΈ΅μ  λΉλ μ¬μ© (21μͺ½)
public class PizzaTest {
    public static void main(String[] args) {
        Calzone calzone = new Calzone.Builder()
                .addTopping(HAM)
                .sauceInside()
                .build();

        System.out.println(calzone);
    }
}
```

- κ³μΈ΅μ μΌλ‘ μ€κ³λ ν΄λμ€μ ν¨κ» μ¬μ©νκΈ° μ’λ€.

## λΉλν¨ν΄κ³Ό Lombokμ @Builderμ μ°¨μ΄μ 
`@Builder` <- λ‘¬λ³΅μ λΉλλ‘ μ½λμ μμ μ€μΌ μ μμ§λ§, λκ°μ§ μ°¨μ΄μ μ΄ μ‘΄μ¬νλ€.  

1. λͺ¨λ  λ§€κ°λ³μλ₯Ό λ°λ μμ±μκ° μλ€λ©΄ μλ μμ±λκΈ° λλ¬Έμ `private` μ κ·Όμ μ΄μλ‘ μμ±ν΄μΌν¨
3. νμ κ°μ μ€μ ν  μ μλ λ°©λ²μ΄ μμ

```java
// μ½λ 2-3 λΉλ ν¨ν΄ - μ μΈ΅μ  μμ±μ ν¨ν΄κ³Ό μλ°λΉμ¦ ν¨ν΄μ μ₯μ λ§ μ·¨νλ€. (17~18μͺ½)
@Builder
@AllArgsConstructor(access = AccessLevel.PRIVATE)
public class NutritionFactsLombok {
    private final int servingSize;
    private final int servings;
    private final int calories;
    private final int fat;
    private final int sodium;
    private final int carbohydrate;

    public static void main(String[] args) {
        NutritionFactsLombok cocaCola = NutritionFactsLombok.builder() // λ‘¬λ³΅ λΉλλ νμ λ§€κ°λ³μ μ€μ μ ν  μ μλ€.
                .servingSize(240)
                .servings(8)
                .calories(100)
                .sodium(35)
                .carbohydrate(27)
                .build();
    }
}
```

## μλ²½ κ³΅λ΅ μμ½

p15, μλ°λΉμ¦, κ²ν°, μΈν°  
p17, κ°μ²΄ μΌλ¦¬κΈ° (freezing)  
p17, λΉλ ν¨ν΄  
p19, IllegalArgumentException  
P21, κ°λ³μΈμ (varargs) λ§€κ°λ³μλ₯Ό μ¬λ¬ κ° μ¬μ©ν  μ μλ€.  

## μλ²½ κ³΅λ΅ 6. μλ°λΉ(JavaBean)μ΄λ?

μ¬μ¬μ© κ°λ₯ν μννΈμ¨μ΄ μ»΄ν¬λνΈ (μ£Όλ‘ GUIμμ)

![what-is-bean](what-is-bean.png)

- java.beans ν¨ν€μ§ μμ μλ λͺ¨λ  κ²
- κ·Έ μ€μμλ μλ°λΉμ΄ μ§μΌμΌ ν  κ·μ½
- args μλ κΈ°λ³Έ μμ±μ getter μ setter λ©μλ μ΄λ¦ κ·μ½ Serializable μΈν°νμ΄μ€ κ΅¬ν
  - λνμ μΌλ‘ @RequestBodyμμ μ¬μ©λλ ObjectMapperκ° κΈ°λ³Έμμ±μλ‘ κ°μ²΄λ₯Ό μμ±νκ³  μλ°λΉ κ·μ½μ λ§κ² μ€μ λ getterλ₯Ό μ°Ύμ νλλ₯Ό λ°μΈλ©νλ€.
- νμ§λ§ μ€μ λ‘ μ€λλ  μλ°λΉ μ€ν© μ€μμλ κΈ°λ³Έμμ±μμ getter, setterκ° μ£Όλ‘ μ°λ μ΄μ λ?
  - JPAλ μ€νλ§κ³Ό κ°μ μ¬λ¬ νλ μμν¬μμ λ¦¬νλ μμ ν΅ν΄ νΉμ  κ°μ²΄μ κ°μ μ‘°ννκ±°λ μ€μ νκΈ° λλ¬Έμλλ€.

## μλ²½ κ³΅λ΅ 7. κ°μ²΄ μΌλ¦¬κΈ° (freezing)

μμμ κ°μ²΄λ₯Ό λΆλ³ κ°μ²΄λ‘ λ§λ€μ΄μ£Όλ κΈ°λ₯ (javascript)

- Object.freeze()μ μ λ¬ν κ°μ²΄λ κ·Έλ€λ‘ λ³κ²½λ  μ μλ€.
  - μ νλ‘νΌν°λ₯Ό μΆκ°νμ§ λͺ»ν¨
  - κΈ°μ‘΄ νλ‘νΌν°λ₯Ό μ κ±°νμ§ λͺ»ν¨
  - κΈ°μ‘΄ νλ‘νΌν° κ°μ λ³κ²½νμ§ λͺ»ν¨
  - νλ‘ν νμμ λ³κ²½νμ§ λͺ»ν¨
- strict λͺ¨λμμλ§ λμν¨
- λΉμ·ν λ₯μ νμμΌλ‘ Object.seal()κ³Ό Object.preverntExtensions()κ° μλ€.
- java μ§μμμ κ΅¬ννλ €λ©΄ freeze μνλ₯Ό κ΅¬λΆν  μ μλ νλκ°μ λκ³  λ€λ₯Έ νλλ₯Ό μμ ν  λλ§λ€ freeze μνλ₯Ό μ²΄ν¬νλ λ‘μ§μΌλ‘ κ΅¬ν
  - νμ§λ§ κ±°μ μ°μ§ μμ
  - κ·Έ μ΄μ λ κ°λ³κ°μ²΄μμ freezingμ νκ³  λΆλ³κ°μ²΄λ₯Ό λ§λλ κ²μΈλ°, μΈμ  μ€νΌλ μ΄μμ΄ λλμ§ νμΈνκΈ° μ΄λ ΅κ³  λ§€μ° μ΄ν΄νκΈ° νλ  κ΅¬μ‘°κ°λ  κ°λ₯μ± λμ

## μλ²½ κ³΅λ΅ 8. λΉλ ν¨ν΄

λμΌν νλ‘μΈμ€λ₯Ό κ±°μ³ λ€μν κ΅¬μ±μ μΈμ€ν΄μ€λ₯Ό λ§λλ λ°©λ².

- λ³΅μ‘ν κ°μ²΄λ₯Ό λ§λλ νλ‘μΈμ€λ₯Ό λλ¦½μ μΌλ‘ λΆλ¦¬ν  μ μλ€.

![builder-director](builder-director.png)

- λΉλ μΈν°νμ΄μ€

```java
public interface TourPlanBuilder {
    TourPlanBuilder nightsAndDays(int nights, int days);

    TourPlanBuilder title(String title);

    TourPlanBuilder startDate(LocalDate localDate);

    TourPlanBuilder whereToStay(String whereToStay);

    TourPlanBuilder addPlan(int day, String plan);

    TourPlan getPlan();
}
```

- λΉλ κ΅¬νμ²΄

```java
public class DefaultTourBuilder implements TourPlanBuilder {
    private String title;
    private int nights;
    private int days;
    private LocalDate startDate;
    private String whereToStay;
    private List<DetailPlan> plans;

    @Override
    public TourPlanBuilder nightsAndDays(int nights, int days) {
        this.nights = nights;
        this.days = days;
        return this;
    }

    @Override
    public TourPlanBuilder title(String title) {
        this.title = title;
        return this;
    }

    @Override
    public TourPlanBuilder startDate(LocalDate startDate) {
        this.startDate = startDate;
        return this;
    }

    @Override
    public TourPlanBuilder whereToStay(String whereToStay) {
        this.whereToStay = whereToStay;
        return this;
    }

    @Override
    public TourPlanBuilder addPlan(int day, String plan) {
        if (this.plans == null) {
            this.plans = new ArrayList<>();
        }

        this.plans.add(new DetailPlan(day, plan));
        return this;
    }

    @Override
    public TourPlan getPlan() {
        return new TourPlan(title, nights, days, startDate, whereToStay, plans);
    }
}
```

- λΉλ λλ ν°

```java
public class TourDirector {
    private TourPlanBuilder tourPlanBuilder;

    public TourDirector(TourPlanBuilder tourPlanBuilder) {
        this.tourPlanBuilder = tourPlanBuilder;
    }

    public TourPlan cancunTrip() {
        return tourPlanBuilder.title("μΉΈμΏ€ μ¬ν")
                .nightsAndDays(2, 3)
                .startDate(LocalDate.of(2020, 12, 9))
                .whereToStay("λ¦¬μ‘°νΈ")
                .addPlan(0, "μ²΄ν¬μΈνκ³  μ§ νκΈ°")
                .addPlan(0, "μ λ μμ¬")
                .getPlan();
    }

    public TourPlan longBeachTrip() {
        return tourPlanBuilder.title("λ‘±λΉμΉ")
                .startDate(LocalDate.of(2021, 7, 15))
                .getPlan();
    }
}
```

- λλ ν° μ¬μ© μ½λ

```java
public class App {
    public static void main(String[] args) {
        TourDirector director = new TourDirector(new DefaultTourBuilder());
        TourPlan cancunPlan = director.cancunTrip();
        TourPlan longBeachPlan = director.longBeachTrip();
    }
}
```

## μλ²½ κ³΅λ΅ 9. IllegalArgumentException

μλͺ»λ μΈμλ₯Ό λκ²¨ λ°μμ λ μ¬μ©ν  μ μλ κΈ°λ³Έ λ°νμ μμΈ

```java
if (deliveryDate.isBefore(LocalDate.now())) {  
    throw new IllegalArgumentException("deliveryDate can't be earlier than " + LocalDate.now());  
}
```

- μ§λ¬Έ1) checked exceptionκ³Ό unchecked exceptionμ μ°¨μ΄?
  - ν΄λΌμ΄μΈνΈμκ² κΌ­ νμΈν΄μ μ²λ¦¬νλΌλ λ©μΈμ§λ₯Ό λ¨κΈ°λ κ²μ΄ checked
  - λ°νμ μ€ λ  μ μλ μ΅μμμ΄ unchecked
    - λ§€μ° λ§μ μμΈκ° μ‘΄μ¬νκΈ° λλ¬Έμ λͺ¨λ λ€ μ²΄ν¬ν  μ μμ
- μ§λ¬Έ2) κ°νΉ λ©μλ μ μΈλΆμ unchecked exceptionμ μ μΈνλ μ΄μ λ?
  - λ°νμ μ€ λλ κ² μ€ κΌ­ νμΈνμΌλ©΄ νλ κ²λ€μ throws νλ€.
- μ§λ¬Έ3) checked exceptionμ μ μ¬μ©ν κΉ?  
  - μ΄ λ©μλλ₯Ό νΈμΆν  λ κΌ­ check νλΌλ μλ―Έ

## μλ²½ κ³΅λ΅ 10. κ°λ³μΈμ

μ¬λ¬ μΈμλ₯Ό λ°μ μ μλ κ°λ³μ μΈ argument (Var+args)

```java
public void printNumbers(int... numbers) {  
    System.out.println(numbers.getClass().getCanonicalName());  
    System.out.println(numbers.getClass().getComponentType());  
    Arrays.stream(numbers).forEach(System.out::println);  
}  
  
public static void main(String[] args) {  
    VarargsSamples samples = new VarargsSamples();  
    samples.printNumbers(1, 20, 20, 39, 59);  
}
```

- κ°λ³μΈμλ λ©μλμ μ€μ§ νλλ§ μ μΈν  μ μλ€.
- κ°λ³μΈμλ λ©μλμ κ°μ₯ λ§μ§λ§ λ§€κ°λ³μκ° λμ΄μΌ νλ€.
- λΉλν¨ν΄μ λ©μλ μ²΄μ΄λ λ°©μμΌλ‘ κ°λ³μΈμλ₯Ό μ¬λ¬κ° λ°μ μ μλ€.

```java
public SampleBuilder strings(String... strings){
    this.strings = strings;
    return this;
}

public SampleBuilder numbers(int... numbers){
    this.numbers = numbers;
    return this;
}

// main
public static void main(String[] args) {  
    new SampleBuilder()
        .strings("1","2","3")
        .numbers(1,2,3,4,5,6)
        .build();
}
```

```toc
```
