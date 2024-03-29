[[JavaScript]]

В предыдущем уроке мы рассказывали, как взаимодействовать с базой данных из графического интерфейса. Но документы в БД создаёт сервер, а не человек.

Нужно как-то «подружить» сервер и базу данных. Наш сервер запрограммирован на JavaScript, который не умеет работать с документами. Зато в JS есть другие структуры данных — объекты. Наша задача: научить JavaScript работать с документами как с объектами.

Чтобы подружить JS с документами, существуют специальные инструменты — ODM, или Object Document Mapper (англ. «сопоставитель объектов и документов»). У каждой БД есть свой сопоставитель. У MongoDB он называется Mongoose и представляет собой мост между двумя мирами: миром документов в базе данных и миром объектов JavaScript.

### Устанавливаем Mongoose

Это делается через `npm`. Откройте в терминале папку с проектом и запустите команду:

Скопировать кодBASH

```
npm i mongoose 
```

Теперь его можно импортировать как модуль и пользоваться.

### Подключаемся к серверу mongo

Эту задачу выполняет метод mongoose.connect:

Скопировать кодJAVASCRIPT

```
// app.js — входной файл

const express = require('express');
const mongoose = require('mongoose');

const app = express();

// подключаемся к серверу mongo
mongoose.connect('mongodb://localhost:27017/mydb', {
  useNewUrlParser: true,
  useCreateIndex: true,
    useFindAndModify: false
});

// подключаем мидлвары, роуты и всё остальное...

app.listen(3000); 
```

Этот метод принимает на вход адрес сервера базы данных.

Адрес состоит из двух частей. Первая — `mongodb://localhost:27017` — адрес сервера mongo по умолчанию. Он запускается на localhost на 27017 порту. Вторая часть — `mydb` — имя базы данных.

### Запускаем сервер MongoDB

Сервер mongo запускают командой `mongod`:

Скопировать кодBASH

```
mongod 
```

Это нужно делать до запуска Node.js приложения. Иначе оно не сможет подключиться к базе данных и взаимодействовать с ней.