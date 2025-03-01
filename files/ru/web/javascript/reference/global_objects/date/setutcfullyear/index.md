---
title: Date.prototype.setUTCFullYear()
slug: Web/JavaScript/Reference/Global_Objects/Date/setUTCFullYear
---

{{JSRef("Global_Objects", "Date")}}

## Сводка

Метод **`setUTCFullYear()`** устанавливает полный год указанной даты по всемирному координированному времени.

## Синтаксис

```
dateObj.setUTCFullYear(yearValue[, monthValue[, dayValue]])
```

### Параметры

- `yearValue`
  - : Целое число, определяющее значение года, например, 1995.
- `monthValue`
  - : Необязательный параметр. Целое число от 0 до 11, представляющее месяцы от января до декабря.
- `dayValue`
  - : Необязательный параметр. Целое число от 1 до 31, представляющее день месяца. Если вы определите параметр `dayValue`, вы также должны определить параметр `monthValue`.

## Описание

Если вы не определите значения параметров `monthValue` и `dayValue`, будут использоваться значения, возвращаемые методами {{jsxref("Date.prototype.getUTCMonth()", "getUTCMonth()")}} и {{jsxref("Date.prototype.getUTCDate()", "getUTCDate()")}}.

Если значение определяемого параметра будет выходить за пределы ожидаемого диапазона, метод `setUTCFullYear()` попытается соответственно обновить другие параметры и информацию о дате в объекте {{jsxref("Global_Objects/Date", "Date")}}. Например, если в качестве `monthValue` передать значение 15, год увеличится на 1 (`yearValue + 1`), а в качестве месяца будет использоваться значение 3.

## Примеры

### Пример: использование метода `setUTCFullYear()`

```js
var theBigDay = new Date();
theBigDay.setUTCFullYear(1997);
```

## Спецификации

| Спецификация                                                                               | Статус             | Комментарии                                            |
| ------------------------------------------------------------------------------------------ | ------------------ | ------------------------------------------------------ |
| ECMAScript 1-е издание.                                                                    | Стандарт           | Изначальное определение. Реализована в JavaScript 1.3. |
| {{SpecName('ES5.1', '#sec-15.9.5.41', 'Date.prototype.setUTCFullYear')}}                   | {{Spec2('ES5.1')}} |                                                        |
| {{SpecName('ES6', '#sec-date.prototype.setutcfullyear', 'Date.prototype.setUTCFullYear')}} | {{Spec2('ES6')}}   |                                                        |

## Совместимость с браузерами

{{Compat}}

## Смотрите также

- {{jsxref("Date.prototype.getUTCFullYear()")}}
- {{jsxref("Date.prototype.setFullYear()")}}
