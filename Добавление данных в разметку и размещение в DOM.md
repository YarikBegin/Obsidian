
[[ООП в интерфейсах. Введение]]

В предыдущем уроке мы подготовили класс к работе с динамическими данными и связали его с разметкой. Теперь добавим в разметку изображение и текст.

Вернёмся к чату:

![image](https://pictures.s3.yandex.net/resources/Untitled_1589708204.png)

_Пример карточки сообщения_

Сейчас разметка карточки выглядит так:

Скопировать кодHTML

```
<template class="card-template">
  <div class="card">
    <img src="" alt="Аватар пользователя" class="card__avatar">
    <div class="card__text">
      <p class="card__paragraph"><!-- здесь будет текст сообщения --></p>
    </div>
  </div>
</template> 
```

Разместим содержание сообщения в вёрстке. Изображение станет значением атрибута `src` элемента с классом `card__avatar`. Текст сообщения попадёт в блок `card__paragraph`. У каждого нового экземпляра класса `Card` — своё содержимое.

Подготовим в `index.js` массив объектов с изображением и текстом:

Скопировать кодJAVASCRIPT

```
const messageList = [
    {
        image: 'https://code.s3.yandex.net/web-code/card__image.jpg',
        text: 'Привет, нам срочно требуется доработать чат!'
    },
    {
        image: 'https://code.s3.yandex.net/web-code/card__image-lake.jpg',
        text: 'Теперь мы можем создавать сколько угодно карточек!'
    },
]; 
```

## Подготовка карточки к публикации

Новый метод `generateCard` подготовит карточку к публикации. Он добавит данные в разметку, а в следующих уроках научится управлять поведением карточек. Метод публичный, чтобы возвращать готовые карточки внешним функциям:

Скопировать кодJAVASCRIPT

```
_getTemplate() {
  const cardElement = document
    .querySelector('.card-template')
    .content
    .querySelector('.card')
    .cloneNode(true);

  return cardElement;
}

generateCard() {
  // Запишем разметку в приватное поле _element. 
  // Так у других элементов появится доступ к ней.
  this._element = this._getTemplate();

  // Добавим данные
  this._element.querySelector('.card__avatar').src = this._image;
  this._element.querySelector('.card__paragraph').textContent = this._text;

  // Вернём элемент наружу
  return this._element;
} 
```

Теперь цикл обойдёт массив `messageList` и для каждого его элемента:

-   создаст новый экземпляр `класса Card`,
-   подготовит карточку к публикации,
-   добавит новую карточку в DOM.

Скопировать кодJAVASCRIPT

```
// в начале файла index.js

const messageList = [{
    image: 'https://code.s3.yandex.net/web-code/card__image.jpg',
    text: 'Привет, нам срочно требуется доработать чат!'
  },
  {
    image: 'https://code.s3.yandex.net/web-code/card__image-lake.jpg',
    text: 'Теперь мы можем создавать сколько угодно карточек!'
  },
];

class Card {
  // код класса
}

messageList.forEach((item) => {
  // Создадим экземпляр карточки
  const card = new Card(item.text, item.image);
  // Создаём карточку и возвращаем наружу
  const cardElement = card.generateCard();

  // Добавляем в DOM
  document.body.append(cardElement);
}); 
```

Результат будет таким:

![image](https://pictures.s3.yandex.net/resources/Untitled_1_1589708228.png)

_Теперь чат такой_

Теперь легко создавать карточки: достаточно добавить новые данные в массив. Отдельный массив для данных, отдельный класс для их обработки — подходящая структура кода.

С итоговым кодом урока можно ознакомиться [по ссылке](https://repl.it/@praktikum/lesson-2). Для запуска нажмите ▶.