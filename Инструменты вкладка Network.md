[[Продвинутый JavaScript]]

Вы научились посылать запросы, получать ответы, читать их тела и заголовки. Но как анализировать полную информацию о запросах и ответах? Неужели всякий раз вставлять `console.log`?

Скопировать кодJAVASCRIPT

```
fetch('https://api.kanye.rest')
    .then(res => {
        console.log(res.status); // какой у ответа статус?
        console.log(res.headers.get('Content-Type')); // а Content-Type какой?
    return res.json();
  })
  .then((data) => {
    console.log(data); // а что пришло в теле?
  }); 
```

Конечно, можно и так, но неудобно. А чтобы было удобно, нужен инструмент дебаггинга. К счастью, он есть: вкладка Network в инструментах разработчика представляет собой средство анализа запросов и ответов.

### Что такое сетевой мониторинг

Network Monitor — инструмент для слежки за запросами, которые происходят в рамках сессии работы с сайтом.

Мы в примерах используем [Яндекс.Браузер](https://browser.yandex.ru/). Если вы пользуетесь Firefox или Opera, команды и вкладки могут отличаться.

### Как открыть вкладку Network

Для этого нужно открыть инструмент разработчика сочетанием клавиш Command+Option+I (macOS) или Control+Shift+I (Windows, Linux). Таким сочетанием клавиш вы попадёте сразу в вкладку Network. Но если этого не случилось, найдите вкладку Network и перейдите на неё.

Вы увидите такой экран:

![image](https://pictures.s3.yandex.net/resources/sprint_9-21_1592644531.png)

Рассмотрим, что расположено на основном экране. Вкладка Network состоит из трёх основных частей. Перезагрузите страницу с открытым инструментом:

![image](https://pictures.s3.yandex.net/resources/sprint_9-22_1592644589.png)

Перед вами все HTTP-запросы, которые делает страница. Здесь есть запросы HTML-документов, CSS- и JS-файлов, картинок, а также fetch-запросы из JavaScript.

## Фильтрация запросов

В таком количестве запросов сложно найти тот, что нас интересует. Но запросы можно фильтровать по типу. Для этого на выделенной панели нужно кликнуть на тип запроса:

![image](https://pictures.s3.yandex.net/resources/sprint_9-25_1592644609.png)

Например, если кликнуть на «JS», браузер отобразит только запросы JavaScript файлов:

![image](https://pictures.s3.yandex.net/resources/sprint_9-23_1592644627.png)

Нас в основном будет интересовать вкладка XHR_._ Именно там появляются запросы сделанные методом `fetch` из JavaScript кода.

## Внутренности запроса

Откройте инструменты разработчика в новом окне, иначе запрос не сработает. Затем скопируйте в консоль такой код и нажмите Enter:

Скопировать кодJAVASCRIPT

```
fetch('https://api.kanye.rest'); 
```

Затем:

-   перейдите во вкладку Network;
-   отфильтруйте XHR запросы;
-   найдите в списке запрос [api.kanye.rest](http://api.kanye.rest/) и кликните на него.

Вы увидите внутренности запроса:

![image](https://pictures.s3.yandex.net/resources/sprint_9-44_1592644653.png)

Справа основная информация о запросе (его URL, метод, статус ответа), а также заголовки запроса и ответа:

![image](https://pictures.s3.yandex.net/resources/sprint_9-43_1592644676.png)

Кликнув на вкладку «Preview», вы увидите тело запроса:

![image](https://pictures.s3.yandex.net/resources/sprint_9-42_1592644698.png)

Часто в теле ответа приходит JSON с множеством разных полей. Вкладка «Preview» позволяет быстро проанализировать структуру и найти нужную информацию.

Во вкладке Network множество инструментов. С их помощью можно:

-   анализировать внутренности запросов;
-   смотреть, какие файлы приходят с сервера и как они влияют на время загрузки сайта;
-   включать режим «медленного интернета» и наблюдать, как сайт загружается.

В этом уроке вы познакомились только с первым пунктом — это то, что понадобится при выполнении практической работы. С остальными инструментами мы будем знакомиться по ходу, но если вам не терпится — добро пожаловать в дополнительные материалы темы.