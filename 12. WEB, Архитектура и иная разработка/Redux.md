# Redux

**Redux** — библиотека, основанная на подходе Flux и вносящая в него некоторые правки.

## Три основных принципа Redux
* Только *один источник правды* (single source of truth) — *Store*.  
* *Состояние* доступно *только для чтения* (state is read-only). Его можно *изменить* лишь с помощью *Actions*.
* *Изменение состояния* происходит только с использованием *чистых функций* (pure functions) — *Reducers*.

## Другие особенности Redux
* *Однонаправленный поток данных*:  
**Action Creator -> Action -> Reducers -> Store -> Views**.  
* *Store* только один, но он может разбиваться на части, за каждую из которых отвечает отдельный Reducer.
* *Store* должен быть *неизменяемым* (immutable). Reducer не изменяет state напрямую, а работает с копией и возвращает её.
* *Отсутствует Dispatcher*, вместо него используется функция `store.dispatch()`.
* В схеме Redux изначально нет места асинхронным функциям, но есть возможность это исправить, подключив middleware (thunk, saga).

## Структура Redux

Части **Action**, **Action Creator**, **Store** и **View** определяются аналогично подходу Flux за исключением того, что в Redux может быть только один Store, а Action Creator не может вызывать функцию dispatch, при необходимости за него это делает **Bound Action Creator**:
```js
const ADD_ITEM = 'ADD_ITEM';

/* Action Creator */
const addItem = item => ({ type: ADD_ITEM, item });
dispatch(addItem(5)); // dispatch action ADD_ITEM

/* Bound Action Creator */
const boundAddItem = item => dispatch(addItem(item));
boundAddItem(5) // dispatch action ADD_ITEM
```

**Reducer** — *чистая функция*, определяющая, как изменится состояние приложения (state) в ответ на *Action*.  
Reducer имеет вид: `(previousState, action) => newState`, что напоминает функцию `reduce()`, откуда и название.  
```js
const reduce = (accumulator, currentValue) => accumulator + currentValue;
```

```js
const initialState = {
  items: [],
};

const itemReducer = (state = initialState, action) => {
  switch (action) {
    case ADD_ITEM:
      return ({
        ...state,
        items: [
          ...state.items,
          action.item,
        ],
      });

    default:
      return state;
  }
};
```
Почему используется деструктуризация `...` и нельзя просто напрямую изменить значение в state?  
Потому что объекты и массивы в JavaScript передаются по ссылке.  
Если изменить их значение напрямую, то произойдёт мутация и Redux не заметит изменений, а значит оно не дойдёт до View в нужный момент.

Когда Action не обрабатывается в текущем Reducer, то он попадает в блок default.  
Состояние должно остаться без изменений, поэтому оно возвращается без деструктуризации.  

В Redux может быть несколько Reducers, каждый из которых отвечает за какую-то часть State.  
Reducers комбинируются в главный Reducer, который передаётся в Store при создании:
```js
import { createStore, combineReducers } from 'redux';

/* ... */

const reducer = combineReducers({
  item: itemReducer,
  another: anotherReducer,
});

const store = createStore(reducer);
/*
const store = {
  item: { ... },
  another: { ... },
};
*/
```

Как и в случае Callbacks из Flux, все Reducers получают приходящий Action.  
Тем не менее между Reducer и Callback есть существенное отличие:  
Callback не принимает в параметрах и не возвращает state (как это делает Reducer), вместо этого он изменяет state напрямую.  
```js
const itemState = {
  items: [],
};

const itemCallback = (action) => {
  if (action.type === ADD_ITEM) {
    itemState.items = [
      ...items,
      action.item,
    ];
  }
};
```

В Redux отсутствует Dispatcher, его работу берёт на Store, предоставляя функцию **dispatch**.
```js
// item's state: { items: [] }
store.dispatch(addItem(7));
// item's state: { items: [7] }
console.log(store.getState())
```


## Redux Thunk vs Redux Saga

### Redux Thunk

**Thunk** — функция, которая оборачивает выражение, чтобы отложить его выполнение.  

```js
// обычный Action Creator
const doSomething = params => ({ type: 'DO_SOMETHING', ...params });

// обычный Bound Action Creator
const boundDoSomething = params => dispatch({ type: 'DO_SOMETHING', ...params });
```
В обоих случаях нет места асинхронным функциям.

