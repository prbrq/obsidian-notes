
Это [[Классы и объекты|класс]], представляющий собой изменяемую строку. 

```cs
var builder = new StringBuilder();
```

**StringBuilder** содержит различные [[Методы|методы]], такие как `Append()`, `Insert()`, `Remove()`. 

```cs
builder.Append("Some ");
builder.Append("string ");
builder.Append("#15");
builder.Remove(0, 5);
builder.Insert(0, "test ")
```

Также можно манипулировать отдельными символами.

```cs
builder[0] = 'T';
```

**StringBuilder** можно превратить в строку.

```cs
var str = builder.ToString();
Console.WriteLine(str
```

Конкатенация со **StringBuilder** работает в сотни раз быстрее.
Однако, в случае 3-4 строк разница не сильно большая, поэтому 
в таком случае пользоваться **StringBuilder** не обязательно.