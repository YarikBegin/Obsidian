[[JavaScript]]

[[Введение в Redux]]
[[React Router]]
[[Компоненты]]
[[Декларативный подход]]
[[«Реакт» и JSX]]
[[Синтаксис JSX основы]]
[[Синтаксис JSX списки и события]]
[[Функциональные компоненты]]
[[Классовые компоненты]]
[[Жизненный цикл классовых компонентов]]
[[Virtual DOM]]
[[Инструментарий «Реакта». Введение]]
* [[Знакомство с Create React App]]
* [[Новый проект]]
* [[Распаковка проекта]]
* [[Структура проекта]]
* [[Импорт модулей]]
* [[Импорт изображений]]
* [[Работа с CSS]]
* [[Шрифты]]
* [[Добавление классового компонента]]
* [[Дебаггинг проекта]]
* [[Установка расширения React DevTools]]
* [[Использование расширения React DevTools]]
* [[Сборка проекта]]
## Хуки:
* [[Внутреннее состояние хук useState]]
* [[Эффекты хук useEffect]]
* [[Зависимости]]

[[Списки и ключи]]
[[Работа с формами]]
[[Рефы]]
[[Чистые компоненты]]
[[Компоненты более высокого порядка]]

# Веб-приложения и фреймворки

### Немного истории

Много лет назад веб-сайты целиком генерировались на сервере, и каждое нажатие кнопки или переход по ссылке вызывали перезагрузку всей страницы. Хоть 56k-модемы и скрипели изо всех сил, скорость соединения всё ещё была далека от сегодняшней, поэтому веб-сайты сильно отставали от обычных системных приложений в удобстве и функциональности. Они содержали в основном статичную информацию, а веб-разработчиков многие даже не причисляли к программистам и снисходительно называли веб-мастерами или даже веб-дизайнерами.

В какой-то момент произошёл переворот: появилась технология AJAX, а вслед за ней SPA — веб-приложения, которые могли реагировать на действия пользователя и подгружать с сервера данные динамически, то есть без перезагрузки основной страницы. Такие приложения начали активно конкурировать с системными, а во многом даже опережать их: например, в скорости доставки новых версий. Стало появляться всё больше сложных веб-приложений, которые раньше трудно было себе представить: к примеру, полноценные графические редакторы — такие как «Фигма».

Началась эпоха Web 2.0, а JavaScript начал свой путь к захвату мира.

https://youtu.be/cKzP61Gjf00

### Зачем нужны фреймворки

HTML, CSS и JavaScript — отличные инструменты для разработки простых веб-страниц. Однако, с увеличением сложности проекта обычно требуются более комплексные решения: библиотеки и фреймворки. Фреймворк — это каркас приложения, предоставляющий разработчику универсальную структуру будущего проекта, а также набор готовых решений для типовых задач.

Первая основная задача — разбиение интерфейса на отдельные функциональные блоки (так называемые «компоненты»), которые можно переиспользовать. В качестве примера можно привести аватар с именем пользователя или кнопку лайка, которые могут встречаться в приложении много раз и в разных местах. Часто такие компоненты используют все три технологии: HTML, CSS и JavaScript. В этом случае удобно хранить их код рядом и отделить его от кода других компонентов. Многие фреймворки, включая «Реакт», предоставляют такую возможность.

Другая важнейшая задача — динамическое обновление интерфейса. Классический (императивный) подход заключается для разработчика в описании последовательности изменений на HTML-странице при наступлении каких-либо событий (например: «при клике на кнопку сделать её цвет синим»). «Реакт» позволяет использовать декларативный подход, при котором разработчику достаточно описать все возможные состояния интерфейса в любой момент времени (например: «если кнопка нажата, она должна быть синей»). Такой подход показал себя более гибким и надёжным в больших проектах.

«Реакт» предлагает универсальные решения для обеих задач, описанных выше. При этом сама библиотека «Реакт» не является фреймворком в классическом понимании, так как не предоставляет разработчику заранее заданную структуру проекта. Однако, на сегодняшний день вокруг «Реакта» сформировалась целая экосистема, включающая в себя дополнительные библиотеки и плагины, и даже методология, определяющая общепринятые подходы к разработке (паттерны), — в том числе относительно структуры приложений.

![image](https://pictures.s3.yandex.net/resources/123_1593870561.png)
