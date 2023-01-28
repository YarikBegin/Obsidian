
[[Элементы создание, добавление, удаление, перемещение, клонирование, Template]]

Теперь вы знаете, как создавать и добавлять элементы на страницу. Но у этих способов есть недостатки.

## Недостатки изученных методов

**Первый способ.** Через HTML-код в виде строки и метод `insertAdjacentHTML`:

Скопировать кодJSX

```
const container = document.querySelector('.container');

container.insertAdjacentHTML('beforeend', '<div class="tiger">Тигр</div>'); 
```

Большой минус такого подхода — он открывает XSS-уязвимость в безопасности сайта.

**Второй способ.** Создание и добавление элементов методами `createElement`, `append` и их аналогами.

Плюс подхода — нет уязвимостей в коде. Минус — даже при простой вёрстке код выглядит так:

Скопировать кодJAVASCRIPT

```
const virusStatsTable = document.createElement('table');

const row = document.createElement('tr');
row.classList.add('table__row');

const confirmed = document.createElement('td');
confirmed.classList.add('table__data');
confirmed.textContent = 1020;

const deaths = document.createElement('td');
deaths.classList.add('table__data');
deaths.textContent = 0;

const recovered = document.createElement('td');
recovered.classList.add('table__data');
recovered.textContent = 980;

row.append(confirmed, deaths, recovered);
virusStatsTable.append(row); 
```

Этот код очень сложно читать и редактировать, хотя он собирает простой элемент:

Скопировать кодHTML

```
<table>
  <tr class="table__row">
    <td class="table__data">1020</td>
    <td class="table__data">0</td>
    <td class="table__data">980</td>
  </tr>
</table> 
```

Но есть способ создания элементов без этих минусов — тег `template`.

## Тег `template`

Тег `template` — заготовка вёрстки. Её используют для создания элементов. Если добавить `template` в HTML, содержимое тега не отобразится на сайте:

Скопировать кодHTML

```
<template id="user">
  <div class="user">
    <img class="user__avatar" alt="avatar">
    <p class="user__name"></p>
  </div>
</template> 
```

Зато в JavaScript мы можем получить этот элемент методом `querySelector`:

Скопировать кодJAVASCRIPT

```
const userTemplate = document.querySelector('#user'); 
```

Чтобы получить содержимое `template`, нужно обратиться к его свойству `content`:

Скопировать кодJAVASCRIPT

```
const userTemplate = document.querySelector('#user').content; 
```

Теперь этот элемент можно клонировать, наполнить содержимым и вставить в DOM.

Скопировать кодJAVASCRIPT

```
const userTemplate = document.querySelector('#user').content;
const usersOnline = document.querySelector('.users-online');

// клонируем содержимое тега template
const userElement = userTemplate.querySelector('.user').cloneNode(true);

// наполняем содержимым
userElement.querySelector('.user__avatar').src = 'tinyurl.com/v4pfzwy';
userElement.querySelector('.user__name').textContent = 'Дюк Корморант';

// отображаем на странице
usersOnline.append(userElement); 
```

Если понадобится ещё один такой элемент, содержимое `template` снова клонируют.

Ещё одно преимущество `template` перед `createElement` — браузер проверяет на валидность код внутри этого тега. Допускается любой корректный HTML-код. Вложенность тегов соблюдать не обязательно: так тег `tr` внутри `template` не обязан быть внутри `table`:

Скопировать кодHTML

```
<template id="user">
  <!-- tr может быть сам по себе -->
  <tr class="data"></tr>
</template> 
```