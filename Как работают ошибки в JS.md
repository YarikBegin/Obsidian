[[JavaScript]]

В этом уроке мы поговорим о том, как работают ошибки в JavaScript, и вспомним основные подходы к их обработке.

В JS для информирования об ошибках обычно используется механизм исключений. Например, исключения могут автоматически выбрасываться самим движком JS, когда при выполнении кода происходит некорректная операция. Вы, вероятно сталкивались с этим, если допускали в коде синтаксическую ошибку. Например, при опечатке. Тогда при запуске вы получали ошибку `SyntaxError`.

Исключения могут быть выброшены и самим программистом при помощи оператора `throw`. Для этого необходимо вызвать `throw` и вместе с ним указать соответствующий объект ошибки, например так:

Скопировать кодJAVASCRIPT

```
throw new Error("Когда в коде появляется ошибка, в мире грустит один тестировщик");  
```

Часто в качестве ошибки класса указывается стандартный `Error`, но это необязательно.

В реальных проектах разработчики нередко используют и собственные классы ошибок, дополняя объект ошибки необходимой информацией. Подробнее об этом мы поговорим в следующем уроке — сейчас достаточно просто знать, что такая возможность есть. А пока разберёмся, из чего состоит объект ошибки.

## Из чего состоит ошибка в JS

Объект ошибки представляет собой обычный JS-объект. Он может содержать произвольные поля, но у него есть 3 обязательных:

- message — содержит текст ошибки, где кратко отражена суть проблемы. Важно: поле предназначено для чтения пользователем и не должно использоваться для определения типа ошибки внутри вашего кода.
- stack — содержит так называемый стектрейс: последовательность вызова функций в коде, которая привела к ошибке. Благодаря значению этого поля мы можем понять, откуда и как была вызвана функция, а также какие функции были вызваны до неё. Это значительно облегчит поиск и исправление возможных проблем в коде.
- name — содержит имя ошибки, которое позволяет идентифицировать её при обработке. Поскольку поле выступает как идентификатор, важно следить, чтобы оно не повторялось для ошибок разного типа, иначе их будет невозможно отличить. Для стандартной ошибки класса Error оно по-умолчанию также равно Error.

Ошибки — обычные объекты, поэтому мы можем записывать информацию в них также, как и в другие объекты, задавая значение необходимому свойству.

Конструктор ошибки Error не позволяет задать имя ошибки сразу при создании объекта (по-умолчанию оно равно Error). Но мы можем установить его значение в уже созданном экземпляре ошибки.

Проделаем это: создадим ошибку с именем NotFoundError и текстом «Объект не найден».

Скопировать кодJAVASCRIPT

```
let err = new Error("Объект не найден"); // создаём стандартную ошибку с текстом "Объект не найден"
err.name = "NotFoundError"; // задаём NotFoundError в качестве имени ошибки
console.log(err.name) // убедимся, что имя сохранилось внутри объекта ошибки
 
```

Теперь вы знаете, как работают и из чего состоят ошибки в JS. В следующем уроке поговорим о том, как правильно их обрабатывать. А ещё научимся создавать собственные классы ошибок, которые позволят избежать ручной «перезаписи» полей.