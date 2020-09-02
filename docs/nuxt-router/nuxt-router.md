## 基礎路由

Nuxt 會監聽 pages 的目錄結構變動來自動配置路由，例如在 pages 新增 user 目錄並新增 index.vue 與 login.vue

```
目錄結構
pages/
--| user/
-----| index.vue
-----| login.vue
--| index.vue
```

Nuxt router 將會自動生成路由，可以在 .nuxt/router.js 中找到

```js
routes: [{
  path: "/user",
  component: _5adf272d,
  name: "user"
}, {
  path: "/user/login",
  component: _2486ca44,
  name: "user-login"
}, {
  path: "/",
  component: _2a3e9450,
  name: "index"
}],
```

> 目錄名稱下的 index.vue 將默認是該路由的頁面。

## 動態路由

在 Nuxt 中只需要使用下底線 `_` 作為 Vue 檔或目錄的路由名稱前綴，就能建立動態路由

```
pages/
--| _slug/
-----| comments.vue
-----| index.vue
--| users/
-----| _id.vue
--| index.vue
```

```js
router: {
  routes: [
    {
      name: "index",
      path: "/",
      component: "pages/index.vue",
    },
    {
      name: "users-id",
      path: "/users/:id?",
      component: "pages/users/_id.vue",
    },
    {
      name: "slug",
      path: "/:slug",
      component: "pages/_slug/index.vue",
    },
    {
      name: "slug-comments",
      path: "/:slug/comments",
      component: "pages/_slug/comments.vue",
    },
  ];
}
```

> :id? 後面的問號表示路由是可選的，想將它設置為必選的路由，需要在 products 目錄內建立一個 index.vue 即可。

一般來說，我們可以透過 this.\$route 取得動態路由參數對後端發請求。接著，取得回應資料後再渲染到畫面上。

```vue
<script>
export default {
  async({ params }) {
    // 注意沒有 this
    console.log(params.id);
  },
  created() {
    // 你可以查看 router 提供的資訊
    console.log(this.$route);

    // 取得動態路由參數，id 會對應到 _id.vue
    console.log(this.$route.params.id);
  },
};
</script>
```

## 路由參數驗證

Nuxt 可以讓你在動態路由組件中定義參數驗證方法。

```vue
<script>
// pages/users/_id.vue
export default {
  validate({ params }) {
    // 必須是 number 類型
    return /^\d+$/.test(params.id);
  },
};
</script>
```

> 更多驗證方法: https://zh.nuxtjs.org/api/pages-validate/

## 嵌套路由

建立內嵌子路由，只需新增一個 Vue 文件，同時添加一個與該文件同名的目錄用來存放子視圖組件。

> 注意，父組件中記得使用 <nuxt-child> 來顯示子組件

```
pages/
--| index/ # 創建一個 index 同名的目錄名
-----| _id.vue # 子路由組件
--| index.vue
```

```js
router: {
  routes: [
    // http://localhost:8000/
    {
      path: "/",
      component: "pages/index.vue",
      name: "index",
      children: [
        {
          // ? 表示可傳可不傳，如果一定要，則 index 目錄下創建一個 index.vue
          // http://localhost:8000/1
          path: ":id?",
          component: "pages/index/_id.vue",
          name: "index-id",
        },
      ],
    },
  ];
}
```
