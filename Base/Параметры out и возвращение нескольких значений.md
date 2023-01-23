```
public static int ReturnThreeValues(int value, out double half, out double twice)
{
    half = value / 2;
    twice = value * 2;
    return value + 1;
}
```

```
var outTest = ReturnThreeValues(2, out double halfTest, out double twiceTest);
Console.WriteLine(outTest);
Console.WriteLine(halfTest);
Console.WriteLine(twiceTest);
```