[[React]]

Второе важное преимущество классовых компонентов — методы обратного вызова для создания «побочных эффектов». Хук `useEffect` позволяет устанавливать аналогичные колбэки и в функциональных компонентах.

Снова сравним пример из предыдущего урока и его аналог с использованием хуков.

Скопировать кодJSX

```
// Пример из предыдущего урока
class NeonCursor extends React.Component {
  constructor(props) {
    super(props);

    this.state = { top: 0, left: 0 };
  }

    // Метод будет вызван сразу после монтирования: создаём эффекты
  componentDidMount() {
    document.addEventListener('mousemove', this.handleMouseMove);
    document.body.classList.add('no-cursor');
  }

    // Метод будет вызван непосредственно перед размонтированием: удаляем эффекты
  componentWillUnmount() {
    document.body.classList.remove('no-cursor');
    document.removeEventListener('mousemove', this.handleMouseMove);
  }

  handleMouseMove = (event) => {
    this.setState({
      top: event.pageY,
      left: event.pageX,
    });
  };

  render() {
    return (
      <img
        src="./cursor.png"
        width={30}
        style={{
          position: 'absolute',
          top: this.state.top,
          left: this.state.left,
          pointerEvents: 'none',
        }}
      />
    );
  }
}  
```

Скопировать кодJSX

```
// Перепишем код с использованием хуков
function NeonCursor() {
  const [position, setPosition] = React.useState({ top: 0, left: 0 });

  React.useEffect(() => {
    function handleMouseMove(event) {
      setPosition({
        top: event.pageY,
        left: event.pageX,
      });
    }

        // Список действий внутри одного хука
    document.addEventListener('mousemove', handleMouseMove);
    document.body.classList.add('no-cursor');

        // Возвращаем функцию, которая удаляет эффекты
    return () => {
      document.body.classList.remove('no-cursor');
      document.removeEventListener('mousemove', handleMouseMove);
    };
  });

  return (
    <img
      src="./cursor.png"
      width={30}
      style={{
        position: 'absolute',
        top: position.top,
        left: position.left,
        pointerEvents: 'none',
      }}
    />
  );
} 
```

Кроме того, что с помощью `React.useState` мы заменили `this.state` на единый объект `position`, произошло ещё одно изменение: управление эффектами переместилось внутрь колбэка, передаваемого в хук `React.useEffect`.

«Реакт» вызовет этот колбэк после того, как компонент будет смонтирован или обновлён — то есть этот колбэк является пересечением функциональности `componentDidMount` и `componentDidUpdate`. На практике такое объединение оказывается более удобным, однако существует способ и вернуть разделение. Его мы рассмотрим позже.

Чтобы «подчистить» результаты эффекта (когда компонент будет размонтирован), колбэк эффекта может вернуть ещё один колбэк — он будет использован движком «Реакта» по аналогии с `componentWillUnmount`. Такой колбэк обычно называется “cleanup” (от англ. «очистка»).

Обратите внимание, что сам код эффекта, его очистка и даже обработчик события `handleMouseMove` описаны рядом: внутри одного вызова `React.useEffect` — такой подход позволяет лучше организовать код. Кроме того, разработчик может создавать сколько угодно эффектов, если он хочет логически их разделить. Например, в нашем варианте мы могли бы добавить отдельные эффекты для подписки на событие `mousemove` и для установки класса `no-cursor`:

Скопировать кодJSX

```
React.useEffect(() => {
  function handleMouseMove(e) {
    setPosition({
      top: e.pageY,
      left: e.pageX,
    });
  }

  document.addEventListener('mousemove', handleMouseMove);

  return () => {
    document.removeEventListener('mousemove', handleMouseMove);
  };
});

React.useEffect(() => {
  document.body.classList.add('no-cursor');

  return () => {
    document.body.classList.remove('no-cursor');
  };
}); 
```

В случае с классовыми компонентами все инструкции должны быть расположены внутри методов жизненного цикла, что зачастую приводит к их плохой читаемости. Код с использованием функциональных компонентов и хуков лишён этого недостатка и в целом выглядит лаконичнее.