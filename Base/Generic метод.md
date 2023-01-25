
В данном примере реализовано нахождение максимального значения из массива любого типа с интерфейсом [[Реализация IComparable|IComparable]].

```cs
namespace GenericMethodSample
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(Max(new int[0]));
            Console.WriteLine(Max(new[] { 3 }));
            Console.WriteLine(Max(new[] { 3, 1, 2 }));
            Console.WriteLine(Max(new[] { "A", "B", "C" }));
        }

        private static InputType Max<InputType>(InputType[] source)
            where InputType : IComparable
        {
            if (source.Length == 0)
                return default(InputType);
            InputType result = source[0];
            foreach (var item in source)
                if (item.CompareTo(result) > 0)
                    result = item;
            return result;
        }
    }
}
```