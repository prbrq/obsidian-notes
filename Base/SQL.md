
Данные, которые возвращает `SELECT`, называются **result-set**.

---

# LIKE паттерны

## %

Следующее выражение возвращает всех `Customers` с `City`, которые начинаются с `ber`.

```sql
SELECT * FROM Customers
WHERE City LIKE 'ber%'
```

## _

Следующее выражение возвращает всех `Customers` с `City`, которые заканчиваются на `ondon`.

```sql
SELECT * FROM Customers
WHERE City LIKE '_ondon'
```

## \[charlist\]

Следующее выражение возвращает всех `Customers` с `City`, которые начинаются с `b`, `s` или `p`.

```sql
SELECT * FROM Customers
WHERE City LIKE '[bsp]%'
```

Следующее выражение возвращает всех `Customers` с `City`, которые начинаются с `a`, `b`, `c` или `d`.

```sql
SELECT * FROM Customers
WHERE City LIKE '[a-d]%'
```

## \[!charlist\]

Следующее выражение возвращает всех `Customers` с `City`, которые не начинаются с `b`, `s` или `p`.

```sql
SELECT * FROM Customers
WHERE City LIKE '[bsp]%'
```

---

# EXIST оператор

Оператор `EXIST` используется для того чтобы проверить существуют ли записи в подзапросе.

Оператор `EXIST` возвращает `TRUE`, если подзапрос возвращает хотя бы одну запись.

```sql
SELECT s_country FROM i_ostTmc
WHERE EXISTS (SELECT iKey FROM s_country WHERE iKey = s_country)
```

---

# Операторы ANY и ALL

Оператор `ANY` возвращает `TRUE`, если хотя бы одно значение подзапроса соответствует условию.

```sql
SELECT * FROM i_ostTmc
WHERE s_tmc > ANY (SELECT iKey FROM s_tmc WHERE iKey IN (64, 70))
-- вернутся все записи, у которых s_tmc больше 64
```

Оператор `ANY` возвращает `TRUE`, если все значения подзапроса соответствуют условию.

```sql
SELECT * FROM i_ostTmc
WHERE s_tmc > ALL (SELECT iKey FROM s_tmc WHERE iKey IN (64, 70))
-- вернутся все записи, у которых s_tmc больше 70
```

---

# SELECT INTO

Выражение `SELECT INTO` копирует данные из заданной таблицы в новую. Можно копировать из одной базы данных в другую, если использовать ключевое слово `IN`.

Общий синтаксис:
```sql
SELECT *
INTO newtable [IN externaldb]
FROM oldtable
WHERE condition
```

---

# CASE

Общий синтаксис:
```sql
CASE
	WHEN condition1 THEN result1
	WHEN condition2 THEN result2
	WHEN conditionN THEN resultN
	ELSE result
END AS aliasName
```

Пример:
```sql
SELECT s_tmc, 
CASE 
	WHEN s_tmc > 30 THEN 'Больше 30'
	WHEN s_tmc = 30 THEN 'Равно 30'
	ELSE 'Меньше 30'
END AS s_tmcText
FROM i_ostTmc
```

---

# Хранимые процедуры

Это SQL-код, который можно сохранить, чтобы затем повторно его использовать. Вызывать хранимую процедуру можно с параметрами.

Общий синтаксис:

```sql
CREATE PROCEDURE procedure_name
AS sql_statement
GO;
```

Выполнение сохраненной процедуры:

```sql
EXEC procedure_name;
```

Пример создания процедуры:

```sql
CREATE PROCEDURE SelectAllCustomers  
AS  
SELECT * FROM Customers  
GO;
```

Пример выполнения:

```sql
EXEC SelectAllCustomers;
```

## Хранимая процедура с несколькими параметрами

Пример создания процедуры:

```sql
CREATE PROCEDURE SelectAllCustomers @City nvarchar(30), @PostalCode nvarchar(10)  
AS  
SELECT * FROM Customers WHERE City = @City AND PostalCode = @PostalCode  
GO;
```

Пример выполнения:

```sql
EXEC SelectAllCustomers @City = 'London', @PostalCode = 'WA1 1DP';
```

---

# Создание таблицы с использованием другой таблицы

Копия существующей таблицы может быть создана с помощью `CREATE TABLE`.

Новая таблица получает те же определения столбцов. Можно выбрать все столбцы или определенные столбцы.

Новая таблица будет заполнена существующими значениями из старой таблицы.

Синтаксис:

```sql
CREATE TABLE _new_table_name_ AS  
    SELECT _column1, column2,..._  
    FROM _existing_table_name_  
    WHERE ....;
```

Пример:

```sql
CREATE TABLE TestTable AS  
SELECT customername, contactname  
FROM customers;
```