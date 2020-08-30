# Nuxt.js 代理解決跨域

如果調用後台介面有跨域問題，需要對請求地址進行代理轉發才可解決。
我們知道在 Vue-cli 中配置代理很方便，只需要在 vue.config.js 中找到 proxyTable 添加即可，而在 Nuxt 中同樣需
要修改 nuxt.config.js 配置文件，前提要安裝了 @nuxtjs/axios 。參考 https://axios.nuxtjs.org/

1. 在 nuxt.config.js 中開啟代理配置

```js
module: [ // 陣列
  '@nuxtjs/axios', // 引用模組
],

axios: { // 物件
  proxy: true // 開啟代理
  prefix: '/api' // 請求前綴
},

proxy: { // 物件
   // 將 /api 替換為 '', 然後代理轉發到 target 指定的 url
  '/api': {
    target: apiPath,
    pathRewrite: {'^/api': ''}
   }
}, // 逗號
```

?> pathRewrite 選項(重寫地址)
/api/ 將被添加到 API 端點的所有請求中。可以使用 pathRewrite 選項刪除。
因為在 ajax 的 url 中加了前綴 /api ，而原本的介面是沒有這個前綴的。
所以需要通過 pathRewrite 來重寫地址，將前綴 /api 轉為 / 或者是 '' 。
如果本身的介面地址就有 /api 這種通用前綴，就可以把 pathRewrite 刪掉

```vue
<script>
export default {
  // 方式2 請求數據介面 async await
  async asyncData({ $axios, error }) {
    try {
      const response = await $axios.$get("/test");
      return { data: response.data.data };
    } catch (e) {
      error({ statusCode: 404, message: "未找到請求資源。" });
    }
  },
};dddd
</script>
```
