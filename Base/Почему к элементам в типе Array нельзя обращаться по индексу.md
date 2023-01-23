
```
int[] someInts = new int[] { 1, 2, 3 };
Array someIntsAsArray = someInts;
someInts[0] = 4;
someIntsAsArray.SetValue(5, 2); // почему в этом случае нельзя просто задать значение по индексу 2?
Console.WriteLine(someInts[0]);
Console.WriteLine(someIntsAsArray.GetValue(2)); // а в этом случае нельзя взять значение по индексу?
Console.Write("\nА вот сам массив: ");
foreach (var integer in someInts)
    Console.Write(integer);
```