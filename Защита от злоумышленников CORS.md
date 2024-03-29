[[Продвинутый JavaScript]]

В современных браузерах есть ограничение: по умолчанию вы не можете отправить запрос из фронтенда одного сайта к другому. Если вы находитесь на сайте `https://one-site.com`, не получится запросить что-то методом `fetch` у сайта `https://another-site.com`. Браузер выдаст ошибку:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-17_at_17.28.47_1613828125.png)

Это браузерное ограничение было введено из соображений безопасности. Вместе с запросом браузер отправляет серверу дополнительные данные — «куки» (англ. cookie). Куки — строка, которая может быть использована сервером для авторизации на сайте.

Если бы ограничений на отправку запросов между сайтами не было, злоумышленники смогли бы использовать куки в своих злодейских целях. Например, создать сайт со специальным скриптом: пользователи заходили бы на него, а их интернет-банкам уходил бы запрос на перевод денег мошенникам. Так в интернете воцарились бы анархия и хаос.

Поэтому по умолчанию кросс-доменные запросы запрещены. Но поведение по умолчанию можно изменить. Это необходимо при создании открытых API — таких как Kanye API, которым вы уже пользовались. К открытому API должна быть возможность обратиться с любого домена, и браузер не должен это блокировать.

## Разрешение кросс-доменных запросов

Браузер не может самостоятельно решить, можно ли отправлять кросс-доменные запросы. Для этого ему нужно разрешение от сервера. Чтобы его получить, есть специальный механизм предзапросов (англ. "preflight request"). Сначала браузер посылает предварительный запрос к серверу и, если получает разрешение, отправляет основной запрос на нужный домен.

Если кросс-доменные запросы нужно разрешить, мы должны запрограммировать сервер так, чтобы он давал браузерам разрешение отправлять такие запросы. Это делают с помощью установки специальных заголовков ответа.

### Как это решается на сервере

Когда совершается запрос, клиент и сервер обмениваются заголовками, в которых прописаны настройки и разрешения для различных действий. За доступ к данным на сервере отвечает заголовок `Access-Control-Allow-Origin`. В публичных API, например kayne.rest, заголовок выглядит так:

Скопировать код

```
Access-Control-Allow-Origin: * 
```

Это позволяет получать доступ к данным с любого сайта, так как звездочка убирает ограничение по конкретным сайтам, с которых можно делать запрос.

В реальном проекте, где данные должны быть защищены, заголовок будет выглядеть примерно так:

Скопировать код

```
Access-Control-Allow-Origin: https://my-site.com 
```

Это значит, что сервер разрешает запросы только с конкретного сайта.

Фронтенд-разработчики сталкиваются со сложностями с CORS в начале разработки или при работе с API, где требуется авторизация. Теперь вы знаете, кто виноват. А что делать — разберём в теме по серверной разработке, — там научимся правильно настраивать эти заголовки.

### Какие запросы обрабатывать? Методы и заголовки

В заголовке Access-Control-Allow-Methods указывают допустимые методы запросов. В Access-Control-Allow-Headers — какие заголовки могут в нём быть.

Скопировать код

```
Access-Control-Allow-Methods: GET,HEAD,PUT,PATCH,POST,DELETE
Access-Control-Allow-Headers: Origin, X-Requested-With, Content-Type, Accept 
```

Полный список заголовков, которые можно разрешить, довольно длинный. Ознакомиться с ними вы сможете в дополнительных материалах в конце темы.