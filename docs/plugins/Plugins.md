# Plugins

Nuxt.js 允許在運行 Vue.js 應用程式之前執行 JS 插件。這需要使用自己的庫或第三方模組時特別有用。
可以將自訂插件注入到 Vue 實例（用戶端），context（伺服器端）、store(Vuex)。
新增的屬性或方法名使用 $ 作為前綴。

## 注入 Vue 實例

將內容注入Vue實例，避免重複引入，在 Vue 原型上掛載注入一個函數，所有組件內都可以訪問(不包含伺服器端)。

```js
// plugins/vue-inject.js :

import Vue from 'vue'
Vue.prototype.$myVueFunction = string => console.log('綁定到 Vue 實例的方法參數', string)
```

```js
// nuxt.config.js
export default {
  plugins: ['~/plugins/vue-inject.js']
}
```

這樣，您就可以在所有Vue組件中使用該函數。

```vue
// plugin-vue.vue

<script>
export default {
  mounted () {
  this.$myVueFunction('test')
}
</script>
```

## 注入 context

context 注入方式和在其它 Vue 應用程式中注入類似。

```js
// plugins/ctx-inject.js :
export default ({ app }, inject) => {
  app.myContextFunction = string => console.log('綁定到 Context 的方法參數', string)
}
```

```js
// nuxt.config.js :
export default {
  plugins: ['~/plugins/ctx-inject.js']
}
```

現在，只要獲得 context，就可以使用該函數。

```vue
// plugin-ctx.vue :
<script>
export default {
  asyncData (context) {
    context.app.myContextFunction('ctx!')
  }
}
</script>
```

## 同時注入在 context ， Vue ， Vuex 實例

如果您需要同時在 context、Vue 實例，甚至 Vuex 中同時注入，可以使用 inject 方法,它是 plugin 導出函數的
第二個參數。 將內容注入 Vue 實例的方式與在 Vue 應用程式中進行注入類似。系統會自動將 $ 添加到方法名的前
面。

```js
// plugins/all-inject.js :
export default ({ app }, inject) => {
  inject('myAllFunction', string => console.log('綁定到 Context 和 Vue 的方法參數', string))
}
```

```js
// nuxt.config.js :
export default {
  plugins: ['~/plugins/all-inject.js']
}
```

現在您就可以在 context ，或者 Vue 實例中的 this ，或者 Vuex 的 actions/mutations 方法中的 this 來調用
myAllFunction 方法。

```vue
// plugin-all.vue
<script>
export default {
  mounted () {
  this.$myAllFunction('mounted 鉤子調用 myAllFunction')
  },
  asyncData (context) {
  context.app.$myAllFunction('asyncData 調用 myAllFunction')
  }
}
</script>
```

## 只在瀏覽器端使用的插件

```js
export default {
  plugins: [
      { src: '~/plugins/combined-inject.js' },
      { src: '~/plugins/combined-inject.js', mode: 'client' }, // 插件只會在用戶端運行。
      { src: '~/plugins/combined-inject.js', mode: 'server' }, // 插件只會在服務端運行。
    ]
}
```