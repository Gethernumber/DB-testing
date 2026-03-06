# Практика по реляционным базам данных

Данное практическое задание было нацелено на закрепление полученных знаний применения SELECT и JOIN запросов.

## _SELECT_

### В качестве предусловий требовалось:

- Установить **DBeaver**.
- Подключиться к базе данных `intern_shop`.

### И в качестве задания необходимо было заполнить таблицу соответствующими запросами:

| Задание                                                       | SQL Запрос                                                                                       |
| :------------------------------------------------------------ | :----------------------------------------------------------------------------------------------- |
| Вывести информацию обо всех продуктах                         | `SELECT * FROM products;`                                                                        |
| Продукты Apple в категории **Phones**                         | `SELECT * FROM products WHERE category = 'Phones' AND manufacturer = 'Apple';`                   |
| Поиск продуктов, содержащих **"sa"** в названии               | `SELECT name, price FROM products WHERE name LIKE '%sa%';`                                       |
| Продукты с ценой в диапазоне **от 100 до 1000**               | `SELECT name, price FROM products WHERE price BETWEEN 100 AND 1000;`                             |
| Количество товаров Samsung (**SAMSUNG TOTAL PRICE**)          | `SELECT COUNT(*) AS 'SAMSUNG TOTAL PRICE' FROM products WHERE manufacturer = 'Samsung';`         |
| Сортировка всех товаров по цене (по убыванию)                 | `SELECT name, price FROM products ORDER BY price DESC;`                                          |
| Список уникальных производителей                              | `SELECT DISTINCT manufacturer FROM products;`                                                    |
| Первые две уникальные категории                               | `SELECT DISTINCT category FROM products LIMIT 2;`                                                |
| Товары на букву **"A"** длиной в 12 символов                  | `SELECT name FROM products WHERE CHAR_LENGTH(name) = 12 AND name LIKE 'A%';`                     |
| Средняя цена всех продуктов (**PRODUCTS AVG PRICE**)          | `SELECT AVG(price) AS 'PRODUCTS AVG PRICE' FROM products;`                                       |
| Продукты производителей **Samsung** и **Huawei** (через `IN`) | `SELECT name, description FROM products WHERE manufacturer IN ('Samsung','Huawei');`             |
| Объединение названий товаров и номеров заказов                | `SELECT name FROM products UNION SELECT CAST(order_id AS CHAR) FROM orders;`                     |
| Категории, где количество товаров **больше 15**               | `SELECT category, COUNT(*) AS total_count FROM products GROUP BY category HAVING COUNT(*) > 15;` |
| Классификация товаров по бренду (через `CASE`)                | См. детальный запрос ниже 👇                                                                     |

### Детальный запрос с использованием CASE:

```sql
SELECT
    manufacturer,
    category,
    price,
    name,
    CASE
        WHEN manufacturer = 'Apple'   THEN 'Это продукт компании Apple'
        WHEN manufacturer = 'Samsung' THEN 'Это продукт компании Samsung'
        WHEN manufacturer = 'Huawei'  THEN 'Это продукт компании Huawei'
        WHEN manufacturer = 'Xiaomi'  THEN 'Это продукт компании Xiaomi'
        ELSE 'Производитель не определен'
    END AS TextMessage
FROM products;
```

---

## _JOIN_

### В качестве предусловий требовалось:

- Создать несколько заказов от имени пользователя и оплатить их через GUI или API.
- Подключиться к базе данных `intern_shop`.

### И в качестве задания необходимо было заполнить таблицу соответствующими запросами, с использованием псевдонимов таблиц:

| Задание                                                                                                                                                                    | Тип связи    | SQL запрос  |
| :------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :----------- | :---------- |
| Выведите логин вашего пользователя, номера его заказов и их стоимость <br> (таблицы users и orders)                                                                        | `INNER JOIN` | См. блок №1 |
| Выведите номера всех заказов, названия товаров в них и их количество <br> (таблицы order_items и products)                                                                 | `INNER JOIN` | См. блок №2 |
| Выведите логины всех пользователей и номера заказов, вне зависимоcти от того, есть ли у <br> них заказ или нет (таблицы users и orders)                                    | `LEFT JOIN`  | См. блок №3 |
| Выведите номера оплаченных заказов и название всех товаров, вне зависимоcти от того, <br> упоминаются ли товары в оплаченных заказах (таблицы products и order_items_paid) | `RIGHT JOIN` | См. блок №4 |
| Используя вложенный запрос выведите названия и стоимость товаров, у которых стоимость <br> товара больше, чем стоимость товара "Samsung Active 5" из таблицы products      | `Subquery`   | См. блок №5 |

### Блок №1 | Объединение Users и Orders (INNER JOIN):

```sql
SELECT u.login, o.order_id, o.total
FROM users u
INNER JOIN orders o ON u.user_id = o.user_id
WHERE u.login = 'testman_1';
```

### Блок №2 | Связь позиций заказа с названиями товаров (INNER JOIN):

```sql
SELECT oi.order_id, p.name, oi.quantity
FROM order_items oi
INNER JOIN products p ON oi.product_id = p.product_id;
```

### Блок №3 | Полный список пользователей (LEFT JOIN):

```sql
-- Позволяет увидеть пользователей даже без заказов (в поле order_id будет NULL)
SELECT u.login, o.order_id
FROM users u
LEFT JOIN orders o ON u.user_id = o.user_id
ORDER BY o.order_id DESC;
```

### Блок №4 | Проверка всех товаров в оплаченных заказах (RIGHT JOIN):

```sql
SELECT oip.order_id, p.name
FROM order_items_paid oip
RIGHT JOIN products p ON oip.product_id = p.product_id;
```

### Блок №5 | Использование вложенного запроса (Subquery):

```sql
SELECT name, price
FROM products
WHERE price > (
    SELECT MAX(price)
    FROM products
    WHERE name = 'Samsung Active 5'
);
```
