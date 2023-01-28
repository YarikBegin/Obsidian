[[Массивы, создание, доступ по индексу, длина массива]]

Отбор элементов массива: метод filter

Метод `filter` отсеивает элементы массива по заданному признаку. Как `forEach` и `map`, метод `filter` принимает колбэк в качестве аргумента. Этот колбэк будет вызван на каждом элементе. Он должен вернуть `true` или `false` в зависимости от того, хотим мы оставить текущий элемент массива или отфильтровать:

Скопировать кодJAVASCRIPT

```
const a = [1, 9, 2, 2, 3, 4, 1, 7, 8, 0, 9, 0, 1, 5, 3];

// отберём элементы больше 5
const b = a.filter(function (item) {
  return item > 5
});

console.log(b); // [9, 7, 8, 9] 
```

Метод `filter` создаёт новый массив из элементов, для которых функция-фильтр вернула `true`. При этом исходный массив не меняется.

Колбэк метода `filter` — те же три параметра, что и у `map` и `forEach`. Текущий элемент, его индекс и исходный массив:

Скопировать кодJAVASCRIPT

```
const a = [1, 9, 2, 2, 3, 4, 1, 7, 8, 0, 9, 0, 1, 5, 3];

const b = a.filter(function (item, position, array) {
  return array.lastIndexOf(item) === position; // вернём уникальные элементы
});

console.log(a); // [1, 9, 2, 2, 3, 4, 1, 7, 8, 0, 9, 0, 1, 5, 3]
console.log(b); // [2, 4, 7, 8, 9, 0, 1, 5, 3] 
```

Так же как и методы `map` и `forEach`, метод `filter` можно использовать для работы с данными, которые потом отрисовываются в DOM. Вернёмся к твитам — вы можете найти твиты по определённым параметрам и, применяя метод `forEach`, добавить их в DOM:

Скопировать кодJAVASCRIPT

```
const tweets = [
    {
        user: '@elonmusk',
        date: '16 марта 2019 года',
        text: "I'm from South Africa."
    },
    {
        user: '@realDonaldTrump',
        date: '24 марта 2019 года',
        text: 'Good Morning, Have A Great Day!'
    },
    {
        user: '@BillGates',
        date: '24 марта 2019 года',
        text: 'I never ate apple in my life'
    }
];

const filteredTweets = tweets.filter(function (item) => {
    return item.text.length > 25;
});

filteredTweets.forEach((item) => {
    document.body.append(addTweet(item));
}); 
```

![image](https://pictures.s3.yandex.net/resources/Untitled_1607698863.png)

Переходите к заданиям и отработайте метод `filter`.