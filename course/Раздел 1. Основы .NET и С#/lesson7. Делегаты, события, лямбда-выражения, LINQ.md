# Лекция: Делегаты, события, лямбда-выражения и LINQ в .NET

**Цель лекции:** Понять фундаментальные концепции, которые лежат в основе многих современных приложений на .NET, включая реактивное программирование, гибкое управление вызовами методов и эффективную работу с данными.

---

### **План лекции:**

1.  **Делегаты**
    *   Что такое делегат? Аналогия.
    *   Объявление, создание и использование делегатов.
    *   Комбинирование делегатов (многопоточные делегаты).
    *   Встроенные делегаты `Action<>` и `Func<>`.

2.  **Лямбда-выражения**
    *   От анонимных методов к лямбда-выражениям.
    *   Синтаксис: (параметры) => выражение-или-блок-кода.
    *   Использование лямбда-выражений с делегатами.

3.  **События**
    *   Паттерн "Наблюдатель" (Observer pattern).
    *   Объявление событий (`event`).
    *   Механизм работы: издатель (publisher) и подписчики (subscribers).
    *   Стандартный подход с `EventHandler` и `EventArgs`.

4.  **LINQ (Language Integrated Query)**
    *   Что такое LINQ? Два основных синтаксиса: запросов и методов расширений.
    *   Основные операции: фильтрация (`Where`), проекция (`Select`), сортировка (`OrderBy`), группировка (`GroupBy`).
    *   Отложенное (deferred) и немедленное (immediate) выполнение.
    *   LINQ to Objects, LINQ to Entities.

5.  **Резюме**
6.  **Контрольные вопросы**

---

### **1. Делегаты**

#### **Что такое делегат? Аналогия.**

**Делегат** — это тип, который представляет ссылку на метод с определенной сигнатурой (списком параметров и типом возвращаемого значения). Проще говоря, делегат — это безопасный указатель на функцию в C++.

**Аналогия:** Представьте себе "список заказов" (делегат) в ресторане. Шеф-повар (ваша программа) не знает заранее, какие именно блюда (методы) будут заказаны. Он просто выполняет заказы из списка. Вы (клиент) можете добавлять в этот список конкретные блюда (методы). Делегат — это и есть этот "список заказов".

#### **Объявление, создание и использование делегатов.**

1.  **Объявление делегата:** Вы определяете сигнатуру, которую должны иметь методы, которые можно будет через этот делегат вызывать.

    ```csharp
    // Ключевое слово delegate, возвращаемый тип, имя и параметры.
    public delegate void MyDelegate(string message);
    public delegate int MathOperation(int a, int b);
    ```

2.  **Создание экземпляра делегата:** Вы связываете делегат с конкретным методом.

    ```csharp
    class Program
    {
        // Метод, который соответствует сигнатуре MyDelegate
        static void DisplayMessage(string text)
        {
            Console.WriteLine($"Сообщение: {text}");
        }

        // Метод, который соответствует сигнатуре MathOperation
        static int Add(int x, int y) => x + y;
        static int Multiply(int x, int y) => x * y;

        static void Main()
        {
            // Создание экземпляра делегата (старый синтаксис)
            MyDelegate del1 = new MyDelegate(DisplayMessage);

            // Создание экземпляра делегата (современный синтаксис)
            MyDelegate del2 = DisplayMessage;

            // Использование делегата
            del1("Привет, Мир!"); // Выведет: "Сообщение: Привет, Мир!"
            del2.Invoke("Еще одно сообщение."); // Альтернативный способ вызова

            // Пример с MathOperation
            MathOperation operation = Add;
            int result = operation(5, 3); // result = 8
            Console.WriteLine(result);

            operation = Multiply;
            result = operation(5, 3); // result = 15
            Console.WriteLine(result);
        }
    }
    ```

#### **Комбинирование делегатов (многопоточные делегаты)**

Делегаты можно комбинировать, создавая цепочки вызовов. Это называется **мulticast-делегат**. Используются операторы `+`, `+=`, `-`, `-=`.

