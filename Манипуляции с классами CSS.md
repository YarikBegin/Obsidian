[[DOM]]

### Получение имени класса. Свойство `className`

У каждого элемента DOM есть свойство `className`. С его помощью можно прочитать или записать значение атрибута `class`:
```html
<div class="princess">Елизавета</div>
```

```js
// выбираем элемент c классом 'princess'
let rank = document.querySelector('.princess');
console.log(rank.className); // princess

rank.className = 'queen'; // принцесса стала королевой, перезаписываем класс на 'queen'
console.log(rank.className); // queen
```

Если у элемента несколько классов, в свойстве `className` они будут разделены пробелами:
```html
<div class="her majesty queen">Елизавета</div>
```

```js
// выбираем элемент c классом 'queen'
let rank = document.querySelector('.queen');

console.log(rank.className); // her majesty queen
```

Управлять стилями через свойство `className` неудобно. Если мы хотим добавить, удалить или изменить один из классов, нам придётся совершать разные манипуляции со строкой: разбить её по пробелам, изменить один из элементов получившегося массива, склеить и записать обратно в `className`. Есть способ получше.

### Получение списка классов. Свойство `classList`

Для управления атрибутом `class` удобнее пользоваться свойством `classList`. Оно содержит список всех классов элемента и обладает собственными методами, чтобы управлять этими классами.
```html
<!-- В именах классов записаны марки машин Её Величества -->
<div class="bentley rolls-royce">Королевский гараж</div>
```

```js
/* получаем список машин королевы в переменной,
обратившись к соответствующему элементу с селектором .bentley */

let garage = document.querySelector('.bentley');
console.log(`Гараж Её Величества: ${garage.classList}`); // Гараж Её Величества: bentley rolls-royce
```

### Проверка наличия класса. Метод `contains`

Метод `contains` проверяет, есть ли у элемента класс, переданный как аргумент:
```js
let garage = document.querySelector('.bentley');

garage.classList.contains('bentley'); // true — bentley есть
garage.classList.contains('jaguar'); // false — а jaguar нет
```

### Присвоение класса элементу. Метод `add`

Метод `add` добавляет элементу класс:
```js
// в королевский гараж поступил Ягуар
garage.classList.add('jaguar');

console.log(`Гараж Её Величества: ${garage.classList}`); // bentley rolls-royce jaguar
```

### Удаление класса. Метод `remove`

Метод `remove` удаляет у элемента класс, переданный как аргумент:
```js
garage.classList.remove('jaguar'); // Ягуар надоел

console.log(`Гараж Её Величества: ${garage.classList}`); // bentley rolls-royce
```

### Переключение класса. Метод `toggle`

Метод `toggle` работает как `add`_,_ если у элемента класс отсутствует, и как `remove` — если присутствует. То есть метод переключает класс у элемента:
```html
<div class="bentley rolls-royce jaguar">Королевский гараж</div>
```

```js
// А если Ягуар есть, нужно от него избавиться  
garage.classList.toggle('jaguar');

console.log(`Гараж Её Величества: ${garage.classList}`); // bentley rolls-royce
```
