
[[React Router]]

В этом уроке вы научитесь устанавливать и настраивать React Router в проекте.

Создайте новый проект с помощью CRA. Для этого в командной строке введите `npx create-react-app emoji-critic`. Это будет приложение для любителей эмодзи:

После установки внутри `src/` создайте папку с именем `components/` и переместите в неё `App.js` со всеми сопутствующими файлами. Также удалите из проекта всё, что было добавлено по умолчанию. В итоге `App.js` должен выглядеть так:

Скопировать кодJAVASCRIPT

```
// App.js

import React from 'react';
// Добавили компонент Header с логотипом проекта
import Header from './Header';
import './App.css';

function App() {
  return (
    <div className="App">
      <Header />
    </div>
  );
}

export default App; 
```

Внутри директории `components/` создайте новый компонент с именем `Dashboard`. Это будет простой функциональный компонент, который возвращает JSX с текстом:

Скопировать кодJAVASCRIPT

```
// Dashboard.js

import React from 'react';
import './Dashboard.css';

function Dashboard () {
  return (
    <div className="dashboard">
      <h2>Emoji Critic — всё об эмодзи</h2>
      <p>
        #1 среди авторов обзоров на эмодзи в этом году!
      </p>
    </div>
  )
}

export default Dashboard; 
```

## Добавление React Router в проект

Чтобы установить библиотеку React Router, откройте главную директорию проекта и введите `npm i react-router-dom@6.3.0`. Эта версия React Router предназначена для маршрутизации в браузерных приложениях.

Работа с React Router всегда начинается с `BrowserRouter`. Компонент `BrowserRouter` отслеживает историю навигации в процессе работы React Router. Когда пользователь переходит назад или вперёд в браузере, `BrowserRouter` синхронизирует отображаемый контент.

В файле `index.js`, который расположен внутри каталога `src/`, оберните основной компонент `App` в компонент `BrowserRouter`:

Скопировать кодJAVASCRIPT

```
// index.js

import React from 'react';
import ReactDOM from 'react-dom';
import { BrowserRouter } from 'react-router-dom'; // импортируем BrowserRouter
import App from './components/App';
import './index.css';

// теперь обернём компонент App в BrowserRouter
ReactDOM.render(
  <React.StrictMode>
    <BrowserRouter> 
      <App />
    </BrowserRouter>
  </React.StrictMode>,
  document.getElementById('root')
); 
```

## Создание первого компонента `Route`

Компонент `Route` — главный строительный блок. Чтобы он работал верно, его необходимо поместить в компонент `Routes`.

Импортируйте компоненты `Routes` в файл `App.js`.

Скопировать кодJAVASCRIPT

```
// App.js

import React from 'react';
import { Routes } from 'react-router-dom'; // импортируем Routes
import Dashboard from './Dashboard';
import Header from './Header';
import './App.css';

function App() {
  return (
    <div className="App">
      <Header />
      <Routes>
                // Тут будут наши роуты
      </Routes>
    </div>
  );
}

export default App; 
```

Приложение должно отображать компонент `Dashboard`, когда URL в адресной строке указывает на `localhost:3000/`. Для этого импортируйте компоненты `Route` и `Dashboard` в файл `App.js`. В пропс `element` компонента `Route` передайте компонент `Dashboard`. Он будет отрисовываться при каждом обращении к URL-пути, который передан пропсу `path`:

Скопировать кодJAVASCRIPT

```
// App.js

import React from 'react';
import { Routes, Route } from 'react-router-dom';
import Dashboard from './Dashboard';
import Header from './Header';
import './App.css';

function App() {
  return (
    <div className="App">
      <Header />
      <Routes>
          <Route path="/" element={<Dashboard />} />
      </Routes>
    </div>
  );
}

export default App; 
```

В нашем примере `Dashboard` будет отрисован при каждом обращении к корневому URL `/`.

Убедимся, что всё работает. Запустите проект командой `npm run start` и откройте в браузере `localhost:3000/`. Вы увидите, что заголовок Emoji Critic правильно отображается между тегами `<h1>`, а `Dashboard` отрисован именно там, где мы разместили компонент `Route` в JSX-коде.

## Всё хорошо, что хорошо маршрутизируется

Чтобы использовать React Router в проекте, нужно импортировать необходимые компоненты.

Если хотите добавить `Route` в приложение, убедитесь, что он обёрнут в компоненты `Routes` и `BrowserRouter`. Компонент `Route` устанавливает связь между путём, который указан в пропсе `path`, и URL-адресом, который в данный момент используется браузером. При каждом обращении к этому URL будет отображаться компонент, переданный в пропс `element`.

Одного маршрута недостаточно, чтобы почувствовать преимущества библиотеки React Router. В следующем уроке научимся добавлять несколько маршрутов.