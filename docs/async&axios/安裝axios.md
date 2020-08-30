# 安裝 @nuxtjs/axios

> 官網: https://axios.nuxtjs.org/

Nuxt.js 官方提供了 `@nuxtjs/axios` 模組，此模組中還包含了 `axios`、`@nuxtjs/proxy` 模組。其中 `@nuxtjs/proxy`
是解決 Nuxt 中跨域問題，進行代理轉發請求。
所以我們只要安裝 `@nuxtjs/axios` 即可：

```shell
npm install @nuxtjs/axios
```

在 nuxt.config.js 引入 @nuxtjs/axios 模組

```js
modules: [
 '@nuxtjs/axios',
],
```