# $axios 攔截器

創建 plugins/interceptor.js 請求攔截器

```js
export default ({ store, route, redirect, $axios }) => {
  $axios.onRequest((config) => {
    console.log("請求攔截器");
    return config;
  });
  $axios.onResponse((response) => {
    console.log("響應攔截器", response);
    return response;
  });
  $axios.onError((error) => {
    console.log("error.response.status", error.response.status);
  });
};
```

引用插件

```js
// nuxt.config.js
plugins: ["~/plugins/interceptor.js"];
```
