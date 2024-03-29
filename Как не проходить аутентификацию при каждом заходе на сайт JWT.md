[[Продвинутый JavaScript]]

Вы узнали, что такое авторизация и аутентификация, и чем они отличаются. Наверняка вы замечали, что при посещении одной и той же социальной сети вам не приходится каждый раз проходить аутентификацию: повторно вводить почту и пароль при каждом посещении сайта.

Удобно, когда браузер запоминает, что вы уже вводили данные и заходили на сайт. Это реализуется путём создания токена. Что это такое — расскажем прямо сейчас. А уже в следующем спринте вы на практике создадите токен .

## Алгоритм

Мы реализуем аутентификацию так:

1.  Пользователь входит в аккаунт по почте и паролю.
2.  Сервер генерирует токен — уникальный набор символов — и отправляет его пользователю.
3.  Токен сохраняется в браузере пользователя.
4.  При повторном открытии сайта пользователь отправляет токен серверу.
5.  Сервер проверяет, есть ли токен в запросе и тот ли это токен, что был выдан пользователю раньше.
6.  Если проверка прошла успешно, пользователь авторизуется, иначе — получает сообщение об ошибке.
7.  Когда пользователь выходит из системы, браузер удаляет токен из памяти. После этого нужно заново входить в систему по почте и паролю.

Весь алгоритм построен на формировании токена. Рассмотрим, что он собой представляет.

## Структура токена

Токен — набор символов, сгенерированный согласно структуре и правилам. Эти структура и правила определяют стандарт создания токена. Мы будем пользоваться стандартом JWT (JSON Web Token).

JWT основан на стандарте JSON. Токен, созданный по этому стандарту, состоит из трёх частей, каждая несёт в себе определённую информацию:

-   header (англ. «хедер») — служебную информацию;
-   payload (англ. «полезная нагрузка») — данные, которые токен несёт в себе;
-   signature (англ. «подпись») — подпись, которая предотвращает подмену информации в токене.

Эти три части разделены точками:

![image](https://pictures.s3.yandex.net/resources/Artboard_1_1613832306.png)

Разберём каждую из них подробнее.

### Header

Хедер, как правило, содержит два поля:

-   тип токена (строка `"JWT"`);
-   алгоритм создания подписи (обычно применяется алгоритм HMAC SHA256 или RSA):
    
    Скопировать кодJAVASCRIPT
    
    ```
      {
        "alg": "HS256",
        "typ": "JWT"
      }
       
    ```
    

Полученный JSON-объект кодируется в строку распространённым алгоритмом Base64Url. Это первая часть токена.

### Payload

Пейлоуд — содержит саму информацию, которая была закодирована. В нашем случае это информация о пользователе:

Скопировать кодJAVASCRIPT

```
{
  "name": "Стас Басов",
  "_id": "39dow8ak8402jf23u4do057s"
} 
```

Аналогично хедеру, пейлоуд кодируется в строку. Получаем вторую часть токена.

### Signature

Подпись гарантирует, что содержимое хедера и пейлоуда не изменились после создания токена. Специальный алгоритм высчитывает подпись, исходя из содержимого хедера и пейлоуда. Также алгоритм использует секретный ключ, который известен только серверу.

Теперь вы знаете о самой структуре токена и алгоритме работы с ним. Пока этого достаточно. В следующих уроках вы научитесь работать с токеном на стороне пользователя — сохранять полученный от сервиса токен, и отправлять его при наличии.

Переходите дальше — там вы познакомитесь с новым проектом.