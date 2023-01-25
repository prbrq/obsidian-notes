
С помощью ключевого слова `params` можно указать параметр [[Статические и динамические методы|метода]], принимающий переменное число аргументов. Тип параметра должен быть одномерным массивом.

```cs
static void Main(string[] args)
{
    Print(1, 2);
    Print("a", 'b');
    Print(1, "a");
    Print(true, "a", 1);
}

public static void Print(params object[] objects)
{
    for (var i = 0; i < objects.Length; i++)
    {
        if (i > 0)
            Console.Write(", ");
        Console.Write(objects.GetValue(i));
    }
    Console.WriteLine();
}
```