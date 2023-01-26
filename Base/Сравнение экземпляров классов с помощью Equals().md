
Любой [[Классы и объекты|класс]] наследует класс `object`, то есть у любого класса есть `virtual` [[Методы|метод]] `Equals()`. Переопределение этого класса с использованием ключевого слова `override` позволяет задать правила для сравнения двух экземпляров этого класса.

```cs
static void Main(string[] args)
{
    var firstPerson = new Person(25, "Arkady");
    var secondPerson = new Person(25, "Arkady");
    Console.WriteLine(firstPerson.Equals(secondPerson));
}

class Person
{
    public int Age { get; }
    public string Name { get; }

    public Person(int age, string name)
    {
        Age = age;
        Name = name;
    }

    public override bool Equals(object? obj)
    {
        var comparedPerson = obj as Person;
        if (comparedPerson == null)
            return false;
        else
            return this.Age == comparedPerson.Age && this.Name == comparedPerson.Name;
    }
}
```

Метод [[Generic Collection|коллекций]] `Contains()` использует метод `Equals()`, поэтому

```cs
List<Person> persons = new List<Person>();
persons.Add(new Person(25, "Arkady"));
persons.Add(new Person(24, "Ivan"));
persons.Add(new Person(19, "Egor"));
Console.WriteLine(persons.Contains(new Person(25, "Arkady"))); // OUTPUT: true
```

Переопределение метода `Equals()` никак не повлияет на математическое сравнение `==`. Дело даже не в том что сравнивая `firstPerson == secondPerson` мы сравниваем переменные с ссылками на кучу, так как в `struct` после переопределения `Equals()` мы также не можем использовать оператор сравнения `==`. Для того, чтобы использовать математические знаки для сравнения, нужно использовать [[Перегрузка операторов|перегрузку операторов]].

IDE будет напоминать о необходимости переопределения метода [[Hash у объектов|GetHashCode()]].