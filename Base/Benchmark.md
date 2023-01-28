
# Использование класса Stopwatch

Для измерения времени выполнения участков кода можно использовать [[Классы и объекты|класс]] `Stopwatch`.

```cs
var sw = new Stopwatch();
sw.Start();
// Измеряемый код
sw.Stop();
Console.WriteLine(sw.Elapsed); // Здесь логируем
```

Чтобы каждый раз не расставлять `Stopwatch` по коду можно унифицировать процедуру измерения через функцию высшего порядка, например, так:

```cs
static void LambdaMeter(string label, Action act)
{
    var sw = new Stopwatch();
    sw.Start();
    act(); // Измеряемый код
    sw.Stop();
    Console.WriteLine($"{label} : {sw.Elapsed}"); // Здесь логируем
}
```

Такой прием крайне ограничен. Дело в том, что в измеряемом коде могут быть выходы `return`, в поэтому в `Action` его не завернуть. Даже использование `Func` не решит проблему. [Здесь](https://habr.com/ru/company/tinkoff/blog/454058/) можно почитать об одном из возможных решений.