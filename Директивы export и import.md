[[Модули в JS. Введение]]

Теперь разберёмся с директивами `export` и `import`.

## Экспорт в момент создания

Переменные, функции и классы можно экспортировать в момент создания. Для этого перед их созданием добавляют директиву `export`:

Скопировать кодJAVASCRIPT

```
// экспорт переменной
export let str = 'Я буду на улице';
export const date = [12, 22, 31];

// экспорт функции
export function giveMeSomeInternet() {
  return 'Internet';
}

// экспорт класса
export class Song {
  constructor() {
        
  }
} 
```

При импорте такого значения мы используем имя, которое дали при создании. Поэтому экспорт в момент объявления называют именованным экспортом.

## Экспорт после создания

Можно экспортировать несколько переменных и функций сразу. То, что извлекается из модуля, нужно перечислить через запятую в фигурных скобках:

Скопировать кодJAVASCRIPT

```
const array = [1, 2, 3, 4];
const arrSquared = arr.map(item => item * item);

// экспорт нескольких значений
export { array, arrSquared }; 
```

## Экспорт с другим именем: директива `export as`

Переменные и функции можно переименовывать при экспорте, чтобы обращаться к методам модуля по новому имени:

Скопировать кодJAVASCRIPT

```
// constants.js

const array = [1, 2, 3, 4];
const arrSquared = arr.map(item => item * item);

// переименовали при экспорте
export { array as arr, arrSquared as sq }; 
```

Скопировать кодJAVASCRIPT

```
// index.js

// в импорте используем новые имена
import { arr, sq } from './constants.js'; 
```

## Директива `import`

Импортировать объекты тоже можно пачками. Переменные, которые добавляются в модуль, нужно перечислить через запятую в фигурных скобках:

Скопировать кодJAVASCRIPT

```
// index.js

// импорт нескольких переменных из файла data.js
import { array, arrSquared } from './data.js'

console.log(array); // [1, 2, 3]
console.log(arrSquared); // [1, 4, 9] 
```

Если импортировать нужно всё, что экспортирует модуль, имена объектов можно не перечислять, а просто поставить `*`:

Скопировать кодJAVASCRIPT

```
// index.js
import * as data from './data.js';

// из файла data.js будет импортировано всё, что из него экспортируется

console.log(data.array); // [1, 2, 3]
console.log(data.arrSquared); // [1, 4, 9] 
```

Но лучше не импортировать через `*`. Такой код сложнее читать: не видно, что конкретно импортируется из файла `data.js`.

## Имя модуля: директива `import as`

Длинные имена модулей можно сокращать и при импорте:

Скопировать кодJAVASCRIPT

```
// index.js
import { array as arr, arrSquared as sq } from './data.js'

console.log(arr); // [1, 2, 3]
console.log(sq); // [1, 4, 9] 
```

## Экспорт и импорт по умолчанию

Из модуля можно возвращать одно или несколько значений. Если нужно экспортировать один класс или функцию, лучше использовать экспорт по умолчанию.

Тогда после директивы `export` пишут `default`, а дальше — значение, которое нужно экспортировать.

Отличие импорта по умолчанию — фигурные скобки не ставятся:

Скопировать кодJAVASCRIPT

```
// render-items.js

export default function renderItems() {
  // код функции
} 
```

Скопировать кодJAVASCRIPT

```
// index.js

import renderItems from './render-items.js';

renderItems(); 
```

Здесь не важно, как называется функция в файле экспорта. Она может быть вообще анонимной, что невозможно при обычном экспорте. При экспорте по умолчанию её имя не играет роли:

Скопировать кодJAVASCRIPT

```
// этот код в разных файлах

// render-items.js
export default function render() {
  // ...
}

// song.js
export default class {
  constructor() { }
}

// data.js
export default [12, 22, 31]; 
```

Имена дают экспортированным данным позже, уже при импорте:

Скопировать кодJAVASCRIPT

```
import renderItems from './render-items.js';
import Song from './song.js';
import someArr from './data.js'; 
```

## Шпаргалка

Виды экспорта:

-   `export default` — по умолчанию. Такой экспорт может быть только один в файле модуля;
-   `export const array = [1, 2, 3]` — именованный экспорт. В файле их может быть несколько;
-   `export { dog, cat }` — сразу несколько сущностей можно экспортировать после их объявления.

Виды импорта:

-   `import { array } from './data.js'` — с именем сущности;
-   `import { array as arr } from './data.js'` — с переименованием сущности;
-   `import data from './data.js'` — по умолчанию. Фигурные скобки не нужны, имя даётся в момент импорта;
-   `import *` — всего содержимого сразу. Но так лучше не делать.