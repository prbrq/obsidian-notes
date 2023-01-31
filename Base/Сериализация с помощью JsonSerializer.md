
```cs
class Student
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public int Age { get; set; }
}

class Program
{
    static void Main(string[] args)
    {
        var students = new List<Student>
        { 
            new Student
            {
                FirstName = "John",
                LastName = "Smith",
                Age = 20,
            },
            new Student
            {
              FirstName = "James",
              LastName = "Adams",
              Age = 19,
            }
        };

        var str = JsonSerializer.Serialize(students);
        Console.WriteLine(str);
    }
}
```