Также можно заметить, что функция `dispatch(action)` сама по себе *возвращает Action*, то есть результат вызова функций `doSomething()` и `boundDoSomething()` одинаков: возвращается Action типа `DO_SOMETHNG`.

Библиотека `redux-thunk` вместо возвращения Action или вызова `dispatch(action)` возвращает функцию, что позволяет отложить вызов функции `dispatch(action)`, а перед вызовом проводить какие-то дополнительные операции, в том числе и асинхронные.
```js
// Action Creator при использовании redux-thunk
const doSomething = params => dispatch => dispatch({ type: 'DO_SOMETHING', ...params });

// Action Creator c асинхронной операцией при использовании redux-thunk
const doSomething = params => async (dispatch) => {
  await new Promise(resolve => setTimeout(resolve, 1000));
  return dispatch({ type: 'DO_SOMETHING', ...params });
};
```
Приятным бонусом является то, что в функции мы можем контролировать возвращаемое значения, поскольку не обязательно возвращать результат выполнения `dispatch(action)`).
```js
const doSomething = params => async (dispatch) => {
  await new Promise(resolve => setTimeout(resolve, 1000));
  dispatch({ type: 'DO_SOMETHING', ...params });
  const data = { /* ... */ };
  return data;
};
```
Таким образом можно вернуть какие-то данные в компоненту (например, пойманные ошибки).

### Redux Saga

**Saga** — архитектурный паттерн их микросервисной архитектуры для реализации транзакции, охватывающей (span) несколько сервисов. 

Внутри сервиса доступны ACID-транзакции, но между несколькими сервисами их использовать нельзя, поэтому и используется *Saga*, последовательно запускающая локальные транзакции в каждом сервисе. В случае, если какая-нибудь локальная транзакция из последовательности не проходит, Saga отменяет всю последовательность при помощи компенсирующих транзакций.

Redux Saga реализована как промежуточный слой (middleware), позволяющий проделывать сайд-эффекты (например, запросы к серверу), при помощи ES6-генераторов.

Функция-генератор возрващает объект-итератор. Каждый вызов метода итератора `next()` выполняет код до следующего выражения `yield` и останавливается.
```js
function* addTwo(x) {
  yield x;
  for (let i = 0; i < 2; i++) {
    yield x += 1;
  }
}

let it = addTwo(0);
console.log(it.next()); // { value: 0, done: false }
console.log(it.next()); // { value: 1, done: false }
console.log(it.next()); // { value: 2, done: false }
console.log(it.next()); // { value: undefined, done: true }
```
Для началы работы с сагами необходимо создать сагу-наблюдателя и сагу-рабочего.

**Сага-наблюдатель** (Watcher Saga) следит за Actions и создаёт новую задачу на каждый отправленный Action при помощи вспомогательной функции `takeEvery`. 
```js
import { takeEvery } from 'redux-saga/effects'

function* watchFetchArticles() {
  yield takeEvery('FETCH_ARTICLES', fetchArticles);
}
```

**Эффект** (Effect) — простой JavaScript-объект (plain JS Object), содержащий необходимые инструкции, которые должен выполнить `redux-saga middleware`. Например: отправить (dispatch) Action, вызвать асинхронную функцию.

**Сага-рабочий** (Worker Saga) использует эффекты, чтобы обработать Action.
```js
import axios from 'axios';
import { call, put } from 'redux-saga/effects'

function* fetchArticles() {
  try {
    const articles = yield call(axios.get, '/articles');
    yield put({ type: 'FETCH_SUCCESS', payload: articles });
  } catch (error) {
    yield put({ type: 'FETCH_ERROR', payload: error });
  }
}
```

В примере выше используются два основных эффекта.  
Первым эффектом является `call(fn, ...args)`, описывающий асинхронный HTTP-запрос.
```js
{
  CALL: {
    fn: axios.get,
    args: ['/articles']
  }
}
```
Вторым — `put(action)`, заменяющий `dispatch`.
```js
{
  PUT: {
    action: { type: 'FETCH_SUCCESS', payload: articles }
  }
}
```