```csharp
class Program
{
    static void Method1() => Console.WriteLine("Method1");
    static void Method2() => Console.WriteLine("Method2");

    static void Main()
    {
        // Делегат без возвращаемого значения (void) особенно полезен для этого
        Action myDelegate = Method1;
        myDelegate += Method2; // Подписываемся на второй метод

        Console.WriteLine("Вызов цепочки делегатов:");
        myDelegate(); // Будут вызваны Method1 и Method2 по очереди.

        myDelegate -= Method1; // Отписываемся от первого метода
        Console.WriteLine("После отписки Method1:");
        myDelegate(); // Будет вызван только Method2
    }
}
```
**Важно:** Если multicast-делегат имеет возвращаемое значение, не `void`, то будет возвращено значение только последнего метода в цепочке.

#### **Встроенные делегаты `Action<>` и `Func<>`**

Чтобы не объявлять свои типы делегатов для каждого случая, .NET предоставляет встроенные обобщенные делегаты:

*   **`Action<>`**: Представляет метод, который не возвращает значение (`void`).
    *   `Action` — метод без параметров.
    *   `Action<int, string>` — метод, который принимает `int` и `string`.
*   **`Func<>`**: Представляет метод, который возвращает значение. **Последний тип-параметр — это тип возвращаемого значения.**
    *   `Func<int>` — метод без параметров, возвращающий `int`.
    *   `Func<int, string, bool>` — метод, который принимает `int` и `string`, а возвращает `bool`.

**Пример:**

```csharp
class Program
{
    static void Main()
    {
        // Использование Action<>
        Action<string> actionDelegate = Console.WriteLine;
        actionDelegate("Привет от Action!");

        // Использование Func<>
        Func<int, int, int> funcDelegate = Add; // Используем метод Add из предыдущего примера
        int sum = funcDelegate(10, 20);
        Console.WriteLine($"Сумма: {sum}");

        // Можно использовать с любым методом, подходящим по сигнатуре
        Func<int, int, int> multiplyDelegate = (a, b) => a * b; // Лямбда-выражение (см. ниже)
        int product = multiplyDelegate(10, 20);
        Console.WriteLine($"Произведение: {product}");
    }

    static int Add(int a, int b) => a + b;
}
```

---

### **2. Лямбда-выражения**

Лямбда-выражения — это краткий способ записи **анонимных методов** (методов без имени). Они идеально сочетаются с делегатами.

#### **Синтаксис**

`(список-параметров) => выражение-или-блок-кода`

*   **Если параметр один**, скобки можно опустить: `x => x * x`
*   **Если параметров нет**, используются пустые скобки: `() => Console.WriteLine()`
*   **Если тело выражения состоит из одного оператора**, оно считается выражением и не требует `return` и `{}`.
*   **Если операторов несколько**, нужно использовать блок `{}` и ключевое слово `return`.

**Примеры:**

```csharp
class Program
{
    static void Main()
    {
        // Пример 1: Простое лямбда-выражение с Func<>
        Func<int, int> square = x => x * x;
        Console.WriteLine(square(5)); // Выведет: 25

        // Пример 2: Лямбда с несколькими параметрами
        Func<int, int, int> add = (a, b) => a + b;
        Console.WriteLine(add(7, 3)); // Выведет: 10

        // Пример 3: Лямбда с блоком кода
        Action<string> log = message =>
        {
            string timestamp = DateTime.Now.ToString("yyyy-MM-dd HH:mm:ss");
            Console.WriteLine($"[{timestamp}] {message}");
        };
        log("Запуск приложения");

        // Пример 4: Использование с делегатом Predicate<T> (частный случай Func<T, bool>)
        List<int> numbers = new List<int> { 1, 2, 3, 4, 5, 6 };
        // Найти все четные числа. Where принимает Func<int, bool>
        var evenNumbers = numbers.Where(n => n % 2 == 0);
        foreach (var num in evenNumbers)
        {
            Console.Write(num + " "); // Выведет: 2 4 6
        }
    }
}
```

---

### **3. События**

#### **Паттерн "Наблюдатель" (Observer pattern)**

События — это реализация паттерна "Издатель-Подписчик" (Publisher-Subscriber). Один объект (**издатель**) рассылает уведомления о своем изменении другим объектам (**подписчикам**), не зная ничего об их типах.

#### **Объявление событий (`event`)**

Ключевое слово `event` используется для объявления события в классе-издателе. Оно основано на делегате.

#### **Механизм работы**

1.  **Издатель (Publisher):** Определяет событие (на основе делегата) и вызывает его, когда происходит что-то важное.
2.  **Подписчики (Subscribers):** Методы, которые подписываются на событие издателя. Эти методы должны соответствовать сигнатуре делегата события.

