[[Дебаггинг JS. Введение]]
[[Типы ошибок]]


В прошлом уроке мы разобрали ошибки, которые ломают код. Если их совершить, программа не будет работать. Но есть более коварный тип ошибок — логические. Они не приводят к поломке кода — он выполняется как ни в чём не бывало, красный текст в консоли не возникает. Но программа выдаёт неверные результаты.

Вот функция для перевода температуры из градусов Фаренгейта в градусы Цельсия:
```js
function fahrToCelsius(fahr) {
    return fahr - 32 * 5/9;
}

fahrToCelsius(50); // Должно получиться 10
// Результат:
// 32.22222222222222
```
Код отработал и никаких ошибок в консоли не появилось, но результат неверный. Проблема в том, что мы забыли про скобки. Правильная формула для перевода температуры такая:
```js
function fahrToCelsius(fahr) {
    return (fahr - 32) * 5/9; // добавили скобки
}

fahrToCelsius(50); // 10. Вот теперь всё хорошо
```
Это и есть логическая ошибка: код всё делает правильно, но делает не то, что нужно.

Находить логические ошибки сложно. Обычно в программе много функций и нужно понять, какая приводит к неверному результату. Приходится отслеживать работу каждой. Есть два способа это сделать. О первом расскажем в этом уроке, а о втором — в следующем.

### Вывод значений в консоль: метод console.log

Можно выводить в консоль результат работы каждой функции и так понять, которая из них возвращает неправильный результат.

```js
// Опишем каждое действие для перевода
// температуры в отдельной функции

// Эта функция будет отнимать от температуры
// в Фаренгейтах 32 градуса
function diff(temp) {
    let res = temp - 32;
    return res;
}

// Эта функция будет умножать разницу на 5/9
function multiply(deltaTemp) {
  let res = deltaTemp/5*9;
  return res;
}

// Эта функция применит две другие
// и вернёт температуру в Цельсиях
function fahrToCelsius(temp) {
  return temp + ' градусов по шкале Фаренгейта равны ' + multiply(diff(temp)) + ' градусов Цельсия';
}

console.log(fahrToCelsius(100)); // Должно получиться 37.77777777777778

// "100 градусов по шкале Фаренгейта равны 122.39999999999999 градусов Цельсия"
```

Что-то не так. Поставим вывод промежуточных значений в консоль:
```js
function diff(temp) {
    let res = temp - 32;
    console.log(res);
    return res;
}

function multiply(deltaTemp) {
  let res = deltaTemp/5*9;
    console.log(res);
  return res;
}

function fahrToCelsius(temp) {
  return temp + ' градусов по шкале Фаренгейта равны ' + multiply(diff(temp)) + ' градусов Цельсия';
}

console.log(fahrToCelsius(100));

/*
  В консоли появятся сообщения:
  68 - это верно
  122.39999999999999 - вот тут уже что-то не так
  100 градусов по шкале Фаренгейта равны 122.39999999999999 градусов Цельсия
*/
```
Теперь понятно, что мы ошиблись в коде функции `multiply`: вместо умножения на 5 и деления на 9, она делит на 5 и умножает на 9.

Метод `console.log` очень удобен, но и у него есть недостаток. Изменять код в больших проектах довольно долго. Чтобы оптимизировать сайт и сделать его кросс-браузерным, разработчики пропускают код через специальные программы. Но они работают долго, а ради каждого `console.log` не хочется ждать по пять минут.

Существуют более удобный способ отладки. О нём расскажем чуть дальше.