Таким образом, вместо вызова сайд-эффектов напрямую
```js
function* fetchArticles() {
  const articles = yield axios.get('/articles');
  dispatch({ type: 'FETCH_SUCCESS', articles })
}
```
используются Эффекты, являющиеся простыми объектами, что вносит большую гибкость и позволяет с лёгкостью их тестировать.

Если есть несколько отправленных одновременно Actions, то `takeEvery` вызывает несколько экземпляров саги-рабочего, наделяя саги свойством параллелизма (concurrency).

## Использование React с Redux

### Возможная структура

Для маленьких проектов и быстрых решений:
```
  - resources/
  -- item.js
  -- store.js
```

Хорошо расширяемая структура (разбиение по смыслу):
```
 - resources/
 -- item/
 --- item.actions.js
 --- item.reducer.js
 --- item.selectors.js
 --- item.types.js
 -- store.js
```

Также расширяемая, но очень удобная (разбиение по типу файлов):
```
 - resources/
 -- actions/
 --- item.actions.js
 -- reducers/
 --- item.reducer.js
 -- selectors/
 --- item.selectors.js
 -- types/
 --- item.types.js
 -- store.js
```
### Настройка

1) Устанавливаем зависимости
```npm
npm i redux react-redux redux-thunk
```

1) Создаём основные компоненты:

Types
```js
export const DELETE_ITEM = 'DELETE_ITEM';
```

Actions (Actions, Action Creators, Bound Action Creators)
```js
/* item.actions */
import { DELETE_ITEM } from './item.types';

const deleteItem = id => dispatch => dispatch({ type: DELETE_ITEM, id });
```

Reducer
```js
/* item.reducer.js */

const initialState = {
  items: [],
  /* ... */
};

const itemReducer = (state = initialState, action) => { /* ... */ };

export default itemReducer;
```

Selectors (вспомогательные функции для получения некоторой части state)
```js
/* item.selectors */

/* state - все данные в Store
   item - название Reducer, отвечающего за данные для этой компоненты
   items - массив элементов */
const getItemById = (state, id) => state.item.items.find(item => item.id === id);
const getItemsLength = state => state.item.items.length;
```

1) Создаём Store, комбинируя Reducers и добавляя redux-thunk в качестве middleware:
```js
/* store.js */
import {
  createStore,
  combineReducers,
  applyMiddleware,
} from 'redux';
import thunk from 'redux-thunk';
import { itemReducer } from './item/item.reducer';

const reducer = combineReducers({ /* ... */ });

const store = createStore(reducer, applyMiddleware(thunk));

export default store;
```

1) Оборачиваем главную компоненту `<App>` компонентой `<Provider>`, в которую передаём созданный Store.
```jsx
import React from 'react'
import { render } from 'react-dom'
import { Provider } from 'react-redux'
import App from './App'
import store from './store'

const AppContainer = () => (
  <Provider store={store}>
    <App />
  </Provider>
);

render(<AppContainer />, document.getElementById('root'));
```

1) Оборачиваем компоненту, которую хотим подключить к Redux, компонентой высшего порядка при помощи функции `connect(mapStateToProps, mapDispatchToProps)`.
* `mapStateToProps(state, props)` отвечает за передачу части Store в props оборачиваемой компоненте (эта часть будет передаваться каждый раз после обновления в Store).
* `mapDispatchToProps` отвечает за оборачивание переданных ему Action Creators в функцию dispatch (чтобы в оборачиваемой компоненте не нужно было вызывать dispatch).

```jsx
import { connect } from 'react-redux';
import { deleteItem } from 'resources/item/item.actions';
import { getItemsLength, getItemById } from 'resources/item/item.selectors';

const ItemView = ({
  item,
  itemsLength,
  dispatchDeleteItem
}) => (
  <>
    <div class="item">{item}</div>
    <div class="index">`Item 1 of ${itemsLength}`</div>
    <button onClick={dispatchDeleteItem}> 
  </>
);

// пусть props.id - параметр, который передаётся от родительской компоненты или от роутера
const mapStateToProps = (state, props) => ({
  item: getItemById(state, props.id),
  itemsLength: getItemsLength(state),
});

const mapDispatchToProps = {
  dispatchDeleteItem: deleteItem,
};

export default connect(mapStateToProps, mapDispatchToProps);
```
