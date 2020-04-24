#Базовые директивы

**v-if**

```html
<h1 v-if="ok_flag">Vue восхитителен!</h1>
```

Блок отобразится, если у переменной в v-if значение true

Если мы хотим подключить else

```html
<h1 v-if="awesome">Vue восхитителен!</h1>
<h1 v-else>О, нет 😢</h1>
```

**v-model**

```html
<input v-model="message" placeholder="отредактируй меня">
<p>Введённое сообщение: {{ message }}</p>
```



**v-for**

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

Если в списке рендерится картинка

html```
<img :src="img_addr">
```

Где img_addr - адрес картинки, который находится в data

**Практика:**

1. Есть checkbox - при его выборе, выводить пользователю сообщение
2. Есть список имен вывести его ввиде списка
3. Есть список товаров, вывести его ввиде каталога. У каждого товара должно быть название, цена и картинка
