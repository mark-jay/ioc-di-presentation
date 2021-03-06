---

## Inversion of Control & Dependency Injection

```
```

---

## План

```text
1. Inversion of Control(IoC)
2. Dependency Injection(DI)
3. IoC контейнеры
4. Dependency Injection в юнит тестах
5. Итог
```

---

### 1. IoC

```text
- Код реюз: библиотеки
```

---

### 1. IoC

```text
- Код реюз: библиотеки или фреймворки?
```

---

### 1. IoC. Пример работы с библиотекой

```text
Использование библиотеки выглядит как-то так:
```

```java
...
String defaultString =
  org.apache.commons.lang3.StringUtils.defaultString(myString, "");
...
```

---

### 1. IoC. Пример работы с фреймворком

```text
Использование фреймворка выглядит как-то так:
```

```java
@OAuthFilter
@Produces(MediaType.APPLICATION_JSON)
@POST
@Path("new")
public StringResponse createNew(
      @Required(label="amount") @FormParam("amount") BigDecimal amount
    ) {
    dao.persist(amount...);
    return new StringResponse("Done");
}
```

---

### 1. IoC. Определение

```text
Инверсия контроля - принцип при котором контроль выполнения программы
остается за фреймворком
```

---

### 1. IoC. Примеры реализации принципа IoC

```text
- strategy pattern
- template pattern
- dependency injection
```

---

### 2. Dependency injection

```text
Dependency injection - это буква D в SOLID.
```

---

### 2. Dependency injection

```text
Dependency? injection?
```

---

### 2. Dependency injection

```text
Пример:
```
```java
class A {

    @Autowired
    private MyService dependencyB;

    public void foo() {
        dependencyB.callBar();
        // do more stuff
    }
}
```
---

### 2. Dependency injection. Определение

```text
Внеднерие зависимостей - шаблон проектирования,
реализующий принцип инверсии контроля(один из).
```

---

### 3. IoC контейнеры

```text
Что это и зачем оно мне?
```

---

### 3. IoC контейнеры

```text
Пример DI без IoC контейнеров:
```
```java
new HelloWorldController(
    new HelloWorldService(
        new HelloDao(),
        new WorldDao()
    )
);
```
---

### 3. IoC контейнеры

```text
Пример DI без IoC контейнеров:
```
```java
dataSource = new DataSource(new DefaultDataSourceConfig())
new HelloWorldController(
    new HelloWorldService(
        new HelloDao(dataSource),
        new WorldDao(dataSource)
    )
);
```

---

### 3. IoC контейнеры

```text
 - CDI
 - spring
 - Pico container
 - guice

```

---

### 3. IoC контейнеры

```text
Почему не static методы:
```

```java
class HelloServiceUtils {

    private HelloServiceUtils() {}

    static void sayHello(String name) {
        HelloDaoUtils.saveName(name);
        // do more stuff
    }
}
```

---

### 4. DI в юнит тестах

```text
Задача:
 - Реализовать метод getGreetings в зависимости от времени суток
```

```java
class GreeterService {

    String getGreetings() {
        if (new Date().getHour() > 21 && new Date().getHour() < 6) {
            return "Good night";
        } else {
            return "Good day";
        }
    }

}
```

---

### 4. DI в юнит тестах

```java
class GreeterService {

    String getGreetings(Date currentTime) {
        if (currentTime.getHour() > 21 && currentTime.getHour() < 6) {
            return "Good night";
        } else {
            return "Good day";
        }
    }

}

// tests
assertEquals(
  new GreeterService().getGreetings(new Date(0L)),
  "Good night"
);
assertEquals(
  new GreeterService().getGreetings(new Date(7*60*60*1000)),
  "Good day"
);
```

---

### 4. DI в юнит тестах

```java
class DateService() {
    Date getCurrentTime() { return new Date(); }
}
class GreeterService {
    private DateService dateService; // constructor omitted

    String getGreetings() {
        if (dateService.getCurrentTime().getHour() > 21 &&
                dateService.getCurrentTime().getHour() < 6) {
            return "Good night";
        } else {
            return "Good day";
        }
    }

}

```

---

### 4. DI в юнит тестах

```java
// tests
DateService dateService = new DateService() {
    @Override Date getCurrentTime() {
        return new Date(0L);
    }
};
assertEquals(
    new GreeterService(dateService).getGreetings(),
    "Good night"
);

```

---

### 4. DI в юнит тестах

```text
Другие примеры статического контекста:

 - new Date()
 - System.currentTimeMillis()
 - SecurityContextHolder.getContext().getAuthentication().getName()
 - SomeClass.staticVariable

```

---

### 5. Итог

```text
 - IoC - принцип когда отдается поток выполнения
 - DI - реализация принципа IoC в виде шаблона проектирования
 - IoC контейнер - фреймворк, занимается внедрением зависимостей
 - DI помогает в написании unit и интеграционных тестов

```
