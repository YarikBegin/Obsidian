[[JavaScript]]

Кеширование позволяет сохранить определённые данные на клиенте или сервере, чтобы не отвечать на один и тот же запрос несколько раз. Это ускоряет работу приложений и взаимодействие сервера с клиентом.

GET-запросы кешируются браузерами по умолчанию. POST-запросы, наоборот, по умолчанию не кешируются, а PUT, PATCH и DELETE не кешируются вообще.

За логику кеширования отвечают специальные заголовки GET и POST запросов, которые указываются на сервере. Об этих заголовках и расскажем.

### Кеширующие заголовки

**Expires**

Данные могут устаревать. Заголовок Expires позволяет настроить, как долго закешированные данные будут актуальны. После того, как этот период истечёт, данные будут запрошены с сервера, а не взяты из кеша. Сам кеш при этом обновится:

Скопировать код

```
Expires: Fri, 20 May 2016 19:20:49 IST 
```

**Cache-Control**

В этом заголовке указывают одну из директив для настройки обращения с закешированными данными. Вот наиболее используемые:

Скопировать код

```
// управление кешированием

Cache-Control: only-if-cached // если ответ в кеше, взять его оттуда, не отправляя запрос
Cache-Control: no-cache // кеш сразу становится просроченным, а затем данные ответа в кеше проверяются

// управление временем жизни

Cache-Control: max-age=30000 // максимальное время в секундах, в течение которого ресурс считается актуальным

~~~~// управление обновлением кеша

Cache-Control: must-revalidate // если кеш просроченый, будет отправлен запрос на проверку данных в кеше

// другое

Cache-Control: no-store // кеш отключён 
```

Данные от клиента к серверу идут не напрямую, а через другие машины — прокси-серверы. На этих промежуточных машинах данные тоже могут быть закешированы. Этим также можно управлять, установив заголовок Cache-Control:

Скопировать код

```
Cache-Control: private // данные могут быть закешированы только на клиенте
Cache-Control: public // данные могут быть закешированы на промежуточных серверах

Cache-Control: proxy-revalidate // перед использованием данных из кеша прокси-сервера, их актуальность должна быть проверена

Cache-Control: no-transform // прокси не могут применять к ресурсу никакие преобразования. Другие настройки — private, public и proxy-revalidate — таких ограничений не накладывают 
```

В Cache-Control можно указать сразу несколько директив через запятую:

Скопировать код

```
Cache-Control: max-age=30000, must-revalidate 
```

Кроме Expires и Cache-Control_,_ есть два заголовка, которые отвечают за валидацию кеша — ETag и Last-Modified_._

**ETag**

Чтобы при каждой валидации не отправлять кеш серверу целиком, был придуман механизм хеширования. Он заключается в следующем: определённый алгоритм анализирует кеш и на основе этого создаёт строку — [хеш](https://praktikum.yandex.ru/learn/web/courses/dbf98e55-0f76-444b-850c-4538708ad571/sprints/1425/topics/b4072eed-2089-45c0-9382-98ea71202341/lessons/72688d32-ec42-41de-a818-a219c59d88b2/) (о нём говорили в теме о технологии Git). Такая строка уникальна для любого набора данных. Можно хранить в кеше только эту строку и передавать её для валидации — это куда эффективнее.

В заголовок Etag можно записать хеш. Если тело ответа меняется, изменится и хеш:

Скопировать код

```
ETag: "abcd1234567n34jv" 
```

**Last-Modified**

Заголовок Last-Modified хранит дату последнего изменения данных на сервере, что позволяет валидировать данные в кеше:

Скопировать код

```
Last-Modified: Fri, 10 May 2016 09:17:49 IST 
```

### Кеширующие заголовки в express

Express по умолчанию работает так:

- Создаёт заголовок ответа Etag, который посылает в каждом ответе сервера. Таким образом кешируется каждый ответ.
- Значение заголовка запоминает браузер и при следующем GET-запросе отправляет Etag внутри другого заголовка — If-None-Match.
- Если значение заголовка If-None-Match совпадает с каким-то кешем на сервере, ответ придёт в статусе 304 Not Modified. И браузер возьмёт значение ответа из своего кеша.

POST-запросы по умолчанию не кешируются. Да и на практике к кешированию POST-запросов прибегают редко. Но оно возможно: достаточно вручную выставить кеширующие заголовки.