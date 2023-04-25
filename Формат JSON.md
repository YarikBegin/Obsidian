[[Продвинутый JavaScript]]

В прошлом уроке мы рассказывали о методе `fetch`: он позволяет отправить запрос на сервер из JavaScript-кода. В этом уроке расскажем, в каком виде от сервера приходит ответ, и что с этим делать.

После того, как мы отправили запрос, его должен обработать сервер и вернуть нам ответ. Логика работы сервера может быть описана на языках Java, Python или C++, а не JavaScript. Так что сервер не может вернуть нам данные внутри массива или объекта — этих структур просто нет у сервера.

Поэтому для передачи структурированных данных были разработаны специальные форматы. Самый распространённый из них — JSON**.**

JSON расшифровывается как JavaScript Object Notation.

Вот как выглядят данные в формате JSON:

Скопировать кодJSON

```
[
  {
    "title": "Вектора",
    "artist": "OZORA" 
  },
  {
    "title": "Страшно",
    "artist": "Shortparis"
  },
  {
    "title": "Ariadna",
    "artist": "Kedr Livansky"
  }
] 
```

Поскольку данные записаны в строго определённой форме, их можно преобразовать в строку и передать в HTTP-запросе. В этом и заключается смысл формата JSON: он позволяет собирать данные в объект, а затем преобразовывать этот объект в строку для передачи в запросе. Затем эту строку превращают обратно в объект.

Обратите внимание, что ключи взяты в двойные кавычки — в JSON это обязательно. Значениями ключей могут быть строки, числа, булевы значения, `null`, объекты и массивы:

Скопировать кодJSON

```
{
  "name": "Иван",
  "age": 30,
  "hasDog": true,
  "hasCat": false,
  "dog": {
    "name": "Бонни",
    "age": 6
  },
  "cat": null
} 
```

На верхнем уровне всегда должен быть объект или массив. JSON не может содержать функций, переменных или комментариев.

## Методы `JSON.stringify` и `JSON.parse`

Как мы уже сказали, объект JSON нужно преобразовывать в строку и обратно. Для этого есть специальные методы: `JSON.stringify` и `JSON.parse`.

Метод `JSON.stringify` делает из объекта строку JSON:

Скопировать кодJAVASCRIPT

```
const songs = [
  {
    title: 'Вектора',
    artist: 'OZORA' 
  },
  {
    title: 'Страшно',
    artist: 'Shortparis'
  },
  {
    title: 'Ariadna',
    artist: 'Kedr Livansky'
  }
];

const songsJSON = JSON.stringify(songs);

console.log(songsJSON);
console.log(typeof songsJSON); // "string" 
```

Метод `JSON.parse` делает обратное — преобразовывает JSON-строку в объект JavaScript:

Скопировать кодJAVASCRIPT

```
const songs = [
  {
    title: 'Вектора',
    artist: 'OZORA' 
  },
  {
    title: 'Страшно',
    artist: 'Shortparis'
  },
  {
    title: 'Ariadna',
    artist: 'Kedr Livansky'
  }
];

const songsJSON = JSON.stringify(songs);

console.log(typeof songsJSON); // "string"

const songsObject = JSON.parse(songsJSON);

console.log(typeof songsObject); // "object"
console.log(songsObject[0].title); // "Вектора" 
```

Методу `JSON.parse` нельзя передать какую угодно строку:

Скопировать кодJAVASCRIPT

```
JSON.parse('Привет'); // SyntaxError: Unexpected token П in JSON at position 0 
```

Строка должна быть JSON-совместимой, то есть:

-   на верхнем уровне должен быть объект или массив;
-   ключи и значения-строки должны быть в двойных кавычках;
-   кроме строк JSON может содержать числовые, булевы значения или `null`.

## Метод `res.json`

Метод `fetch` возвращает объект ответа:

Скопировать кодJAVASCRIPT

```
fetch('https://praktikum.yandex.ru')
  .then((res) => {
    console.log(res); // В консоли окажется объект ответа от сервера
  }); 
```

Мы познакомимся с ним подробнее, но сейчас нас интересует один его метод — `json`. Метод `json` читает ответ от сервера в формате `json` и возвращает промис_._ Из этого промиса потом можно доставать нужные нам данные.

Поскольку метод `json` асинхронный, то и использовать его нужно таким образом:

Скопировать кодJAVASCRIPT

```
fetch('https://praktikum.yandex.ru')
  .then((res) => {
    return res.json(); // возвращаем результат работы метода и идём в следующий then
  })
  .then((data) => {
    console.log(data.user.name); // если мы попали в этот then, data — это объект
  })
  .catch((err) => {
    console.log('Ошибка. Запрос не выполнен');
  }); 
```

### Важно

Ещё раз: `json` — асинхронный метод. Такой код некорректен:

Скопировать кодJAVASCRIPT

```
fetch('https://praktikum.yandex.ru')
  .then((res) => {
    const data = res.json();
    console.log(data.user.name); // data — это промис, она ещё не готова
  })
  .catch((err) => {
    console.log('Ошибка. Запрос не выполнен');
  }); 
```