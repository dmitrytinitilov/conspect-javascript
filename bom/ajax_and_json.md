# AJAX и JSON

Давайте рассмотрим JavaScript и HTML коды, а затем постараемся построчно их разобрать. Подразумевается, что JavaScript подключен к HTML коду.

```js
function makeXHR()
{
    var xhr=new XMLHttpRequest(); //создание объекта для работы с асинхронными запросами

    xhr.onreadystatechange=function() {
        if (xhr.readyState==4 && xhr.status==200) {
            document.getElementById("myDiv").innerHTML=xhr.responseText;
        }
    }
    
    //формирование запроса 
    xhr.open("GET","ajax_info.txt",true);
    //отправка запроса
    xhr.send();
}
```

```html
<div id="myDiv">
    <h2>Ajax поменяет этот текст</h2>
</div>
<button type="button" onclick="makeXHR()">Послать AJAX</button>
```

Для работы с AJAX нам понадобится объект от конструктора XMLHttpRequest. Любой браузер, поддерживающий AJAX содержит данный конструктор. Создаем мы такой объект строчкой

```js
var xhr=new XMLHttpRequest();
```

Название объекта может быть любым, xhr выбрано просто, чтобы легче было запомнить (сокращение от Xml Http Request)

Далее пропустим пока что строчки для установки обработчика события. и перейдем сразу к формированию AJAX-запроса 

Строка xhr.open подготавливает(но не отправляет) AJAX-запрос. Мы можем отправлять запросы двумя методами протокола http - GET и POST. В данном случае мы используем метод GET. Запрос пойдет к файлу ajax_info.txt, который находится в той же папке, что и наш скрипт. Последний параметр true, говорит о том, что запрос будет асинхронным, то есть мы не будем приостанавливать выполнение скрипта, ожидая получения ответа от сервера
```js
xhr.open("GET","ajax_info.txt",true);
```

Ну и непосредственно отправка запроса на сервер
```js
xhr.send();
```

Рано или поздно, если всё было в порядке, сервер пришлет нам свой ответ, и его нужно обработать. Когда данные поступают от сервера в браузере срабатывает событие onreadystatechange. Поэтому нам необходимо поставить на него обработчик

```js
xhr.onreadystatechange=function() {
    if (xhr.readyState==4 && xhr.status==200) {
        document.getElementById("myDiv").innerHTML=xhr.responseText;
    }
}
```

Данные от сервера могут поступать порциями, затем будут проходить стадию подготовки. Каждый раз, при изменении состояния этого процесса будет вызываться событие onreadystatechange. Нас же интересует только тот момент, когда данные полностью загрузились и готовы для работы с ними. Это соответствует моменту когда xhr.readyState == 4. Строка xhr.status==200 означает, что сервер, к которому мы обращались нормально отработал и сформировал свой ответ.

После того как данные получены, они находятся в поле responseText объекта xhr. В нашем примере мы загружаем их внутрь div'a c id myDiv.

Для того, чтобы протестировать работоспособность этого примера, скопируйте код, приведенный ниже, сохраните его в файле index.html и откройте этот файл в Mozilla Firefox (Chrome и Safari не работают с локальными файлами). Не забудьте создать файл ajax_info.txt - напишите там, например Hello World! Файл создавать лучше в Sublime или Notepad++, но не в Блокноте или WordPad, потому что могут возникнуть проблемы с кодировками, из-за которых ничего не будет работать.

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Document</title>
	<script>
		function makeXHR()		{
			var xhr=new XMLHttpRequest(); //создание объекта для работы с асинхронными запросами

			xhr.onreadystatechange=function() {
				if (xhr.readyState==4 && xhr.status==200) {
					document.getElementById("myDiv").innerHTML=xhr.responseText;
				}
			}
			//формирование запроса
			xhr.open("GET","ajax_info.txt",true);
			//отправка запроса
			xhr.send();
		}
	</script>
</head>
<body>
	<div id="myDiv"><h2>Ajax поменяет этот текст</h2></div>
    <button type="button" onclick="makeXHR()">Послать AJAX</button>
