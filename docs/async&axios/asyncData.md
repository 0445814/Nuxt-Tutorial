## asyncData

Nuxt.js 擴展了 Vue.js，增加了一個叫 `asyncData` 的方法，使得我們可以在渲染組件之前非同步獲取數據。
`asyncData` 方法會在組件（限於頁面組件）每次載入之前被調用。它可以在服務端或路由更新之前被調用。 在這
個方法被調用的時候，第一個參數 `context` 被設定為當前頁面的上下文對象，你可以利用 `asyncData` 方法來獲取
數據，Nuxt.js 會將 `asyncData` 返回的數據與 data 方法返回的數據一起合併後返回給當前組件。
調用後台數據介面我們採用 axios 非同步發送請求。

## 重要特性

1. 只有 pages 的 .vue 才可使用，components 不行
2. 第一次進入網頁時，後端會執行，之後切換路由時，只有前端會執行
3. asyncData return 出來的 data 會取代 Vue 的 data (達成 SSR 目的)
4. asyncData 發送 Ajax 請求，需返回 Promise，這樣才能蓋掉 Vue 的 data (通常會搭配 async、await)

!> 由於 asyncData 方法是在組件初始化前被調用的，所以在方法內是沒有辦法通過 this 來引用組件的實例對象(this 指向 undefined)。
!> created 鉤子可以從後端印出 Vue component 實例，但 asyncData 不行

Nuxt.js 提供了幾種不同的方法來使用 asyncData 方法，你可以選擇自己熟悉的一種來用：

1. 返回一個 Promise, Nuxt.js 會等待該 Promise 被解析之後才會設置組件的數據，從而渲染組件.

```vue
<script>
export default {
  // 載入組件之前伺服器端會調用
  // 方式1：使用了兩個 return
  asyncData({ $axios }) {
    return $axios.$get(api).then((response) => {
      console.log("response", response);
      const data = response.data;
      return { data }; // {data: data}
    });
  },
};
</script>
```

2. 使用 async await

如果項目中直接使用了 node_modules 中的 axios，並且使用 axios.interceptors 添加攔截器對請求或響應數據進行了處理，確保使用 axios.create 創建實例後再使用。否則多次刷新頁面請求服務器，伺服器端渲染會重複添加攔截器，導致數據處理錯誤。

```js
import axios from "axios";
const myaxios = axios.create({
  // ...
});
myaxios.interceptors.response.use(
  function (response) {
    return response.data;
  },
  function (error) {
    // ...
  }
);
```

使用 async、await

```vue
<script>
export default {
  async asyncData({ params }) {
    const { data } = await axios.get(`https://my-api/posts/${params.id}`);
    return { title: data.title };
  },
};
</script>
```

asyncData 方法返回的數據在融合 data 方法返回的數據後，一併返回給模板進行展示。

## 建立簡單 API

使用 express 建立 API

1. 安裝 Node 的 express 框架，-g 是全域安裝

2. 建立 server 目錄，並新增一個 api.js

```js
// api.js
const express = require('express')
const app = express()

/**
 * 當使用者 get 請求到 /person 路徑時，回傳一個物件
 */
app.get('/person', (req, res) => {
  res.json({ name: 'Ken', age: 27, job: 'Front-end developer' })
})

module.exports = app
```

3. nuxt.config.js 註冊 serverMiddleware

```js
export default {
  serverMiddleware: [
    { path: '/api', handler: '~/server/api.js' },
  ],
}
```

這時在網址輸入

```
http://localhost:3000/api/person
```

就應該可以看到回傳的資料了 !

