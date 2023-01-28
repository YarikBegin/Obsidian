[[DOM]]

DOM — древовидная структура: все элементы связаны друг с другом. Есть родительские, дочерние и сестринские. Последние ещё называют соседскими. Каждый элемент содержит ссылки на «родителей», «детей» и «соседей». Рассмотрим специальные свойства, в которых хранятся эти ссылки.

## Ссылка на родителя — `parentElement`

Свойство `parentElement` содержит ссылку на родительский элемент:

![image](https://pictures.s3.yandex.net/resources/DOM_BOM_JS___1__28_1587288298.png)

_Абзацы ссылаются на своего родителя — элемент body_

Обратиться к свойству можно через точку:

Скопировать кодJAVASCRIPT

```
const element = document.querySelector('p');

console.log(element.parentElement); // body, так как это родитель p 
```

## Псевдомассив детей — `children`

Свойство `children` содержит псевдомассив всех дочерних элементов указанного элемента. Псевдомассивы очень похожи на массивы, но при работе с ними есть ограничения. Подробнее о псевдомассивах расскажем в следующей теме.

![image](https://pictures.s3.yandex.net/resources/DOM_BOM_JS___1__200_1587288375.png)

_body содержит ссылки на все дочерние элементы_

Дочерние элементы `body`:

Скопировать кодJAVASCRIPT

```
/* script.js */

const body = document.querySelector('body');

console.log(body.children); // HTMLCollection(3) [p, p, p] 
```

## Первый и последний ребёнок — `firstElementChild` и `lastElementChild`

Свойства `firstElementChild` и `lastElementChild` позволяют получить первый и последний дочерние элементы:

![image](https://pictures.s3.yandex.net/resources/DOM_BOM_JS___1__13_1587288418.png)

_Элемент body содержит ссылки на первый и последний дочерний элемент_

Первый и последний ребёнок:

Скопировать кодJAVASCRIPT

```
/* script.js */

const body = document.querySelector('body');

console.log(body.firstElementChild); // <p>Ребёнок раз</p>
console.log(body.lastElementChild); // <p>Ребёнок три</p> 
```

Если у элемента нет дочерних элементов, `firstElementChild` и `lastElementChild` вернут `null`.

## Предыдущий и следующий сосед — `previousElementSibling` и `nextElementSibling`

Свойства `previousElementSibling` и `nextElementSibling` содержат ссылки на предыдущий и следующий соседний элементы:

![image](https://pictures.s3.yandex.net/resources/DOM_BOM_JS___1__14_1587288490.png)

_Свойства ссылаются на первый и последний элемент body_

Предыдущий и следующий сосед:

Скопировать кодJAVASCRIPT

```
/* script.js */

const element = document.querySelectorAll('p')[1];

console.log(element.previousElementSibling); // <p>Ребёнок раз</p>
console.log(element.nextElementSibling); // <p>Ребёнок три</p>

// если соседа нет — вернётся null
console.log(element.nextElementSibling.nextElementSibling); // null 
```

Все эти свойства доступны только для чтения. Перезаписать их не получится:

Скопировать кодJAVASCRIPT

```
/* script.js */

const body = document.querySelector('body');

console.log(body.children); // HTMLCollection(3) [p, p, p]
body.children = [];
console.log(body.children); // HTMLCollection(3) [p, p, p] 
```