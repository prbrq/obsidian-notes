
Существует два варианта использования ключевого слова `using`: импорт пространства имен и генерация оператора `finally`, который вызывает метод `Dispose` для объекта, реализующего интерфейс `IDisposable`.

Во втором случае компилятор изменяет блок оператора `using` на оператор `try` и `finally` без оператора `catch`. Кроме того, в блоке `using` возможно использовать вложенные операторы `try`, так что при необходимости можно перехватить какие-либо исключения.

```cs
using (FileStream file2 = File.OpenWrite(Path.Combine(path, "file2.txt")))
{
    using (StreamWriter writer2 = new StreamWriter(file2))
    {
        try
        {
            writer2.WriteLine("Welcome, .NET Core!");
        }
        catch (Exception ex)
        {
            WriteLine($"{ex.GetType()} says {ex.Message}");
        }
    } // автоматический вызов метода Dispose, если объект не равен null
} // автоматический вызов метода Dispose, если объект не равен null
```