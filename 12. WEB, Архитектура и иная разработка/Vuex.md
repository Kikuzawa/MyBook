**Vuex** — *шаблон управления состоянием* (state management pattern) приложения, а также *библиотека*, разработанная для Vue.js приложений. 
 
## Особенности Vuex
* *Однонаправленный поток данных*:  
**Action -> Mutations -> State -> Views**.  
Action создаётся при взаимодействии пользователя со View, но может создаваться и самим приложением.
* Может быть *только один State*, но в нём могут быть выделены отдельные *независимые блоки* — *Modules*.
* *State* *изменяем* (mutable), *единственный способ* его *изменить* — *совершить* (commit) *Mutation*.
* *Action* может использовать *асинхронные функции*, *Mutation* — не может.

## Структура Vuex
 
**State** — *объект*, в котором *хранится состояние приложения*.  
*Располагается* в *Store* и *инициализируется* вместе с ним.  
```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);
const store = new Vuex.Store({
  state: {
    items: [],
  },
});
```
Чтобы *State* был *доступен во всех компонентах*, нужно *встроить Store* в приложение.
```js
  const app = new Vue({
    /* ... */
    store,
  });
```
Использование *State внутри компонент*: `this.$store.state`.
```js
const Items = {
  template: `<ul>
  <li v-for="item in items">
    {{ item }}
  </li>
</ul>`,
  computed: {
    items () {
      return this.$store.state.items,
    }
  }
}
```

**Getter** — *вспомогательная функция*, *принимающая State* и *возвращающая* некоторую его *часть* или же *вычисляющая* на его основании *что-то новое*.
*Параметры Getter*: `(state, getters)`, `state` — *ссылка* на `store.state`, `getters` — другие Getters, которые можно использовать.
```js
const store = new Vuex.Store({
  /* ... */,
  getters: {
    itemsLength: state => state.items.length,
    numbers: state => state.items.filter(item => typeof item === 'number'),
    getItemByIndex: state => index => state.items[index],
  },
});
```
*Использование Getters*.
```js
store.getters.itemsLength;
store.getters.getItemByIndex(0);

// в компоненте
this.$store.getters.numbers;
```

**Mutation** — *функция*, в которой происходит *изменение State*.  
*Единственный способ изменить State* — *совершить* (commit) *Mutation*.  
*Mutation* — *синхронная транзакция*, для *асинхронности* используется *Action*.  
*Параметры Mutation*: `(state, payload)`, `state` — *ссылка* на `store.state`, `payload` — *объект с* переданными *параметрами*.
```js
// mutation-types.js
export const ADD_ITEM = 'ADD_ITEM';
```
```js
// store.js
import { ADD_ITEM } from './mutation-types';

const store = new Vuex.Store({
  /* ... */,
  mutations: {
    [ADD_ITEM] (state, payload) {
      // мутируем State (не можем испозовать state.items.push(), потому
      // что должна измениться ссылка на массив, произойти мутация)
      state.items = [...state.items, item];
    },
  },
});
```
*Совершение Mutation*: `commit(type, payload)`.
```js
// state: { items: [] }
store.commit(ADD_ITEM, { item: 7 });
// или
store.commit({ type: ADD_ITEM, item: 7 });
// state: { items: [7] }

// в компоненте
this.$store.commit(ADD_ITEM, { item: 7 });
```

**Action** — *функция*, которые *совершает* (commit) в своём теле *Mutations* и может выполнять *асинхронные операции*.  
*Параметры Action*: `(context, payload)`, `context` — объект, который содержит:
* `state` — то же, что и `store.state`
* `commit(type, payload)` — функция для *совершения Mutation*
* `dispatch(type, payload)` — функция для *отправки Action*
* `getters` — *Getters*

```js
import api from '/* ... */'; // связующее звено между клиентом и сервером

const store = new Vuex.Store({
  /* ... */,
  actions: {
    /* синхронный Action */
    addItem (context, payload) {
      context.commit('addItem', payload.item);
    },
    /* асинхронный Action */
    async fetchItem (context) {
      const item = await api.get(/*...*/); // получение item откуда-либо, допустим получили 7
      
      context.dispatch('addItem', { item });
      // или
      context.dispatch({ type: 'addItem', item });
    },
  },
});
```
*Отправка* (dispatch) *Action*: `dispatch(type, payload)`.
```js
// state: { items: [] }
store.dispatch('addItem', 5);
// state: { items: [5] }
store.dispatch('fetchItem');
// state: { items: [5, 7] }

// в компоненте
this.$store.dispatch('fetchItem');
```

**View** — UI-компонента (Vue.js) 

**Modules** позволяют создавать независимые блоки глобального State, в которых есть свои локальные State, Actions, Getters и Mutations. 
```js
const itemModule = {
  state: { /* ... */ },
  mutations: { /* ... */ },
  actions: { /* ... */ },
  getters: { /* ... */ },
};

const userModule = {
  state: { /* ... */ },
  mutations: { /* ... */ },
  actions: { /* ... */ },
  getters: { /* ... */ },
};

const store = new Vuex.Store({
  modules: {
    item: itemModule,
    user: userModule,
  }
});

store.state.item // item's state
store.state.user // user's state
```
Структура локальных Mutations, Actions, Getters такая же, только теперь в качестве `state` берётся локальный State.  
В локальных Actions и Getters есть возможность обратиться к глобальным State и Getters при помощи `rootState` и `rootGetters`.  
```js
const itemModule = {
  state: {
    items: [],
  },
  getters: {
    itemsLength (state, getters, rootState, rootGetters) { /* ... */ },
  },
  mutations: {
    ADD_ITEM (state, payload) {
      state.items = [...state.items, item];
    },
  },
  actions: {
    addItem (context, payload) {
      context.rootGetters;
    },
  },
};
```

По умолчанию Actions, Getters, Mutations вызываются так, словно они были определены в глобальном State.  
Это удобно, если все их названия уникальны, но может стать проблемой в большом приложении, где названия могут совпадать.  
```js
store.commit('ADD_ITEM');
store.dispatch('addItem');
store.getters.itemsLength;
```
Чтобы это исправить, можно замкнуть функционал в Module, задав ему namespace.
```js
const itemModule = {
  namespaced: true,
  /* ... */
};

const store = Vuex.Store({
  modules: {
    item: itemModule,
  },
});
```
Тогда в обращение добавится префикс с названием модуля, а предыдущее обращение перестанет работать.
```js
store.commit('item/ADD_ITEM');
store.dispatch('item/addItem');
store.getters['item/itemsLength'];
```
<!-- Обращения вроде `store.item.commit()` не работают. -->

Modules можно подключать и удалять динамически (многие Vue.js плагины подключают свои Modules так к приложению).
```js
const adminModule = { /* ... */ };

/* подключение модуля */
store.registerModule('user', userModule);
/* удаление модуля */
store.unregisterModule('user');
```
Module может содержать другие Modules.  
```js
const moderatorModule = { /* ... */ };

const userModule = {
  modules: {
    moderator: moderatorModule,
  },
};
```

В больших приложениях State может сильно разрастаться (становится неудобно и сложно его поддерживать), модули помогают решить эту проблему и сделать приложение более расширяемым.  
