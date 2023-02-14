[[Формы]]

Все формы на странице хранятся в виде псевдомассива в свойстве `document.forms`:

Скопировать кодHTML

```
<!-- index.html -->

<form>
  <h2>Форма ввода слова «Яндекс»</h2>
  <input type="text" placeholder="Яндекс">
</form>

<form>
  <h2>Форма про вселенную</h2>
  <input type="number" placeholder="Ответ на главный вопрос жизни">
  <input type="text" placeholder="Какова форма земли?">
</form> 
```

При такой разметке в `document.forms` окажется коллекция из двух элементов-форм. К каждой из них можно обратиться по индексу:

Скопировать кодJAVASCRIPT

```
/* script.js */

document.forms[0]; // первая форма
document.forms[1]; // вторая форма 
```

Но обращаться по индексу ненадёжно. Если поменяют порядок форм или одну из них удалят, код придётся переписывать.

Лучше обращаться через атрибут формы `name`. Его значение становится свойством объекта `document.forms`:

Скопировать кодHTML

```
<!-- index.html -->

<form name="form1">
  <h2>Форма ввода слова «Яндекс»</h2>
  <input type="text" placeholder="Яндекс">
</form>

<form name="form2">
  <h2>Форма про вселенную</h2>
  <input type="number" placeholder="Ответ на главный вопрос жизни">
  <input type="text" placeholder="Какова форма земли?">
</form> 
```

Теперь к формам можно обращаться по имени:

Скопировать кодJAVASCRIPT

```
/* script.js */

document.forms.form1; // первая форма
document.forms.form2; // вторая форма 
```

Такой код более устойчив к изменениям.

![image](https://pictures.s3.yandex.net/resources/actions___1__125_1588447461.png)

_Новым формам всё нипочём_

Переходите к заданиям и потренируйтесь работать с формами.