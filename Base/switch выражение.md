
```cs
var random = new Random();

string message = random.Next(0, 9) switch
{
    > 5 => "больше пяти",
    < 5 => "меньше пяти",
    _ => "пять"
};

Console.WriteLine(message);
```