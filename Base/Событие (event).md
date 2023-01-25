
Напишем таймер, который раз в 1000 миллисекунд будет вызывать переданный в него [[Статические и динамические методы|метод]].

```cs
using System;
using System.Threading;

public class Timer
{
    public int Interval;
    public Action Tick;

    public void Start()
    {
        while(true)
        {
            if(Tick != null) // если в Tick ничего не присвоить и вызвать, будет NullReferenceException
                Tick();
            Thread.Sleep(Interval); // ждет заданное время
        }
    }
}

public class Program
{
    public static void Main()
    {
        var timer = new Timer();
        timer.Interval = 1000;
        timer.Tick = () => Console.WriteLine("Tick!");
        timer.Start();
    }
}
```

У таймера есть проблема на уровне контроля целостности. `Tick()` можно вызвать в обход события таймера.

```cs
public static void Main()
{
    timer.Tick();
}
```

Чтобы избавиться от этой проблемы нужно добавить к объявлению Tick ключевое слово `event`.

```cs
public event Action Tick;
```

После добавления event
- менять методы с помощью `+=` и `-=` можно отовсюду, в том числе извне [[Классы и объекты|класса]] Timer,
- вызвать Tick можно только внутри класса Timer,
- использовать обычное `=` можно только внутри класса Timer. Присваивание не безопасно, потому что перетирает список методов, которые уже были подписаны на событие.

`event` - это не модификатор доступа, а отдельный вид членов класса, как поле, метод или свойство. Он похож на свойство: у свойства при чтении и присваивании вызываются методы `get` и `set`. А для event при `+=` и `-=` вызываются методы `add` и `remove`.

`public event Action Tick;` - это сокращенная запись события с методами по умолчанию. Полная версия записывается так:

```cs
private Action tick;
public event Action Tick
{
    add => tick += value;
    remove => tick -= value;
}
```