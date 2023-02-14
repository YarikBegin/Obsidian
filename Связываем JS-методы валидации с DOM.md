[[Валидация]]

В предыдущем уроке вы узнали, как получить данные о валидации в JS. Они нужны, чтобы модифицировать интерфейс: изменить цвет текстового поля, показать сообщение об ошибке.

Этим мы и займёмся в уроке.

## Изменение стиля поля при ошибке

Начнём с простой задачи. Нужно изменить стили поля, в которое ввели некорректные данные. Для этого невалидному полю `input` добавим класс со стилем ошибки.

Возьмём простую форму:

Скопировать кодHTML

```
<form class="form" novalidate>
  <label class="form__field">
    Введите адрес электронной почты
    <input class="form__input" type="email" placeholder="Email" required>
  </label>
  <button class="form__submit">Войти</button>
</form> 
```

И опишем стили для класса невалидного `input`:

Скопировать кодCSS

```
.form__input_type_error {
  border-bottom-color: red;

  /* Нижняя рамка становится красной всякий раз, 
  когда введённые данные некорректны */
} 
```

Настроим поведение поля ввода. Оно должно быть разным для корректных и некорректных данных.

Для этого выделим несколько функций:

-   `showInputError` — показывает элемент ошибки;
-   `hideInputError` — скрывает элемент ошибки;
-   `isValid` — проверяет валидность поля, внутри вызывает `showInputError` или `hideInputError`.

Код с этими функциями выглядит так. Комментарии подробные, чтобы не запутаться, где что происходит:

Скопировать кодJSX

```
// Вынесем все необходимые элементы формы в константы
const formElement = document.querySelector('.form');
const formInput = formElement.querySelector('.form__input');

// Функция, которая добавляет класс с ошибкой
const showInputError = (element) => {
  element.classList.add('form__input_type_error');
};

// Функция, которая удаляет класс с ошибкой
const hideInputError = (element) => {
  element.classList.remove('form__input_type_error');
};

// Функция, которая проверяет валидность поля
const isValid = () => {
  if (!formInput.validity.valid) {
    // Если поле не проходит валидацию, покажем ошибку
    showInputError(formInput);
  } else {
    // Если проходит, скроем
    hideInputError(formInput);
  }
};

// Вызовем функцию isValid на каждый ввод символа
formInput.addEventListener('input', isValid); 
```

Мы разделили функции так, чтобы каждая из них выполняла что-то одно. Так проще вызывать их для других действий, если это потребуется.

В результате получим:

Поле ввода реагирует корректно. Форма не меняется и валидация не начинается, пока пользователь не начинает вводить данные.

Но мы добавили атрибут `novalidate`. Из-за этого пропало сообщение об ошибке, когда пытаются отправить некорректную форму.

## Добавление `span` ошибки

Одной красной рамки недостаточно, чтобы указать на некорректность введённых данных. Поэтому добавим своё сообщение об ошибке. Это элемент `span` с классом `form__input-error`:

Скопировать кодHTML

```
<form class="form" novalidate>
  <label class="form__field">
    Введите адрес электронной почты
    <input class="form__input" type="email" placeholder="Email" required>
    <span class="form__input-error">Необходимо заполнить данное поле</span>
  </label>
  <button class="form__submit">Войти</button>
</form> 
```

Теперь нужно связать элемент ошибки с полем, которое ей соответствует. Надёжный способ — добавить атрибут `id` полю ввода и задать уникальный класс тегу `span`. Далее искать по этим значениям.

Добавляйте `id` для `input` и уникальный класс для `span` сразу. Часто в формах много полей ввода: легко запутаться, какую ошибку с чем нужно связать.

Допишем уникальный класс элементу ошибки:

Скопировать кодHTML

```
<input id="email-input" class="form__input" type="email" placeholder="Email" required>
<span class="email-input-error form__input-error">Необходимо заполнить данное поле</span> 
```

Теперь `id` поля `form__input` можно получить так:

Скопировать кодJAVASCRIPT

```
const formInput = formElement.querySelector('.form__input');

console.log(formInput.id); // "email-input" 
```

И с применением шаблонных строк найти эту ошибку:

Скопировать кодJAVASCRIPT

```
const formError = formElement.querySelector(`.${formInput.id}-error`); 
```

Расширим возможности функций. Пусть они ещё отвечают за показ сообщения об ошибке:

Скопировать кодJAVASCRIPT

```
const formElement = document.querySelector('.form');
const formInput = formElement.querySelector('.form__input');

// Выбираем элемент ошибки на основе уникального класса 
const formError = formElement.querySelector(`.${formInput.id}-error`);

const showInputError = (element) => {
  element.classList.add('form__input_type_error');
  // Показываем сообщение об ошибке
  formError.classList.add('form__input-error_active');
};

const hideInputError = (element) => {
  element.classList.remove('form__input_type_error');
  // Скрываем сообщение об ошибке
  formError.classList.remove('form__input-error_active');
};

// Остальной код такой же 
```

Под текстовым полем появляется `span` сообщения об ошибке.

Теперь мы настроили стили ошибки и её поведение.

Но у нас один текст ошибки для всех случаев. Это вводит в заблуждение. Лучше показывать разный текст для разных ошибок валидации. Для этого нужно свойство `validationMessage`.

## Изменение сообщения об ошибке

Свойство `validationMessage` есть у всех полей ввода. В нём записан текст сообщения об ошибке. Браузер показывает его по умолчанию, когда вводят некорректные данные. Чтобы добавить это сообщение в свой код, нужно передать функции `showInputError` свойство `validationMessage`.

Удалим содержимое `form__input-error`:

Скопировать кодHTML

```
<span class="email-input-error form__input-error"></span> 
```

Добавим новые возможности в скрипт:

Скопировать кодJAVASCRIPT

```
const formElement = document.querySelector('.form');
const formInput = formElement.querySelector('.form__input');
const formError = formElement.querySelector(`.${formInput.id}-error`);

// Передадим текст ошибки вторым параметром
const showInputError = (element, errorMessage) => {
  element.classList.add('form__input_type_error');
  // Заменим содержимое span с ошибкой на переданный параметр
  formError.textContent = errorMessage;
  formError.classList.add('form__input-error_active');
};

const hideInputError = (element) => {
  element.classList.remove('form__input_type_error');
  formError.classList.remove('form__input-error_active');
  // Очистим ошибку
  formError.textContent = '';
};

const isValid = () => {
  if (!formInput.validity.valid) {
    // Передадим сообщение об ошибке вторым аргументом
    showInputError(formInput, formInput.validationMessage);
  } else {
    hideInputError(formInput);
  }
};
 
// Остальной код такой же 
```

Можно не придумывать свой текст ошибки, а использовать стандартный.

Теперь вы знаете, как установить связь между скриптом и DOM. Переходите к заданиям и попрактикуйтесь в этом.

А в следующем уроке вы разберёте, как проверять данные на корректность сразу в нескольких полях и как блокировать и разблокировать другие элементы интерфейса.