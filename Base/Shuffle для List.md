
Реализация [[Методы расширения|метода расширения]] для перетасовки списка List.

```cs
static class ListExtensions
{
    static Random random = new Random();

    public static void Shuffle<T>(this IList<T> list)
    {
        for (var n = list.Count - 1; n > 1; n--)
        {
            int k = random.Next(n + 1);
            T value = list[k];
            list[k] = list[n];
            list[n] = value;
        }
    }
}
```