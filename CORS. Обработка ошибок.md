[[JavaScript]]

Сейчас наше приложение отлично работает на локальном сервере и по умолчанию оно готово принимать запросы с любых страниц.

В учёбе это не принципиально, но в реальных проектах — небезопасно. Чтобы это исправить, нужно задать заголовки для поддерживаемых сайтов с безопасными запросами. Подробнее мы рассматривали [CORS в предыдущем спринте](https://praktikum.yandex.ru/learn/web/courses/4da123a7-d4aa-4bc2-b299-b48f921da09c/sprints/2798/topics/5dc7307b-8486-40a5-b418-0888ac73337e/lessons/8f98665e-1efd-4a6c-baed-10f7ec7891b7/).

Чтобы сделать приложение более безопасным, в express ему добавляют мидлвэр:

Скопировать кодJAVASCRIPT

```
app.use(function(req, res, next) {
  res.header('Access-Control-Allow-Origin', 'https://praktikum.tk');
  next();
}); 
```

В этом же мидлвэре настраивают разрешённые заголовки и методы:

Скопировать кодJAVASCRIPT

```
app.use(function(req, res, next) {
  res.header('Access-Control-Allow-Origin', 'https://praktikum.tk');
  res.header('Access-Control-Allow-Headers', 'Origin, X-Requested-With, Content-Type, Accept');
  res.header('Access-Control-Allow-Methods', 'GET,HEAD,PUT,PATCH,POST,DELETE');

  next();
}); 
```

### Как разрешить запросы с нескольких ресурсов

В заголовок Access-Control-Allow-Origin можно записать либо один URL, либо сразу все. Иногда нужно разрешить кросс-доменные запросы с нескольких ресурсов, но не всех. Например, с локального сервера и продакшн-сайта.

У любого запроса есть заголовок Origin. Он содержит адрес, с которого идёт этот запрос. Мы можем создать массив разрешённых доменов, а затем проверять, есть ли среди них тот, что указан в заголовке Origin.

Если домен найден среди разрешённых, просто переписываем значение Origin в заголовок ответа Access-Control-Allow-Origin:

Скопировать кодJAVASCRIPT

```
// Массив разешённых доменов
const allowedCors = [
  'https://praktikum.tk',
  'http://praktikum.tk',
  'localhost:3000'
];

app.use(function(req, res, next) {
  const { origin } = req.headers; // Записываем в переменную origin соответствующий заголовок

  if (allowedCors.includes(origin)) { // Проверяем, что значение origin есть среди разрешённых доменов
    res.header('Access-Control-Allow-Origin', origin);
  }

  next();
}); 
```

Защита от кросс-доменных запросов — это хорошо. Но ещё лучше уметь этой защитой управлять.