[[Продвинутый JavaScript]]

Рассмотрим ещё один пример, [который мы приводили](https://praktikum.yandex.ru/trainer/web/lesson/213a1fd1-cf8f-4806-97fb-34fa03351a15) до того, как познакомились с классами:

Скопировать кодJAVASCRIPT

```
// функция createSong создаёт объекты
function createSong(title, artist) {
  const newSong = {
    title,
    artist,
    isLiked: false,
    // каждый из этих объектов хранит функцию like
    like: function () {
      newSong.isLiked = !newSong.isLiked;
    }
  };

  return newSong;
}

// можно создать несколько песен одной функцией
const song1 = createSong('Футбольный мяч', 'Антоха MC');
const song2 = createSong('На заре', 'Альянс');
const song3 = createSong('Ай', 'Хаски'); 
```

Мы уже упоминали, что в этом примере есть проблема. Функция `like` делает для каждого объекта одно и то же, но хранится как свойство каждой отдельной песни. Бестолку множить сущности не стоит, поэтому функцию вынесем в отдельный объект — прототип. Затем привяжем все песни к этому прототипу.

Скопировать кодJAVASCRIPT

```
// этот объект должен стать прототипом объектов песен
const songPrototype = {
  like: function () {
    newSong.isLiked = !newSong.isLiked;
  }
}; 
```

Правда теперь переменной `newSong` у нас нет. Чтобы функция `like` стала универсальной заменим `newSong` в её теле на `this`:

Скопировать кодJAVASCRIPT

```
const songPrototype = {
  like: function () {
    this.isLiked = !this.isLiked;
  }
}; 
```

Осталось сделать этот объект прототипом песни. Один из способов сделать это — метод `Object.create`.

## Метод `Object.create`

Метод `Object.create` создаёт новый пустой объект. Первый аргумент метода — обязательный — объект, который должен стать прототипом нового объекта:

Скопировать кодJAVASCRIPT

```
// это будет прототип объекта-песни
const songPrototype = {
  like: function () {
    this.isLiked = !this.isLiked;
  }
};

// создаём пустой объект с указанным прототипом
const newSong = Object.create(songPrototype);

console.log(newSong); // пустой объект 
```

Скопируйте этот код в консоль — там появится пустой объект:

![image](https://pictures.s3.yandex.net/resources/sprint_4-45_1592649526.png)

И откройте свойство `__proto__`, кликнув по нему:

![image](https://pictures.s3.yandex.net/resources/sprint_4-46_1592649547.png)

Теперь функция `like` «живёт» в прототипе объекта `newSong`.

Добавим объекту `newSong` нужные свойства:

Скопировать кодJAVASCRIPT

```
// прототип объекта newSong
const songPrototype = {
  like: function () {
    this.isLiked = !this.isLiked;
  }
};

// создаём пустой объект с прототипом
const newSong = Object.create(songPrototype);

// добавляем нужные свойства в объект
newSong.title = 'Футбольный мяч';
newSong.artist = 'Антоха MC';
newSong.isLiked = false;

console.log(newSong); // теперь данные хранятся в объекте newSong, а функциональность в прототипе 
```

Логика понятна, теперь перенесём её в функцию `createSong`:

Скопировать кодJAVASCRIPT

```
// прототип объекта newSong
const songPrototype = {
  like: function () {
    this.isLiked = !this.isLiked;
  }
};

function createSong(title, artist) {
  // создаём пустой объект с прототипом
  const newSong = Object.create(songPrototype);

  // добавляем нужные свойства в объект
  newSong.title = title;
  newSong.artist = artist;
  newSong.isLiked = false;

  // возвращаем объект песни
  return newSong;
}

const song1 = createSong('Футбольный мяч', 'Антоха MC');
const song2 = createSong('На заре', 'Альянс');
const song3 = createSong('Ай', 'Хаски');

console.log(song1); // { title: 'Футбольный мяч' ... }
console.log(song2); // { title: 'На заре' ... }
console.log(song1.like === song2.like); // true 
```

Скопируйте этот код в консоль и исследуйте содержимое объектов `song1` и `song2`. Как видите эти объекты содержат все нужные свойства, но функция `like` теперь в объекте по ссылке `__proto__`. Этой ссылкой все три песни ссылаются на `songPrototype`. Код больше не дублируется.

## Резюме

Метод `Object.create` создаёт новый пустой объект с указанным прототипом:

Скопировать кодJAVASCRIPT

```
const objPrototype = {
  foo: function() {
    console.log('foo');
  }
};

const obj = Object.create(objPrototype);

console.log(obj); // {} — пустой объект
console.log(obj.__proto__); // { foo: ƒ } — прототип 
```

После создания объекта таким образом, ему можно добавить свойства или методы:

Скопировать кодJAVASCRIPT

```
const objPrototype = {
  foo: function() {
    console.log('foo');
  }
};

const obj = Object.create(objPrototype);

obj.bar = 'bar';

console.log(obj); // { bar: "bar" } — появилось свойство bar
console.log(obj.__proto__); // { foo: ƒ } 
```