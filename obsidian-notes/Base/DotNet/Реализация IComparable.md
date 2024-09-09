
Например, у нас есть класс `Point`, который реализует точку на графике. В классе есть два динамических поля с типом `double`, они содержат значения координат. Реализация интерфейса IComparable для класса Point будет следующая:

```cs
public class Point : IComparable
{
    public double X;
    public double Y;

    public int CompareTo(object obj)
    {
        var point = (Point)obj;
        double thisDistance = Math.Sqrt(X * X + Y * Y);
        double thatDistance = Math.Sqrt(point.X * point.X + point.Y * point.Y);
        return thisDistance.CompareTo(thatDistance); // это не рекурсия, thisDistance и thatDistance имеют тип double, который реализует интерфейс IComparable
        //или
        //if (thisDistance < thatDistance) return -1;
        //else if (thisDistance == thatDistance) return 0;
        //else return 1;
    }
}
```