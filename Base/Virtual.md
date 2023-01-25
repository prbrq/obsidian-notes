
`Virtual` [[Статические и динамические методы|метод]] - метод, функциональность которого можно переопределить с помощью ключевого слова `override`. 

```cs
class Point
{
    public int X;
    public int Y;

    public override string ToString()
    {
        return string.Format("{0}, {1}", X, Y); 
    }
}
internal class Program
{
    static void Main(string[] args)
    {
        var point = new Point { X = 1, Y = 3 };
        Console.WriteLine(point);
    }
}
```

В этом примере [[Классы и объекты|класс]] `Point`, как и любой другой класс, наследует базовый класс `Object`. В этом базовом классе есть метод `ToString()` с ключевым словом `virtual`. Поэтому в классе `Point` можно переопределить метод `ToString()`, используя ключевое слово `override`.