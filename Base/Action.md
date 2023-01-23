
Это сокращенный вариант использования делегатов, который возвращает `void`.

```cs
namespace DelegateProject
{
    //public delegate void TellUser(string input); определение типа делегата не нужно, так как в примере используется сокращенная запись Action<>
    class Program
    {
        static void Main(string[] args)
        {
            Run(Console.WriteLine);
            Run(TellUpperCase);
        }

        static void Run(Action<string> tellUser)
        {
            tellUser("hi!");
            tellUser("how r u?");
        }

        static void TellUpperCase(string input)
        {
            Console.WriteLine(input.ToUpper());
        }
    }
}
```