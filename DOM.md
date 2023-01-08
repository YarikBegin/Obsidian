DOM расшифровывается как Document Object Model. Фактически это и есть HTML-разметка: блоки, из которых состоит документ.

## DOM: выбор элементов

JavaScript не умеет работать напрямую с HTML. Чтобы их подружить, придумали DOM — интерфейс, позволяющий JS работать с разметкой документа. Когда вы открываете веб-страницу, браузерный движок переводит её содержимое в понятный для JavaScript вид — создаёт DOM-дерево.

Структура этого дерева такая же, как у разметки самого документа. Если в HTML-коде одни теги вкладываются в другие, то же самое происходит в DOM: одни элементы становятся дочерними по отношению к другим.

Чтобы посмотреть на DOM, откройте в инструментах разработчика вкладку ”Elements“. То, что вы увидите, очень похоже на разметку веб-страницы. Но это не совсем так: на самом деле вы смотрите не на теги, а на элементы DOM-дерева. Отсюда и название вкладки.

Разберёмся, как устроено DOM-дерево. Возьмём веб-страницу с несложной разметкой:

Скопировать кодHTML
```html
<html>
    <head>
    </head>
    <body>
      <nav>
        <a>к основной части</a>
        <a>к футеру</a>
      </nav>
      <main>
      <div></div>
          <h1>заголовок</h1>
      <div>
            <h2>подзаголовок</h2>
            <p>текст</p>
            <p></p>
            <p>текст параграфа</p>
          </div>
          <p>текст параграфа</p>
      </main>
      <footer>
          copyright
      <a>
            praktikum.yandex.ru
                <!-- Каждый может стать -->
          </a>
          <a>yandex.ru</a>
      </footer>
    </body>
</html>
```

Любое ветвление дерева называют узлом. Он возникает там, где есть:

-   открывающий тег;
-   текст;
-   HTML-комментарий.

Воспользуемся этой логикой и построим DOM-дерево:

![[Image.png]]

![[Image (1).png]]

![[Image (2).png]]

![[Image (3).png]]

Обратите внимание: узел — более ёмкое понятие, чем элемент. Элемент — то, что мы называем тегом: он может быть парным или одиночным.

-   `<img>` или `<a></a>` — элемент.
-   Текст `<a>находящийся внутри ссылки</a>` — текстовый узел.
-   Набор тегов `<a></a>` — узел.

Узел — это все сущности, которые мы можем увидеть на странице, в том числе и элементы. Текст, комментарии, теги — это узлы.

Получается, что все элементы — узлы, но не все узлы — элементы. Например, HTML-комментарий — узел, но не элемент. Дальше мы будем говорить только об элементах веб-страницы.

## Поиск элементов в DOM-дереве

Научимся искать элементы в DOM-дереве. Будем работать с методами: `querySelector` и `querySelectorAll`.

Оба метода принимают на вход строку — селектор нужного элемента. Удобно, что аргументы нужно писать так же, как в CSS:

```html
<!-- index.html -->

<main id="container">
  <div class="content">
    <div class="content__item"></div>
    <div class="content__item"></div>
    <div class="content__item"></div>
  </div>
</main>
```

```js
/* script.js */

// Для выбора элемента по идентификатору
let container = document.querySelector('#container');

// По имени класса
let content = container.querySelector('.content');

/* Обратите внимание: мы ищем элементы не по всему
DOM-дереву, а внутри уже найденного элемента */
let contentItems = content.querySelectorAll('.content__item');
```
Если передать класс как аргумент методу `querySelector`_,_ метод вернёт первый на странице элемент с этим классом (тот, что выше в вёрстке).

`querySelectorAll` вернёт все подходящие под селектор элементы.
```html
<!-- index.html -->

<main id="container">
  <div class="content">
    <div class="content__item"></div>
    <div class="content__item"></div>
    <div class="content__item"></div>
  </div>
</main>
```

```js
/* script.js */

let container = document.querySelector('#container');
let content = container.querySelector('.content');
let contentItem = content.querySelector('.content__item');

console.log(contentItem) // Выведет: <div class="content__item"></div>

let contentItems = content.querySelectorAll('.content__item');

console.log(contentItems); /* Выведет все элементы c классом .content__item */
```
Главное преимущество этих методов — возможность искать элементы по сложным составным селекторам:

```html
<!-- index.html -->

<section class="bag">
  <div class="item">
    Очешник
        <p>Очки</p>
    </div>
  <p class="item">Расчёска</p>
  <p class="item">Зеркальце</p>
  <div class="item bag">Косметичка
    <p class="item">Помада</p>
    <p class="item">Тушь</p>
  </div>
  <p class="item wallet">Бумажник</div>
</section>
```

```js
/* script.js */

let cosmeticBagContent = document.querySelectorAll('section.bag div.bag .item');
console.log(cosmeticBagContent); // Будут выбраны помада и тушь
```

## Другие способы поиска элементов

Инструменты `querySelector` и `querySelectorAll` поддерживаются браузерами относительно недавно. Раньше для поиска элементов в DOM-дереве использовались методы:

-   `getElementById` (получить элемент по идентификатору);
-   `getElementsByClassName` (получить элементы по имени класса);
-   `getElementsByTagName` (получить элементы по имени тега).

Главное отличие от методов `querySelector` и `querySelectorAll` в том, что getElementBy... не поддерживают составные селекторы. Имена идентификаторов в `getElementById` и имена классов в `getElementsByClassName` указываются без символов `#` и `.` в начале:

```js
// Получаем элемент по идентификатору через querySelector
let container1 = document.querySelector('#container');

// Получаем элемент по идентификатору через getElementById
let container2 = document.getElementById('container');

// Проверяем элементы на идентичность
console.log(container1 === container2); // true - элементы идентичны
```
Мы не будем пользоваться этими методами, так как `querySelector` и `querySelectorAll` более гибкие и предоставляют больше возможностей.

После того как элементы выбраны, можно переходить к управлению ими. О том, как это делается, расскажем дальше.