
```cs
var a = 401;
var b = 785;
a = a ^ b;
b = b ^ a;
a = a ^ b;
Console.WriteLine(a); // OUTPUT: 785
Console.WriteLine(b); // OUTPUT: 401
```