#### **Стандартный подход с `EventHandler` и `EventArgs`**

.NET предлагает стандартный делегат для событий: `EventHandler` и `EventHandler<TEventArgs>`. Это лучшая практика.

*   `EventHandler`: для событий, которые не передают дополнительных данных.
*   `EventHandler<TEventArgs>`: для событий, которые передают дополнительные данные. `TEventArgs` должен быть производным от класса `EventArgs`.

**Пример: Система оповещения о заказе**

```csharp
using System;

// Класс для хранения дополнительной информации о событии
public class OrderEventArgs : EventArgs
{
    public string OrderId { get; }
    public string Status { get; }

    public OrderEventArgs(string orderId, string status)
    {
        OrderId = orderId;
        Status = status;
    }
}

// Класс-Издатель (Order)
public class Order
{
    // 1. Объявление события с использованием EventHandler<T>
    public event EventHandler<OrderEventArgs> OrderStatusChanged;

    private string _status = "Pending";

    public string Status
    {
        get => _status;
        set
        {
            _status = value;
            // 2. Когда статус меняется, вызываем событие
            OnOrderStatusChanged(value);
        }
    }

    // Метод, ответственный за вызов события. Виртуальный для возможности переопределения.
    protected virtual void OnOrderStatusChanged(string newStatus)
    {
        // ?. - проверка на null (если нет подписчиков)
        OrderStatusChanged?.Invoke(this, new OrderEventArgs("ORD123", newStatus));
    }
}

// Класс-Подписчик (NotificationService)
public class NotificationService
{
    // Метод, который соответствует сигнатуре EventHandler<OrderEventArgs>
    public void OnOrderStatusChanged(object sender, OrderEventArgs e)
    {
        Console.WriteLine($"Уведомление: Заказ {e.OrderId} сменил статус на '{e.Status}'.");
        // Здесь может быть логика отправки email, SMS и т.д.
    }
}

class Program
{
    static void Main()
    {
        // Создаем издателя
        Order order = new Order();

        // Создаем подписчика
        NotificationService notificationService = new NotificationService();

        // 3. Подписываемся на событие
        order.OrderStatusChanged += notificationService.OnOrderStatusChanged;

        // 4. Изменяем статус заказа (это вызовет событие)
        Console.WriteLine("Меняем статус на 'Processing'...");
        order.Status = "Processing";

        Console.WriteLine("Меняем статус на 'Shipped'...");
        order.Status = "Shipped";

        // Отписываемся от события
        // order.OrderStatusChanged -= notificationService.OnOrderStatusChanged;
    }
}
```

---

### **4. LINQ (Language Integrated Query)**

LINQ — это технология, которая позволяет писать запросы к данным прямо в коде C#, независимо от их источника (коллекции в памяти, база данных, XML, JSON).

#### **Два основных синтаксиса**

1.  **Синтаксис запросов (Query Syntax):** Напоминает SQL.
    ```csharp
    var result = from item in collection
                 where item.Condition
                 select item.Property;
    ```
2.  **Синтаксис методов расширений (Method Syntax):** Цепочка методов.
    ```csharp
    var result = collection.Where(item => item.Condition)
                           .Select(item => item.Property);
    ```

Оба синтаксиса компилируются в одно и то же. Синтаксис методов используется чаще.

#### **Основные операции**

Рассмотрим на примере коллекции студентов.

```csharp
public class Student
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Group { get; set; }
    public double AverageGrade { get; set; }
}

List<Student> students = new List<Student>
{
    new Student { Name = "Алексей", Age = 20, Group = "ИТ-101", AverageGrade = 4.5 },
    new Student { Name = "Мария", Age = 19, Group = "ИТ-102", AverageGrade = 5.0 },
    new Student { Name = "Иван", Age = 21, Group = "ИТ-101", AverageGrade = 3.7 },
    new Student { Name = "Ольга", Age = 18, Group = "ИТ-102", AverageGrade = 4.9 }
};
```

*   **Фильтрация (`Where`):** Выбор элементов, удовлетворяющих условию.
    ```csharp
    var topStudents = students.Where(s => s.AverageGrade >= 4.5);
    ```

*   **Проекция (`Select`):** Преобразование каждого элемента в новую форму.
    ```csharp
    var studentNames = students.Select(s => s.Name);
    // Или создание нового анонимного объекта
    var studentInfo = students.Select(s => new { s.Name, s.Group });
    ```

