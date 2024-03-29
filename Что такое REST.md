[[Продвинутый JavaScript]]

REST, или Representational State Transfer (англ. «передача состояния представления»), — набор принципов, которых рекомендовано придерживаться при создании API. Если API сделан по этим принципам, его называют REST API.

Раньше клиент и сервер были тесно связаны. Например, сервер собирал веб-страницу и отдавал клиенту. Серверу нужно было знать, какие технологии работают на стороне клиента, чтобы тот смог открыть вернувшуюся веб-страницу.

Принципы REST позволили отделить клиента от сервера. Благодаря этому:

-   стало проще переносить веб-приложение на другие платформы;
-   появилась возможность делать открытые API;
-   разрабатывать и тестировать серверное ПО стало проще и быстрее.

Рассмотрим принципы REST, которые важно понимать для программирования сервера.

## Принципы REST

### 1. Клиент-сервер

Сервер и клиент отвечают за разные вещи. Ответственность клиента — пользовательский интерфейс, а ответственность сервера — данные. Если API возвращает HTML-страницу, его нельзя назвать REST API: ведь при этом сервер берёт на себя ответственность за интерфейс.

В REST API сервер обычно возвращает данные в формате JSON. Именно этот принцип делает возможным точечную отрисовку страницы и существование единого API для браузера и мобильного приложения.

### 2. Отсутствие состояния (англ. "Stateless")

Запрос клиента к серверу должен содержать всю информацию, необходимую для обработки этого запроса. В проекте Mesto вы отправляли на сервер токен, чтобы получить карточки. Токен сообщал системе, что вы — это вы: запрос содержал информацию о том, кто запрашивает данные и какие.

Раньше серверы программировали иначе. При аутентификации сервер создавал сессию, то есть запоминал, что пользователь совершил вход. Сейчас такой подход применяется редко.

При отправке токена создавать сессию не нужно. Сервер не хранит информацию о состоянии пользователя, поэтому принцип и называется «отсутствие состояния».

### 3. Единый интерфейс (англ. "Uniform Interface")

Интерфейс обращения к серверу не зависит от клиента. Он одинаковый для всех.

Скопировать код

```
GET https://nomoreparties.co/cards/5d1f0611d321eb4bdcd707dd 
```

Так, запрос к карточке может быть сформирован из браузера, мобильного приложения и с умного чайника — для всех интерфейс един.

### 4. Многоуровневость

Первый принцип гласит, что в коммуникации участвуют двое: клиент и сервер. Но, благодаря многоуровневости, мы всё равно можем строить более сложные системы, не нарушая этого принципа.

API сервиса Яндекс.Такси может использовать API Яндекс.Навигатора. Вы как клиент взаимодействуете только с API Яндекс.Такси, а он, в свою очередь, является клиентом навигатора.

### 5. Кешируемость

Данные ответа могут быть закешированы. Это значит, мы можем сохранить данные на клиенте и при идентичном запросе взять их из кеша — памяти клиента. Чуть подробнее про кеширование мы поговорим в теме об Express — серверном фреймворке на Node.js.

### 6. Код по запросу (англ. "Code on demand")

Это необязательный принцип. Он подразумевает, что функциональность клиента может быть расширена кодом, который приходит с сервера. Сейчас такое можно встретить повсеместно: мы получаем с сервера JS-файлы и исполняем их в браузере. Но принципы формулировались в 2000 году — тогда с сервера код возвращали редко, потому разработчики REST API и выделили это в отдельный пункт.

## Зачем использовать REST API?

REST API помогает разработчику делать пользовательский опыт лучше. Например, при переходе страницы отрисовываются моментально, а пользователь не видит этот раздражающий белый экран или значок загрузки. А ещё REST API — настоящее спасение при нестабильном подключении сети. Теперь вашим приложением смогут пользоваться даже там, куда интернет, казалось бы, провели недавно.

В REST API есть некоторые нововведения, например авторизация через JWT, а не сессии. Но про это мы расскажем чуть позже. Кроме этого, вы узнаете, как правильно работать с REST API с фронтенда.

## Задание

Запросы и информацию о них можно посмотреть из инструментов разработчика. Для этого откройте браузер, а в нём — инструменты разработчика. Перейдите во вкладку Network, если вы используете Yandex.Browser, Chrome или Firefox. Затем перейдите по ссылке:

Скопировать код

```
https://nomoreparties.co/tests/1 
```