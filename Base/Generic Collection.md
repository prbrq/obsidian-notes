Объект типа **generic collection** может хранить любой тип объекта. Также такой объект предоставляет методы, которые работают с любым типом объектов.

Пример - `List<T>`

Для создания своего `Generic` класса нужно использовать `<TValue>`. 
```
class SomeClassName<TValue>
{
    // код
}
```

В методе `main` теперь можно создать экземпляр класса `SomeClassName` с каким-либо типом.
```
static void Main(string[] args)
{
    var someVar = new SomeClassName<int>();
}
```

Тип данных `TValue` можно использовать в коде.