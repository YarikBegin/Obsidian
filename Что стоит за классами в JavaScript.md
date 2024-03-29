[[Продвинутый JavaScript]]

Сейчас наша функция-конструктор выглядит так:

Скопировать кодJAVASCRIPT

```
// создаём функцию-конструктор
function Song(title, artist) {
  // в её теле модифицируем this
  this.title = title;
  this.artist = artist;
  this.isLiked = false;
}

// добавляем метод в прототип
Song.prototype.like = function () {
  this.isLiked = !this.isLiked;
};

// создаём объект
const song1 = new Song('Футбольный мяч', 'Антоха MC'); 
```

Вам ничего не напоминает это код?

Да. Приведённая функция-конструктор делает то же самое, что и такой класс:

Скопировать кодJAVASCRIPT

```
// создаём класс Song
class Song {
  constructor(title, artist) {
    // в конструкторе класса модифицируем this
    this.title = title;
    this.artist = artist;
    this.isLiked = false;
  }

    // добавляем классу методы, они попадут в прототип объекта
  // который будет создан таким классом
  like() {
    this.isLiked = !this.isLiked;
  }
}

// создаём объект классом Song
const song1 = new Song('Футбольный мяч', 'Антоха MC'); 
```

Вот и раскрыт главный секрет классов в JS.

> Класс — это не какая-то отдельная сущность или тип данных. Это просто более удобная запись функции-конструктора.

До 2015 года в JS не было классов и все пользовались только функциями-конструкторами. Но разработчики спецификации решили, что неудобно каждый раз писать такие функции и записывать методы в их свойство `prototype`. Так был придуман синтаксис классов.

Именно синтаксис, так как классы не добавляют новых возможностей в язык. Когда мы создаём класс, на самом деле, мы:

-   создаём функцию-конструктор;
-   добавляем методы в её свойство `prototype`.

![image](https://pictures.s3.yandex.net/resources/JS4_5___1__4_1592650137.jpg)

Просто синтаксис классов позволяет сделать это в более удобной форме. Так действительно удобнее, но не забывайте, как это работает внутри. На собеседовании обязательно спросят.