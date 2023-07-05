[[JavaScript]]

В предыдущем уроке мы написали код контроллера логина:

Скопировать кодJAVASCRIPT

```
// controllers/users.js

module.exports.login = (req, res) => {
  const { email, password } = req.body;

  User.findOne({ email })
    .then((user) => {
      if (!user) {
        return Promise.reject(new Error('Неправильные почта или пароль'));
      }

      return bcrypt.compare(password, user.password);
    })
    .then((matched) => {
      if (!matched) {
        // хеши не совпали — отклоняем промис
        return Promise.reject(new Error('Неправильные почта или пароль'));
      }

      // аутентификация успешна
      res.send({ message: 'Всё верно!' });
    })
    .catch((err) => {
      res
        .status(401)
        .send({ message: err.message });
    });
}; 
```

Эта цепочка промисов работает так:

- проверяет, есть ли в базе пользователь с указанной почтой;
- если пользователь найден, сверяет хеш его пароля;

В этом уроке мы улучшим код: сделаем код проверки почты и пароля частью схемы User. Для этого напишем метод `findUserByCredentials`, который принимает на вход два параметра — почту и пароль — и возвращает объект пользователя или ошибку.

Сделать это позволяет Mongoose. Чтобы добавить собственный метод, запишем его в свойство `statics` нужной схемы:

Скопировать кодJAVASCRIPT

```
// models/user.js

const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 8
  }
});

// добавим метод findUserByCredentials схеме пользователя
// у него будет два параметра — почта и пароль
userSchema.statics.findUserByCredentials = function (email, password) {

};

module.exports = mongoose.model('user', userSchema); 
```

Осталось написать код метода. В будущем мы хотим использовать метод вот так:

Скопировать кодJAVASCRIPT

```
User.findUserByCredentials('stasbasov@yandex.ru', 'StasBasov1989')
  .then(user => {
    // получаем объект пользователя, если почта и пароль подошли
  })
  .catch(err => {
    // получаем ошибку, если нет
  }); 
```

### Описываем метод `findUserByCredentials`

Чтобы найти пользователя по почте, нам потребуется метод `findOne`, которому передадим на вход `email`. Метод `findOne` принадлежит модели `User`, поэтому обратимся к нему через ключевое слово `this`:

Скопировать кодJAVASCRIPT

```
// models/user.js

userSchema.statics.findUserByCredentials = function (email, password) {
  // попытаемся найти пользователя по почте
  return this.findOne({ email }) // this — это модель User
    .then((user) => {
      // не нашёлся — отклоняем промис
      if (!user) {
        return Promise.reject(new Error('Неправильные почта или пароль'));
      }

      // нашёлся — сравниваем хеши
      return bcrypt.compare(password, user.password);
    });
};

module.exports = mongoose.model('user', userSchema); 
```

Функция `findUserByCredentials` не должна быть стрелочной. Это сделано, чтобы мы могли пользоваться `this`. Иначе оно было бы задано статически, ведь стрелочные функции запоминают значение `this` при объявлении.

Осталось добавить обработку ошибки, когда хеши не совпадают. Опишем этот код в ещё одном обработчике `then`.

Сначала разберём, как делать не стоит:

Скопировать кодJAVASCRIPT

```
// models/user.js

userSchema.statics.findUserByCredentials = function (email, password) {
  // попытаемся найти пользователя по почте
  return this.findOne({ email })
    .then((user) => {
      // не нашёлся — отклоняем промис
      if (!user) {
        return Promise.reject(new Error('Неправильные почта или пароль'));
      }

      // нашёлся — сравниваем хеши
      return bcrypt.compare(password, user.password);
    })
    .then((matched) => {
      if (!matched) // отклоняем промис
      
      return user; // но переменной user нет в этой области видимости
    });
};

module.exports = mongoose.model('user', userSchema); 
```

Здесь во втором `then` мы обращаемся к объекту `user`, которого нет в этой области видимости. Он остался в предыдущем `then`.

Чтобы решить эту проблему, следует организовать цепочку промисов иначе: добавить обработчик `then` вызову метода `bcrypt.compare`:

Скопировать кодJAVASCRIPT

```
// models/user.js

const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  email: {
    type: String,
    required: true,
    unique: true
  },
  password: {
    type: String,
    required: true,
    minlength: 8
  }
});

userSchema.statics.findUserByCredentials = function (email, password) {
  return this.findOne({ email })
    .then((user) => {
      if (!user) {
        return Promise.reject(new Error('Неправильные почта или пароль'));
      }

      return bcrypt.compare(password, user.password)
        .then((matched) => {
          if (!matched) {
            return Promise.reject(new Error('Неправильные почта или пароль'));
          }

          return user; // теперь user доступен
        });
    });
};

module.exports = mongoose.model('user', userSchema); 
```

Код метода готов. Теперь мы можем применить его в обработчике аутентификации:

Скопировать кодJAVASCRIPT

```
// controllers/users.js

module.exports.login = (req, res) => {
  const { email, password } = req.body;

  return User.findUserByCredentials(email, password)
    .then((user) => {
      // аутентификация успешна! пользователь в переменной user
    })
    .catch((err) => {
      // ошибка аутентификации
      res
        .status(401)
        .send({ message: err.message });
    });
}; 
```

Теперь нужно сделать так, чтобы система запоминала пользователя и ему не приходилось вводить данные каждый раз, когда он открывает приложение. Как это сделать — разберём в следующем уроке.

О собственных методах схем в документации Mongoose: [https://mongoosejs.com/docs/guide.html#statics](https://mongoosejs.com/docs/guide.html#statics).