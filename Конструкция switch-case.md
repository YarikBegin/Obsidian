[[JavaScript]]



Представьте, что вы пришли на почту. Взяли талончик, ждёте — и через какое-то время слышите: «Клиент номер Л055, пройдите к окну 2». Вы находите на табло свой номер и рядом — номер окна, к которому нужно подойти.

Опишем эту логику на JavaScript:

Скопировать кодJAVASCRIPT

```
const yourNumber = 'Л055';
let windowNumber;

if (yourNumber === 'Л054') {
  windowNumber = 1;
} else if (yourNumber === 'Л055') {
  windowNumber = 2;
} else if (yourNumber === 'Л056') {
  windowNumber = 3;
} else if (yourNumber === 'Л057') {
  windowNumber = 4;
} else if (yourNumber === 'Л058') {
  windowNumber = 5;
}

console.log(windowNumber); 
```

Код работает, но перегружен, хотя задача примитивна. Тут поможет `switch`.

Оператор `switch` применяют, когда в условии только один ответ правильный. Каждый вариант описывают между ключевым словом `case` и инструкцией `break`:

Скопировать кодJAVASCRIPT

```
switch (/* Переменная для проверки */) {
    case /* Первое возможное значение:
        Выполняемый код */
        break;
    case /* Второе возможное значение:
        Выполняемый код */
        break;
    ...
    case /* n-ое возможное значение:
        Выполняемый код */
        break
} 
```

В примере с почтой:

Скопировать кодJAVASCRIPT

```
const yourNumber = 'Л055';
let windowNumber;

switch (yourNumber) {
  case 'Л054':
    windowNumber = 1;
        break;
  case 'Л055':
    windowNumber = 2;
        break;
  case 'Л056':
    windowNumber = 3;
        break;
  case 'Л057':
    windowNumber = 4;
        break;
  case 'Л058':
    windowNumber = 5;
}

console.log(windowNumber); // 2 
```

Визуально код не стал короче, зато теперь его намного проще читать. Как на электронном табло, можно найти значение и посмотреть, какой код будет выполнен в этом случае.

Каждый раз `case` завершается `break`: так движок JS понимает, что из конструкции `switch-case` нужно выйти. Если не поставить `break`, сработает код не только «нужного» `case`, но и всех под ним:

Скопировать кодJAVASCRIPT

```
const yourNumber = 'Л055';
let windowNumber;

switch (yourNumber) {
  case 'Л054':
    windowNumber = 1;
  case 'Л055':
    windowNumber = 2;
  case 'Л056':
    windowNumber = 3;
  case 'Л057':
    windowNumber = 4;
  case 'Л058':
    windowNumber = 5;
}

console.log(windowNumber); // 5 
```

Поэтому для последнего `case` писать `break` не нужно.

Можно сознательно пропустить `break`, чтобы прописать логику сразу нескольких случаев:

Скопировать кодJAVASCRIPT

```
let catName;
const cartoon = 'Зима в Простоквашино';

switch (cartoon) {
    case 'Зима в Простоквашино':
    case 'Весна в Простоквашино':
    case 'Трое из Простоквашино':
        catName = 'Матроскин';
        break;
    case 'Лето кота Леопольда':
        catName = 'Леопольд';
}

console.log(catName); // "Матроскин" 
```

Ещё есть необязательная инструкция `default`. Сюда записывают код, который должен сработать, если ни один `case` не подошёл:

Скопировать кодJAVASCRIPT

```
let catName;
const cartoon = 'Лето кота Леопольда';

switch (cartoon) {
    case 'Зима в Простоквашино':
        catName = 'Матроскин';
        break;
    default:
        catName = 'Леопольд';
}

console.log(catName); // "Леопольд" 
```

Если будет время, сделайте задание по теме. Для этого используйте любой текстовый редактор.

## Задание

Допишите код, чтобы перевести картину Виктора Васнецова «Витязь на распутье» на JavaScript. Используйте `switch-case`:

Скопировать кодJAVASCRIPT

```
const where = prompt('Куда едешь? Налево, направо или прямо?', '').toLowerCase();

console.log('Быть тебе женатым'); // если ответ "налево",
console.log('Живым не бывать'); // если "прямо",
console.log('Быть тебе богатым'); // если "направо". 
```

И не забывайте про `break`.