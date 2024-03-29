[[Объектно-ориентированное программирование. Введение]]

До этого мы устанавливали пакеты: «Вебпак» и локальный сервер. Теперь нужно настроить их: объяснить правила, по которым «Вебпаку» предстоит собирать код.

Все эти правила прописывают в конфигурационном файле «Вебпака»: `webpack.config.js`. Создайте его в корне вашего проекта и откройте в редакторе кода.

Первым делом нужно создать объект `module.exports`. В него запишем все настройки:

Скопировать кодJAVASCRIPT

```
module.exports = {

}

// module.exports — это синтаксис экспорта в Node.js 
```

В прошлом уроке мы рассказывали о точке входа — файле, куда «Вебпак» заглядывает в первую очередь при сборке. Сначала нужно описать эту точку входа в `webpack.config.js`.

Точка входа — это объект `entry`. Ему нужно прописать путь к точке входа в свойстве `main`:

Скопировать кодJAVASCRIPT

```
module.exports = {
 entry: { main: './src/index.js' }
}

// указали первое место, куда заглянет webpack, — файл index.js в папке src 
```

Кроме точки входа есть точка выхода. Это итоговый файл, куда «Вебпак» сложит весь js-код. Её нужно указать в объекте `output`. У этого объекта 3 свойства: путь к точке выхода, имя файла, куда «Вебпак» положит код, и свойство для обновления путей внутри CSS- и HTML-файлов.

Скопировать кодJAVASCRIPT

```
module.exports = {
  entry: { main: './src/index.js' },
  output: {
    path: './dist/',
    filename: 'main.js',
        publicPath: ''
  }
}
// указали в какой файл будет собираться весь js и дали ему имя 
```

Есть одна трудность. «Вебпак» не понимает относительный путь для точки выхода. Поэтому в свойство `path` нужно обязательно записывать абсолютный путь, то есть путь от корневой папки.

Это можно сделать автоматически. В Node.js есть утилита, которая превращает относительный путь в абсолютный. Она называется `path`, а подключить его в файл можно функцией `require`:

Скопировать кодJAVASCRIPT

```
// webpack.config.js
const path = require('path'); // подключаем path к конфигу вебпак

module.exports = {
  entry: { main: './src/index.js' },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
        publicPath: ''
  }
}

// переписали точку выхода, используя утилиту path 
```

1.  Объявлена константа `path`. Она нужна, чтобы подключить к проекту новые методы для работы с путём.
2.  Вместо относительного пути, который мы указали в свойстве `path`, теперь стоит вызов метода `path.resolve`. Ему переданы два аргумента: ссылка на текущую папку `__dirname` и относительный путь к точке выхода.

Проверьте, всё ли работает. Сначала добавьте в `index.js` такой код:

Скопировать кодJAVASCRIPT

```
console.log('Hello, World!') 
```

Сейчас нужна финальная сборка, а не рабочая. Поэтому запустите сборку командой `npm run build`. Она соберёт весь код в точке выхода — файле `main.js`. Откройте файл `main.js` в редакторе кода. В нём должен появиться наш код:

Скопировать кодJAVASCRIPT

```
console.log('Hello, World!') 
```

Сборка настроена, но пока она работает только для JS-файлов. Перед тем как собирать вебпаком целый сайт, нужно настроить окружение разработки.

## Настраиваем окружение для разработки

Мы попробовали режим финальной сборки, но нужно настроить второй режим, который запускают командой `npm run dev` . Он нужен, чтобы упростить разработку, здесь будут дополнительные инструменты.

Чтобы начать эту настройку добавьте этот режим внутрь `module.exports`:

Скопировать кодJAVASCRIPT

```
const path = require('path'); // подключаем path к конфигу вебпак

module.exports = {
  entry: { main: './src/index.js' },
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'main.js',
        publicPath: ''
  },
  mode: 'development' // добавили режим разработчика
} 
```

Теперь добавим настройки локального сервера:

Скопировать кодJAVASCRIPT

```
module.exports = {
  // ... предыдущие настройки
  mode: 'development',
  devServer: {
    static: path.resolve(__dirname, './dist'), // путь, куда "смотрит" режим разработчика
    compress: true, // это ускорит загрузку в режиме разработки
    port: 8080, // порт, чтобы открывать сайт по адресу localhost:8080, но можно поменять порт

    open: true // сайт будет открываться сам при запуске npm run dev
  },
} 
```

Скоро вы обнаружите, что в режиме разработки сайт сам обновляется, когда вы сохраняете изменение в коде. Это его фишка, пусть это не будет для вас сюрпризом.

### Проверьте, что все работает:

1.  Запустите команду `npm run build`. Проверьте, что в папке `/dist` появился JS-код (скорее всего, с дополнительными комментариями и обёртками от «Вебпака»).

2.  Запустите команду `npm run dev` . Браузер с адресом `localhost:8080` откроется автоматически.