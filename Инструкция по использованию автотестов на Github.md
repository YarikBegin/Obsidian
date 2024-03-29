[[JavaScript]]

При выполнении задания этого спринта вас ждут некоторые нововведения в виде автотестов на базе Github Actions. Платформа тренажёра автоматически создаёт стартовый репозиторий с тестовыми сценариями.

Перед тем, как сдать работу на ревью, нужно сделать репозиторий публичным, добавить на него ссылку в файле README и убедиться, что код проектной работы успешно проходит тестовые сценарии.

## Делаем репозиторий публичным

Для этого нужно пройти по шагам:

1. Открыть созданный репозиторий проектной работы. По умолчанию он называется `express-mesto-gha` и расположен последним в списке созданных репозиториев.
2. Перейти на вкладку Settings:

![image](https://pictures.s3.yandex.net/resources/01_1645104269.png)

3. Опуститься вниз страницы до блока Danger Zone и нажать кнопку “Change visibility”:

![image](https://pictures.s3.yandex.net/resources/02_1645104561.png)

4. В открывшемся модальном окне выбрать “Make public”, ввести адрес репозитория и подтвердить действие нажатием кнопки “I understand, change repository visibility”:

![image](https://pictures.s3.yandex.net/resources/03_1645104616.png)

5. После этого вы будете перенаправлены на главную страницу настроек репозитория. Убедитесь, что тип репозитория “Public”. Он указан рядом с названием репозитория. Если это не так, повторите все шаги.

![image](https://pictures.s3.yandex.net/resources/04_1645104648.png)

6. Добавьте ссылку на ваш репозиторий в файл README.md.

## Проверяем статус прохождения тестов

Тестовые сценарии и результат их выполнения доступны во вкладке Actions в панели Workflow. В левой колонке (Workflows) содержится список сценариев проверки, а в правой — история их запуска.

![image](https://pictures.s3.yandex.net/resources/2038_1645104680.png)

Чтобы успешно завершить работу над заданием 13 проектной работы, нужно добиться прохождения тестов “Tests 13 sprint”. Тесты не наследуются, поэтому не нужно проходить сценарий предыдущей проектной работы. К примеру, для 14 проектной работы нужно пройти только “Tests 14 sprint” и так далее.

Если код успешно прошёл тесты, рядом с описанием последнего коммита располагается зелёная галочка.

![image](https://pictures.s3.yandex.net/resources/2039_1645104701.png)

Если бейдж красный, значит какие-то из тестов завершились с ошибкой.

Чтобы узнать причину падения тестов, нужно кликнуть по соответствующему коммиту в истории, затем — по сбойному тесту.

![image](https://pictures.s3.yandex.net/resources/2040_1645104727.png)

Если какие-то тесты завершились ошибкой, мы рекомендуем внимательно перечитать требования и условия проектной работы, а также проверить её по чек-листу, если он есть.

После того, как все тесты успешно пройдены, вы можете сдавать работу на проверку.

Идеального кода не существует и код тестов — не исключение. Если тесты пройти не удалось, но вы твёрдо уверены, что в решении нет ошибок — вы можете сдать проектную работу в качестве исключения.

В этом случае следует сообщить куратору о возникшей проблеме и приложить ссылку на репозиторий, указать название упавшего теста и ваши предположения, почему так произошло, к примеру вы использовали нестандартный, но корректный подход или новый синтаксис. Это поможет нам сделать тесты лучше и точнее.