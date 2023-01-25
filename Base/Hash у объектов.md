
Если нужно сравнивать два экземпляра [[Классы и объекты|класса]], то помимо переопределения метода [[Сравнение экземпляров классов с помощью Equals()|Equals()]] может потребоваться переопределение метода `GetHashCode()`.

```cs
class Point
{
    public int X { get; set; }
    public int Y { get; set; }

    public override bool Equals(object? obj)
    {
        var compObject = (obj as Point);
        if (compObject == null)
            return false;
        return X == compObject.X && Y == compObject.Y;
    }

    public override int GetHashCode()
    {
        return (X.GetHashCode() * 1234) + (Y.GetHashCode() * Y.GetHashCode());
    }
}

class Program
{
    static void Main(string[] args)
    {
        Point firstPoint = new Point { X = 1, Y = 1 };
        Point secondPoint = new Point { X = 1, Y = 1 };
        var captions = new Dictionary<Point, string>();
        captions[firstPoint] = "Это тестовая точка, у которой оба значения - один";
        Console.WriteLine(captions[firstPoint]);
        Console.WriteLine(captions[secondPoint]);
    }
}
```

В данном примере для одинаковых пар X и Y `GetHashCode()` будет возвращать одинаковые значения.
Проверка ключа в [[Dictionary]] выполняется по хэшу. Так как хэш `firstPoint` и `secondPoint` равен, в словаре можно использовать любой из этих ключей.