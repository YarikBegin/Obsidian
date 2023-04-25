[[JavaScript]]

Мы уже разобрали 3 привязки из 4: привязку по умолчанию, неявную привязку и явную. Остался четвёртый способ задать значение `this`: при вызове функции с оператором `new`. Напомним, что происходит при вызове функции с `new`:

![image](https://pictures.s3.yandex.net/resources/JS4_3___1__3_1593431558.jpg)

Оператор `new` используют, когда функцию нужно вызвать как конструктор:

Скопировать кодJAVASCRIPT

```
function Human(name) {
  this.name = name;
}

const bob = new Human('Bob');

console.log(bob) // Human { name: 'Bob' } 
```

Когда функцию вызывают с оператором `new`, движок автоматически выполняет три действия:

1.  Конструирует новый объект.
2.  Устанавливает этот объект в значение `this` для вызванной функции.
3.  Возвращает `this`.

Если вызвать функцию с `new`, `this` возвращается автоматически. Функция может ничего не возвращать или возвращать примитивное значение, но при её вызове с `new` всё равно вернётся `this`.

Но есть одно исключение. Если функция возвращает объект, вызов функции с `new` вернёт этот объект:

Скопировать кодJAVASCRIPT

```
function Plane(model) {
  this.model = model;

  return { model: 'TU-134' };
}

const airbus = new Plane('Airbus');

console.log(airbus); // { model: 'TU-134' } 
```

## Оператор `new` и методы `call`, `apply`, `bind`

Оператор `new` не работает вместе с методами `call`, `apply` и `bind`:

Скопировать кодJAVASCRIPT

```
function Driver(name, category) {
  this.name = name;
  this.category = category;
}

const Henry = new Driver.call(window, 'Henry', 'A'); // TypeError: Driver.call is not a constructor
const Peter = new Driver.apply(window, ['Peter', 'B']); // TypeError: Driver.apply is not a constructor
const Matt = new Driver.bind(window, 'Matt', 'C')(); // TypeError: Driver.bind is not a constructor 
```

Но мы можем явно указывать контекст для функции в уже созданном объекте:

Скопировать кодJAVASCRIPT

```
function Windows(name) {
  this.name = 'Windows 10';
  this.printInfo = function () {
    console.log('Your OS: ' + this.name);
  };
}

const Linux = {
  name: 'Ubuntu',
};

const theOs = new Windows();

theOs.printInfo(); // Your OS: Windows 10
theOs.printInfo.call(Linux); // Your OS: Ubuntu
theOs.printInfo.apply(Linux); // Your OS: Ubuntu
theOs.printInfo.bind(Linux)(); // Your OS: Ubuntu 
```