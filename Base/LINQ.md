
Это встроенный в C# механизм для удобной работы с [[Generic Collection|коллекциями]].

Большинство алгоритмов, которые на менее развитых языках принято писать с помощью циклов и условных операторов, более компактно и красиво выражаются с помощью примитивов **LINQ**.

В основе **LINQ** лежит возможно использовать со всеми наследниками [[IEnumerable|IEnumerable<T>]].

---

# Методы фильтрации и преобразования

`Where` используется для фильтрации перечисляемого. Он принимает в качестве параметра функцию-предикат и возвращает новое перечисляемое, состоящее только их тех элементов исходного перечисляемого, на которых предикат вернул `true`.

`Select` используется для поэлементного преобразования перечисляемого. Он принимает в качестве параметра преобразующую функцию и возвращает новое перечисляемое, полученное применением этой функции к каждому элементу исходного перечисляемого.

`Take` обрезает последовательность после указанного количества элементов.

`Skip` обрезает последовательность, пропуская указанное количество элементов с начала.

`ToArray` и `ToList` используется для преобразования `IEnumerable<T>` в массив `T[]` или в `List<T>`, соответственно.

> [!Note] Важно понимать
> Эти методы не меняют исходную коллекцию, а возвращают новую последовательность.

---

# Method Chaining

Несколько последовательных действий с перечислением можно объединить в одну цепочку вызовов. Такой примем называется **Method Chaining**. Однако для улучшения читаемости кода настоятельно рекомендуется каждый вызов метода помещать в отдельную строку, вот так:
```cs
var originalIntArray = new[] { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
var resultIntArray = originalIntArray
    .Where(x => x % 2 == 0)
    .Select(x => x * x)
    .Skip(1)
    .Take(2)
    .ToArray();
```

**Method Chaining** делает код компактнее, но скрывает информацию о типах и семантике промежуточных значений. Иногда все же стоит оставлять вспомогательные переменные, чтобы сделать код более читаемым.

---

# SelectMany

Этот метод несколько менее очевиден, чем предыдущие, однако он довольно часто пригождается в самых разных задачах.

В качестве аргумента он принимает функцию, преобразующую каждый элемент исходной последовательности в новую последовательность. Результатом работы является конкатенация всех полученных последовательностей.

Следующий пример пояснит работу этого метода:
```cs
string[] words = { "ab", "", "c", "de" };
var letters = words.SelectMany(word => word); // 'a', 'b', 'c', 'd', 'e'
```

Еще один пример использования метода SelectMany:
```cs
public class Classroom
{
    public List<string> Students = new List<string>();
}

internal class Program
{
    static void Main(string[] args)
    {
        Classroom[] classes =
        {
            new Classroom {Students = {"Pavel", "Ivan", "Petr"},},
            new Classroom {Students = {"Anna", "Ilya", "Vladimir"},},
            new Classroom {Students = {"Bulat", "Alex", "Galina"},}
        };
        var allStudents = GetAllStudents(classes);
        Array.Sort(allStudents);
        Console.WriteLine(string.Join(" ", allStudents));
    }

    public static string[] GetAllStudents(Classroom[] classes)
    {
        return classes.SelectMany(x => x.Students).ToArray();
    }
}
```

---

# OrderBy и Distinct

Для сортировки последовательности в **LINQ** имеется четыре метода. Это `OrderBy`, `OrderByDescending`, `ThenBy` и `ThenByDescending`. 

Первые два дают на выходе последовательность, упорядоченную по возрастанию/убыванию ключей.
```cs
var names = new[] { "Pavel", "Alexander", "Anna" };
IOrderedEnumerable<string> sorted = names.OrderBy(n => n.Length);
```

Если при равенстве ключей необходимо отсортировать элементы по другому критерию, на помощь приходит метод `ThenBy`. Например, в следующем примере все имена сортируются по убыванию длин, а при равных длинах — лексикографически.
```cs
var names = new[] { "Pavel", "Alexander", "Irina" };
var sorted = names
	.OrderByDescending(name => name.Length)
	.ThenBy(n => n);
```

Чтобы убрать из последовательности все повторяющиеся элементы, можно воспользоваться функцией `Distinct`.
```cs
var numbers = new[] { 1, 2, 3, 3, 1, 1, };
var uniqueNumbers = numbers.Distinct();
```

---

# Функции агрегирования

В **LINQ** есть удобные методы для вычисления минимума, максимума, среднего и количества элементов в последовательности. Вот все они в действии:
```cs
IEnumerable<int> nums = new int[] { 8, 9, 0, 1, 2, 3, 4, 5, 6, 7 };
var numsCount = nums.Count(); // 10
var numsMin = nums.Min(); // 0
string[] words = { "hi", "kitty" };
var maxLenInWords = words.Select(word => word.Length).Max(); // 5
var anotherMaxLenInWords = words.Max(word => word.Length); // 5
var numsAvarge = nums.Average(n => n * n); // 28.5
```
Все эти методы при вызове полностью обходят коллекцию. Исключение составляет только метод `Count()` — если последовательность на самом деле реализует интерфейс `ICollection` (в котором есть свойство `Count`), то **LINQ**-метод `Count()` не станет перебирать всю коллекцию, а сразу вернет значение свойства `Count`.

Благодаря этой оптимизации, временная сложность работы **LINQ**-метода `Count()` на массивах, списках, хеш-таблицах и многих других структурах данных — `O(1)`.

Есть еще две полезные функции: `All()` и `Any()`, которые проверяют, выполняется ли заданный предикат для всех элементов последовательности или хотя бы для одного элемента соответственно.
```cs
int[] numbers = { 1, 2, 6, 2, 8, 0, 10, 6, 1, 2 };
var firstCheck = numbers.All(n => n >= 0); // true
var secondCheck = numbers.All(n => n % 2 == 0); // false
var thirdCheck = numbers.Any(n => n == 0); // true
var fourthCheck = numbers.Any(n => n < 0); // false
```

---

# Группировка

**LINQ** содержит несколько методов группировки элементов последовательности по некоторому признаку. Основной способ группировки — это метод `GroupBy()`, который группирует значения по переданному предикату. В следующем примере имена будут сгруппированы по первой группе:
```cs
string[] names = { "Pavel", "Peter", "Andrew", "Anna", "Alice", "John" };
var groups = names
    .GroupBy(name => name[0]);
```

В некотором смысле `GroupBy` — это метод противоположный по действию методу `SelectMany`. `GroupBy` создает группы, а `SelectMany` из списка групп делает плоский список.

---

# ToDictionary и ToLookup

Нередко встречается необходимость, сгруппировав элементы, преобразовать их в структуру данных для поиска группы по ключу группировки. Это можно сделать с помощью метода `ToDictionary()`.

```cs
string[] names = { "Pavel", "Peter", "Andrew", "Anna", "Alice", "John" };

Dictionary<char, List<string>> namesByLetter = names
    .GroupBy(name => name[0])
    .ToDictionary(group => group.Key, group => group.ToList());
```

Но еще проще воспользоваться специальным методом `ToLookup()`:
```cs
string[] names = { "Pavel", "Peter", "Andrew", "Anna", "Alice", "John" };

ILookup<char, string> namesByLetter = names.ToLookup(name => name[0], name => name);

var noNames = namesByLetter['Z']; // возвращает пустое перечисление
```

`ILookup` по неизвестному ключу возвращает пустую коллекцию. Часто это удобнее, чем поведение Dictionary, который в такой ситуации бросает исключение.