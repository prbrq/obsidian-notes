
`Dictionary` представляет собой реализацию стандартной [хеш-таблицы](http://en.wikipedia.org/wiki/Hashtable). Подробнее можно почитать в этой [статье](https://habr.com/ru/post/198104/).

Метод `ContainsKey()`, работает гораздо быстрее `ContainsValue()`. Так как `ContainsKey()` просто обращается к `Dictionary` по ключу, а `ContainsValue(value)` сравнивает все записи словаря с `value`.