[[JavaScript]]

Прежде чем приступить к уроку, запустите сервер MongoDB: откройте терминал (macOS и Linux) или Git Bash (Windows) и введите команду:

Скопировать кодBASH

```
mongod 
```

Если вы устанавливали Mongo на macOS через brew и команда `mongod` не запустила сервер, воспользуйтесь альтернативой. Но обратите внимание на номер версии mongodb — она должна совпадать с установленной:

Скопировать кодBASH

```
brew services start mongodb-community@4.2 
```

Сервер запущен. Можно приступать к чтению.

## Как взаимодействовать с MongoDB

Есть несколько способов. Мы расскажем о двух: графическом интерфейсе и через сервер на ноде.

### Графический интерфейс

Это очень наглядный способ. Через графический интерфейс можно просматривать и редактировать базы данных и записи в них. От слов — к делу.

Если работаете на macOS или Linux, скачайте и установите Compass — графический интерфейс для MongoDB. На Windows он уже установлен вместе с самой Mongo.

Ссылка на установщик Compass: [https://mongodb.prakticum-team.ru/download-center/compass](https://mongodb.prakticum-team.ru/download-center/compass).

Откройте Compass. Перед вами окажется такой экран:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_11.27.08_1569332673.png)

Оставьте всё как есть и нажмите Connect: Compass подключится к MongoDB серверу. Если этого не произошло и появилась красное сообщение об ошибке, проверьте, запущен ли MongoDB сервер.

Compass покажет уже имеющиеся базы данных. Создадим ещё одну. Нажмите Create Database:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_11.28.31_1569332710.png)

Введите имя новой БД (mydb) и первой коллекции в ней (users). Затем жмите Create Database:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_13.25.00_1569332982.png)

Имя базы нечувствительно к регистру: userdb и UserDB — одна и та же база. Чтобы не создавать конфликтующих имён, пользуйтесь только строчными буквами.

База данных состоит из коллекций, например, users, posts, comments. Ограничимся пока что одной — users. Нажмите Insert Document и создайте в ней документ:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_13.25.28_1569333014.png)

Откроется окно создания нового документа. У него уже есть одно поле — _id:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_13.27.26_1569333032.png)

Это уникальный идентификатор документа, MongoDB создаёт его сам. Добавьте ещё два поля: name и age. В правом столбце можно выбрать тип данных, записанных в поле. Для age выберите числовой тип — Int32:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_13.27.48_1569333057.png)

Затем нажмите Insert — и документ будет создан. Создайте ещё несколько:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_13.28.15_1569333075.png)

Документы в коллекции можно фильтровать. Это помогает искать их в базе. Просто напишите запрос в текстовом поле Filter. Запрос должен представлять собой объект:

![image](https://pictures.s3.yandex.net/resources/Screen_Shot_2019-09-22_at_13.28.29_1569333099.png)

Вот ещё несколько примеров запросов:

Скопировать кодJAVASCRIPT

```
{} // найдёт все документы коллекции
{ name: 'Стас' } // найдёт документы, где поле name равно "Стас"
{ name: 'Стас', age: 34 } // найдёт документы, где поле name равно "Стас" и поле age равно 34 
```

Искать документы через графический интерфейс очень удобно. Но добавлять их таким способом приходится нечасто — обычно этим занимается сервер. Для этого базу данных нужно связать с приложением на JavaScript. Этим мы и займёмся в следующем уроке.

## Дополнительные Ссылки

**Установщик Compass**

[https://mongodb.prakticum-team.ru/download-center/compass](https://mongodb.prakticum-team.ru/download-center/compass)

**Compass. Как писать запросы**

[https://mongodb.prakticum-team.ru/docs/manual/tutorial/query-documents/](https://mongodb.prakticum-team.ru/docs/manual/tutorial/query-documents/)

**Mongo Shell. Документация**

[https://mongodb.prakticum-team.ru/docs/v4.4/mongo/](https://mongodb.prakticum-team.ru/docs/v4.4/mongo/)

Какое поле есть у каждого документа в MongoDB?

`documentId`

`id`

Правильный ответ

`_id`

Правильно. Это поле MongoDB создаёт автоматически.

`document`

Как в Сompass найти всех тридцатилетних Олегов?

Правильный ответ

{ name: 'Олег', age: 30 }

Да, это правильный синтаксис!

name == "Олег" && age == 30

find: 'Олег 30'

return (name === 'Олег' && age === 30)