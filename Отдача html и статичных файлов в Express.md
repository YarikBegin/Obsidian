[[JavaScript]]

Мы разобрались с передачей данных в ответе на запрос. Но пользователю нужно отдавать и статичные файлы: разметку, стили, скрипты, изображения. Писать обработчики для всей этой информации неудобно и не нужно, вместо этого научимся пользоваться методом `static`.

По умолчанию у пользователей нет доступа к файлам на вашем сервере. Это можно изменить методом `static`. Он принимает адрес папки на вход и делает её доступной «снаружи»:

Скопировать кодJAVASCRIPT

```
app.use(express.static(__dirname)); // сделали всё, что есть на сервере, доступным пользователю 
```

Такой код даст доступ к корню сервера, но там могут храниться данные, которые пользователям видеть не нужно, например, ключи доступа к другим ресурсам или базы данных. Файлы, которые нужно отдавать клиентам, складывают в отдельную папку — её принято называть "public".

Скопировать кодJAVASCRIPT

```
app.use(express.static(path.join(__dirname, 'public'))); // теперь клиент имеет доступ только к публичным файлам 
```

Метод `static` задаёт определённую логику отправки файлов. Если обратиться к корню сервера, мы автоматически попадём в папку public и получим файл index.html. Получается, что не нужно добавлять в запрос имя файла, достаточно просто обратиться в корень — по адресу `"/"`.

Публичная папка — отправная точка в проекте, которую мы уже указали с помощью express.static. Осталось добавить относительные пути ко всем файлам:

|Нет|Да|
|---|---|
|/public/style.css|/style.css|