</body>
</html>
```

**Формат JSON**

Как правило нам нужно получать от сервера более сложную информацию, чем просто строку. Если у нас есть разнородная информация, ее необходимо как-то структурировать. Как один из вариантов такого структурирования был придуман формат XML. Ему обязан своим названием объект XMLHttpRequest и сам протокол AJAX (Asynchronous JavaScript And XML). Но поскольку он довольно громоздкий, со временем он был заменён форматом JSON. Пример формата JSON можно посмотреть ниже.

Внимание! Для того, чтобы JSON парсился без ошибок, в нем не должно быть переносов строк

Хорошая подсветка для JSON - Monokai JSON+

```json
{
   "firstName": "Остап",
   "lastName": "Бендер",
   "address": {
       "streetAddress": "Большая Арнаутская 23a",
       "city": "Одесса",
       "postalCode": 65000
   },
   "phoneNumbers": [
       "+380 (48) 728‒10‒68",
       "+380 (48) 777–02–42"
   ]
}
```

Если присмотреться, то JSON очень напоминает способ создания объекта с помощью литеральной нотации. Из отличий - мы должны везде использовать двойные кавычки: для названий свойств это обязательно, для значений исключение делается только для значений-чисел (посмотрите на postalCode в примере). 

Функции внутри JSON'а не описываются. Если нам нужен объект, мы также можем использовать фигурные скобки, если нам нужен массив, то используем квадратные скобки.

**JSON.parse**

JSON вряд ли бы стал сильно популярным, если бы не одно его свойство. Строку из JSON очень просто перевести в JavaScript объект. Для этого используется метод parse объекта JSON. Выглядит это преобразование примерно вот так.

```js
var  obj = JSON.parse(json_string);
```

Заменим содержимое нашего файла ajax_info.txt на JSON следующего вида

```js
{"name":"Остап","lastname":"Бендер"}
```

Если мы оставим строку с xhr.responseText без изменений (код ниже)
```js
document.getElementById("myDiv").innerHTML=xhr.responseText;
```

То при нажатии на кнопку мы увидим наш JSON-код. Это вряд ли то, чего мы хотели добиться! Изменим немного код этой строки

```js
var person = JSON.parse(xhr.responseText);
document.getElementById("myDiv").innerHTML=person.name+' '+person.lastname;
```

Теперь у нас должно вывестись Остап Бендер вместо Hello World! Значит Вы таки да освоили JSON ;)

**JSON.stringify**

Иногда нужно JavaScript-объект преобразовать в JSON. Для этого используется метод JSON.stringify. Например

```js
var obj = {};

obj.a = 5;
obj.b = 7;

var str = JSON.stringify(obj);

console.log(str);//{"a":"5","b":"7"}
```

Поля с методами в объекте либо убираются при таком преобразованиями, либо значение такого поля будет null


**Примеры JSON-файлов**

Мы можем создать JSON состоящий из массива объектов

```json
[
{"name":"Mark","surname":"Twain"},
{"name":"Lev","surname":"Tolstoy"},
{"name":"Antoine","surname":"de Saint-Exupéry"}
]
```

Допустим мы получили этот JSON в переменную str. После того как мы ее распарсим, мы получим массив с тремя элементами, в роли которых будут объекты

```js
var obj=JSON.parse(str);

console.log(obj.length); //3

console.log(obj[1].surname);//Tolstoy
```

**FormData**

```js
    var xhr = new XMLHttpRequest(),
    fd = new FormData();

    fd.append( 'file', input.files[0] );
    xhr.open( 'POST', '/api/upload');
    xhr.onreadystatechange = handler;
    xhr.send( fd );
```

**Полезное чтиво:**

1. XMLHttpRequest VS fetch API
https://www.sitepoint.com/xmlhttprequest-vs-the-fetch-api-whats-best-for-ajax-in-2019/

2. Почему я до сих пор использую xhr вместо fetch 
https://gomakethings.com/why-i-still-use-xhr-instead-of-the-fetch-api/


**Практика:** 

1. Cоздаем JSON файл со свойствами прямоугольника – шириной, высотой и фоновым цветом.  С помощью AJAX считываем его. Парсим с помощью JSON.parse и используем полученные данные для вывода прямоугольника на экран.

2. Есть JSON-файл в нем хранятся имена людей. Получаем данные из файла с помощью AJAX. Вывести квадраты с именами людей внутри.

3. Создаем JSON файл продукта. В нем должно содержаться название, описание, цена и адрес картинки.

4. Создаем JSON, в котором будут храниться информация о товарах. Получить его AJAX'ом и вывести товары ввиде каталога.
5. Создать Single Page Application, имитирующее трехстраничный сайт