*   **Сортировка (`OrderBy`, `OrderByDescending`, `ThenBy`):**
    ```csharp
    var sortedByGrade = students.OrderByDescending(s => s.AverageGrade);
    var sortedByGroupAndGrade = students
                                .OrderBy(s => s.Group)
                                .ThenByDescending(s => s.AverageGrade);
    ```

*   **Группировка (`GroupBy`):**
    ```csharp
    var studentsByGroup = students.GroupBy(s => s.Group);
    foreach (var group in studentsByGroup)
    {
        Console.WriteLine($"Группа: {group.Key}");
        foreach (var student in group)
        {
            Console.WriteLine($"  {student.Name}");
        }
    }
    ```

**Полный пример с LINQ:**

```csharp
// Найти имена всех студентов из группы "ИТ-101" с оценкой выше 4.0, отсортированных по имени.
var query = students
    .Where(s => s.Group == "ИТ-101" && s.AverageGrade > 4.0)
    .OrderBy(s => s.Name)
    .Select(s => s.Name);

Console.WriteLine("Результат LINQ запроса:");
foreach (var name in query)
{
    Console.WriteLine(name); // Выведет: Алексей
}
```

#### **Отложенное (Deferred) vs Немедленное (Immediate) выполнение**

*   **Отложенное выполнение:** Запрос не выполняется в момент объявления. Он выполняется в момент **перечисления результатов** (например, в цикле `foreach`). `Where`, `Select`, `OrderBy` — отложенные.
    ```csharp
    var deferredQuery = students.Where(s => s.Age > 19);
    students.Add(new Student { Name = "Петр", Age = 22, Group = "ИТ-103", AverageGrade = 4.1 }); // Добавляем нового студента
    // Петр БУДЕТ включен в результат, т.к. запрос выполнится здесь, в foreach.
    foreach (var student in deferredQuery) { ... }
    ```

*   **Немедленное выполнение:** Запрос выполняется сразу же. Методы, которые возвращают одно значение (агрегатные) или коллекцию (`ToList`, `ToArray`, `Count`, `First`, `Max`), вызывают немедленное выполнение.
    ```csharp
    var immediateResult = students.Where(s => s.Age > 19).ToList();
    students.Add(new Student { Name = "Петр", Age = 22, ... });
    // Петр НЕ будет включен в immediateResult, т.к. запрос уже выполнился при вызове ToList().
    ```

---

### **5. Резюме**

*   **Делегаты** — это типобезопасные указатели на методы, основа для событий и LINQ.
*   **Лямбда-выражения** — это краткий синтаксис для создания анонимных методов, которые широко используются с делегатами.
*   **События** — это механизм для реализации слабой связанности между компонентами по паттерну "Издатель-Подписчик".
*   **LINQ** — это мощный и унифицированный синтаксис для запросов к данным различных типов. Он основан на делегатах и лямбда-выражениях.

Вместе эти концепции образуют мощный фундамент для создания гибких, поддерживаемых и эффективных приложений на .NET.

---

### **6. Контрольные вопросы**

1.  Что такое делегат и в чем его основное предназначение? Приведите пример из реальной жизни.
2.  В чем разница между делегатами `Action<>` и `Func<>`? Когда следует использовать каждый из них?
3.  Объясните, что такое лямбда-выражение. Как бы вы переписали следующий метод в виде лямбда-выражения?
    `bool IsEven(int number) { return number % 2 == 0; }`
4.  Опишите жизненный цикл события: от объявления в классе-издателе до обработки в классе-подписчике.
5.  Почему при объявлении события используется ключевое слово `event`, а не просто публичный делегат? В чем заключается его защитная роль?
6.  В чем разница между синтаксисом запросов и синтаксисом методов в LINQ? Какой из них вы предпочтете и почему?
7.  Объясните разницу между отложенным и немедленным выполнением в LINQ. Какие операторы вызывают немедленное выполнение?
8.  Напишите LINQ-запрос (любым синтаксисом) для выборки всех уникальных имен из списка пользователей, которые старше 25 лет, отсортированных по убыванию возраста.
9.  Как оператор `Select` отличается от оператора `Where`? Могут ли они использоваться вместе?
10. Как события помогают достичь слабой связанности (loose coupling) в архитектуре приложения?