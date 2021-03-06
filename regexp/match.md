#match

https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/String/match

В отличии от search match позволяет находить несколько вхождений паттерна в строку, либо умеет разбивать исходную строку на подстроки за счет подмасок в регулярном выражении

**Работа с подмасками**

Допустим мы хотим найти числа, разделенные двоеточием

```js
var str = 'Есть у нас вот такая строка с числами 175:27 и всё';

var res = str.match(/(\d+):(\d+)/);

```

В res мы получим массив ['175:27','175','27'] , то есть значение первой подмаски пойдет в первый элемент массива, значение второй подмаски пойдет во второй элемент массива.

**Поиск нескольких вхождений**

Иногда нам нужно найти все вхождения нашего паттерна в строку. Для этого существует модификатор **g** в регулярных выражениях

```js
var str = "Все эти пары чисел 489:80, 34:26, 89:17" будут найдены";

var results = str.match(/\d+:\d+/g);
```

results будет массивом с элементами ['489:80','34:26','89:17']

**Практика:**

1. Найти упоминание времени в строке, например 19:40 . Обратите внимание, что записи вроде 25:67 не являются записями о времени