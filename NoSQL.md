# Практика по нереляционным базам данных

### В качестве предусловий требовалось:

- создать коллекцию в **MongoDB**, которая будет повторять содержимое таблицы `card_info` в базе данных `intern_shop`;
- создать кластер в **MongoDB Atlas**;
- установить соединение в **MongoDB Compass**;

### И в качестве задания необходимо было заполнить таблицу соответствующими запросами:

| Задание                                                 | Запрос (MongoDB)                                                                              |
| :------------------------------------------------------ | :-------------------------------------------------------------------------------------------- |
| Создать новую базу данных `qa_shop`                     | `use qa_shop`                                                                                 |
| Создать коллекцию `card_info`                           | `db.createCollection('card_info')`                                                            |
| Наполнить коллекцию тестовыми данными                   | [Перейти к блок-коду](#Тестовые-данные)👇                                                     |
| Вывести `_id` и `card_type` со статусом **valid**       | `db.card_info.find({ status: "valid" }, { card_type: 1 })`                                    |
| Вывести карты, у которых `expiry_month > 6`             | `db.card_info.find({ expiry_month: { $gt: 6 } }, { card_type: 1 })`                           |
| Вывести карты, у которых `expiry_month` от 6 до 10      | `db.card_info.find({ expiry_month: { $gt: 5, $lt: 11 } }, <br> { card_type: 1 })`             |
| Посчитать общее количество карт типа **VISA**           | `db.card_info.countDocuments({ card_type: "VISA" })`                                          |
| Найти карты, в названии типа которых есть буква **"r"** | `db.card_info.find({ card_type: { $regex: /r/i } })`                                          |
| Разблокировать карту (смена статуса на **valid**)       | `db.card_info.updateOne({ card_code: 6107972359241284 }, <br> { $set: { status: "valid" } })` |
| Удалить все заблокированные карты                       | `db.card_info.deleteMany({ status: "blocked" })`                                              |

### Тестовые данные:

```javascript
db.card_info.insertMany([
  {
    card_type: "VISA",
    expiry_month: 12,
    expiry_year: 26,
    cvv: 0,
    balance: 2000,
    status: "blocked",
    card_code: 9181347306820824,
  },
  {
    card_type: "MasterCard",
    expiry_month: 12,
    expiry_year: 26,
    cvv: 456,
    balance: 2000,
    status: "valid",
    card_code: 5248106661644884,
  },
  {
    card_type: "VISA",
    expiry_month: 12,
    expiry_year: 26,
    cvv: 890,
    balance: 0,
    status: "valid",
    card_code: 7178218557247775,
  },
  {
    card_type: "VISA",
    expiry_month: 12,
    expiry_year: 26,
    cvv: 123,
    balance: 2000,
    status: "valid",
    card_code: 8820354696467284,
  },
  {
    card_type: "MasterCard",
    expiry_month: 6,
    expiry_year: 26,
    cvv: 567,
    balance: 2000,
    status: "stolen",
    card_code: 1151842195999505,
  },
  {
    card_type: "VISA",
    expiry_month: 1,
    expiry_year: 20,
    cvv: 789,
    balance: 2000,
    status: "expired",
    card_code: 4340511554108849,
  },
  {
    card_type: "MasterCard",
    expiry_month: 1,
    expiry_year: 20,
    cvv: 101,
    balance: 2000,
    status: "expired",
    card_code: 307328035514696,
  },
  {
    card_type: "VISA",
    expiry_month: 6,
    expiry_year: 26,
    cvv: 234,
    balance: 2000,
    status: "stolen",
    card_code: 779330784258313,
  },
  {
    card_type: "MasterCard",
    expiry_month: 12,
    expiry_year: 26,
    cvv: 0,
    balance: 2000,
    status: "blocked",
    card_code: 6107972359241284,
  },
  {
    card_type: "MasterCard",
    expiry_month: 12,
    expiry_year: 26,
    cvv: 112,
    balance: 0,
    status: "valid",
    card_code: 3320643190265792,
  },
]);
```
