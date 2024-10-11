---
title: State Management System in Vue.js - Vuex and Pinia\
layout: doc
---

# State Management System in Vue.js - Vuex and Pinia

## Abstract

Store in Vue.js is a centralized, global state management system. Typically, it is used to share data across different components, making state management more maintainable. In class, Arvind introduced the modern store package **Pinia**, and here I would like to introduce **Vuex** as well, which also served as state management library in Vue.js.

## Version Compatibilty

- Vuex:
> Vue 2 uses Vuex 3, and Vue 3 uses Vuex 4. 

- Pinia:
> Pinia both support Vue 2 and Vue 3. 

> According to Pinia's documentation, they started by exploring how the next iteration of Vuex should be and implemented most of what they want in Vuex 5. It can be seen as a rebranded Vuex 5. Vuex developer also mentioned that Pinia is now the default for state management library.

## Usage

### Example Usage of Vuex

```js
import Vue from 'vue';
import Vuex from 'vuex';

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    count: 0
  },
  getters: {
    count(state) {
      return state.count;
    }
  }
  mutations: {
    increment(state) {
      state.count++;
    }
  },
  actions: {
    incrementAsync({ commit }) {
      setTimeout(() => {
        commit('increment');
      }, 1000);
    }
  },
});

```
The components can access or modify the state with the following basic structures::

1. **state**: includes the data that is stored and shared across components
2. **getter**: allows user to compute or filter state data
3. **mutations**: synchronous function to directly change the state
4. **actions**: allow for asynchronous operations before committing a mutation
> for example, in the code above `incrementAsync` calls a mutation function `increment`

### Example Usage of Pinia

```js
import { defineStore } from 'pinia';

export const useCounterStore = defineStore('counter', {
  state: () => ({
    count: 0
  }),

  getters: {
    doubleCount(state) {
      return state.count * 2;
    }
  }

  actions: {
    increment() {
      this.count++;
    },
    async incrementAsync() {
      setTimeout(() => {
        this.increment();
      }, 1000);
    }
  },

});
```
1. **state**: data you want to manage globally, similar comcept as Vuex
2. **getter**: also similar to Vuex, also similar to the computed properties in Vue, derive state or compute values based on the state
3. **actions**: operations that can modify state or perform side effects, which can be asynchronous or synchronous, which sort of serving as combination of **mutations** and **actions** in Vuex.


### Comparison (or why Pinia)

Here are some basic comparison between these two state management system.

1. Simiplicity in API:

The API in Pinia is more straightforward and modern, where Vuex requires calling the mutation operations. Note that in the Pinia example, `count` is defined in `state()` of the store already, we cannot create new property in this way if it is not defined.

```js
// Pinia
const store = useStore()
store.count++
```
```js
this.$store.commit('increment');
```

2. Mutations/ Actions
As mentioned above, Vuex has seperated the concept of mutations and actions, which represents synchronized and asynchronized operations respectively. Yet, Pinia's actions just allows all actions that can modify the state.

3. Lightweight
Pinia only weighs 1KB, which is easy to incorporate and may improve application performance.

4. Typescript Support
Pinia has full typescript support, including autocompletion for Javascript, which makes adding typescript in Pinia easier than in Vuex.

5. Pineapple (Pinia has a cute logo!)
(In https://pinia.vuejs.org/, the eyes of the pineapple move along with your cursor!)

### Summary

For beginners, Pinia is more flexible and is easier to learn, which is suitable for smaller projects. Yet, if one is working in a larger project that requires stricter structure, Vuex will probably be more suitable.
As for the context in 6.1040, where we will be working on a smaller scale project, Pinia will be more suitable!

## References:

https://dev.to/delia_code/a-beginners-guide-to-using-vuex-4egh
https://dev.to/bcostaaa01/how-does-vuex-work-and-what-to-use-it-for-4d8k
https://pinia.vuejs.org/
https://vuejsdevelopers.com/2023/04/11/pinia-vs-vuex---why-pinia-wins/
https://medium.com/@halitenes/what-is-pinia-vuex-vs-pinia-dcdd94917326