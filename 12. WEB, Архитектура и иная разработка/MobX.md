**MobX** — библиотека, помогающая просто и гибко управлять состоянием приложения, основанная на подходе прозрачного реактивного функционального программирования (Transparent functional reactive programming, TFRP).  

Сам по себе *MobX не является архитектурой или хранилищем состояния*, но обладает функционалом, с помощью которого можно это всё построить, из-за чего его часто считают альтернативой Redux.  

## Основной принцип MobX
> Всё, что может быть извлечено (be derived) из состояния приложения, должно быть извлечено. Автоматически.  

Речь идёт о *текущем состоянии приложения*: при его обновлении должно автоматически обновиться всё, что его использовало.

Суть того, к чему *стремится реактивное программирование*, можно показать на примере (достигается при помощи Observable).
```js
a = 5
b = 7
c = a + b // с = 12

b = 10 // c = 15
a -=4 // c = 11
```

## Особенности MobX

* *Однонаправленный поток данных*:  
**Actions -> State -> Derivations (computed values) -> Reactions**.
* *State* представлен любым Observable, изменения которого отслеживаются при помощи Reactions; из-за этого State децентрализован, раздроблен.
* *State* *изменяем* (mutable).
* Нужные *Derivations* пересчитываются и *Reactions* вызываются автоматически при изменении *State*.

## Структура MobX

### State

**State** — *данные приложения* (объекты, массивы, примитивы), составляющие в совокупности Модель приложения (как в подходах по типу MVC).  
 
Любое значение, которое в приложении имеет тип Observable и его изменения отслеживаются при помощи Reactions, является частью State.

<!--Чтобы была возможность подписаться на обновление значения, нужно задать ему тип Observable. -->

Синтаксис:
* `observable(value)` (только для массивов и объектов)
* `observable.box(value)` (для примитивных значений)
* `@observable property = value` (только в классах)

```js
import { observable } from 'mobx';

// массивы и объекты
const items = observable([1, 2, 3]);
const item = observable({ title: 'Foo', description: 'Bar' });

// примитивы
const string = observable.box('string');
const number = observable.box(7);
const boolean = observable.box(true);

// Observable.box создаёт объект. Для получения примитивного значения нужно использовать метод get()
console.log(number.get()) // 7

// внутри классов можно использовать декораторы (официально пока ещё экспериментальные) или функции
class ItemStore {
  @observable item = { title: 'Foo', description: 'Bar' }
  @observable string = 'string' // в декораторе примитив без .box
  /* или */
  item = observable({ title: 'Foo', description: 'Bar' })
  string = observable.box('string')
}
```

### Action

**Action** — всё, что изменяет State.  

В отличие от архитектуры Flux и её производных, MobX не ставит ораничений на то, как должны быть обработаны события пользователя.

Простые Actions для массивов и объектов
```js
const item = observable({ title: 'foo', description: 'bar' });
const items = observable([1, 2, 3]);

// Actions
item.title = 'new';
item.title = `${item.title} title`;
item.description = 'new description';
items.pop()
items.push(4)
items[1] = 8;
```
Простые Actions для примитивов:
```js
let number = observable.box(1);

// Action
number.set(3); // в консоль выведется 'Changed to 3'

// как ниже делать нельзя, поскольку это перезапишет переменную number, сделав её обычным примитивом
number = 2; // в консоль не выведется ничего
number.set(4); // TypeError: number.set is not a function
```
MobX сам заботится, чтобы все изменения State, произошедшие при помощи Action, автоматически обработались в Derivations и Reactions.

Если строить архитектуру управления состоянием при помощи MobX, то лучше всего использовать встроенный функционал `action`.
* `action(fn)`
* `action(name, fn)`
* `@action classMethod()`

В этом случае так же следует запретить любые изменения State (то есть изменения любого Observable) вне Actions.
```js
import { configure } from 'mobx';

configure({ enforceActions: 'always' });
```
С такой конфигурацией простые примеры выше с Actions не будут работать (после появления Reactions), поскольку будет ошибка: `Error: [mobx] Since strict-mode is enabled, changing observed observable values outside actions is not allowed`.  

Нужно изменить код следующим образом:
```js
import { observable, action } from 'mobx';

const item = observable({ title: 'foo', description: 'bar' });
const items = observable([1, 2, 3]);

const changeItem = action(({ title, description } = {}) => {
  item.title = title;
  item.description = description;
  
  // нельзя переприсваивать, поскольку item перестанет быть Observable
  item = { title, description };
});

const addItem = action(item => items.push(item));
```
```js
let number = observable.box(1);
// set - автоматически является action, но если его добавить в функцию, то это уже работать не будет
number.set(3); // может работать
const set = value => number.set(value); // не работает
const set = action(value => number.set(value)); // работает
```


### Derivation

**Derivation (computed values)** — любое значение, которое может вычисляется автоматически после обновления State.  
Derivation может быть переменной, UI-компонентой и многим другим.  
Derivations используются в приложении, как и Observables.  

На практике Derivation — результат выполнения функции, которая имеет тип Computed.  
В этой функции не должно быть сайд-эффектов (side effects).

Синтаксис:
* `computed(() => derivation)`
* `@computed get property() { return derivation; }` (только в классе)

```js
const statement = observable.box(3 > 1); // true

const oppositeStatement = computed(() => !statement.get());

console.log(oppositeStatement); // false
trueStatement.set(false);
console.log(oppositeStatement); // true
```

### Reaction

**Reaction** — это функция, которая запускается автоматически после обновления State.  
Reaction похож на Derivation, но вместо генерации нового значения в нём обрабатываются сайд-эффекты: вывод в консоль, запросы к серверу и прочее.

```js
// Автоматически вызывается для всех Observable значений, использующихся в сайд-эффектах
autorun(() => { /* side effects here */ }));

// подписка на изменения
autorun(() => console.log(item)); // сработает только при инициализации (ссылка на объект не меняется) и выведет в консоль Observable
autorun(() => console.log('Title changed', item.title)); // при обновлении поля поля title объекта item
autorun(() => console.log('Title or description changed', item.title, item.description)); // при обновлении одного из полей title или description объекта item
autorun(() => console.log('Data changed', { ...item })); // при обновлении любого поля объекта item

// подписка на изменения
// Observable — объект. Чтобы получить значение переменной number, нужно использовать number.get()
autorun(() => console.log('Changed to', number.get()); 

autorun(() => { console.log('Item changed', item) });

when(() => condition, () => { /* side effects here */ });

reaction(() => data, data => { /* side effects here */ });

reaction(
  () => arr.map(item => [item.a, item.b]),
  data => console.log("CHANGED A,B:", data)
);
```

При использовании MobX с React наиболее популярный Reaction — `observer`.  
Он оборачивает метод класса `render()` или функциональный компонент в `autorun`, тогда в случае изменения любого Observable внутри них, компонент автоматически перерендерится.  
```jsx
import { Fragment } from 'react';
import { render } from 'react-dom';
import { observable } from 'mobx';
import { observer } from 'mobx-react';

const state = observable({
  number: 0,
});

const increment = () => {
  state.number += 1;
};

// пример функциональной компоненты
const Number = observer(({ state }) => (
  <button onClick={increment}>Click me to increase: {state.number}</button>
));

// пример класса
@observer
class NumberClass extends React.Component {
  render() {
    return <button onClick={increment}>Click me to increase: {this.props.state.number}</button>;
  }
}

const App = () => (
  <Fragment>
    <Number state={state} />
    <NumberClass state={state} />
  </Fragment>
);

render(<App />, document.getElementById('root'));
```
