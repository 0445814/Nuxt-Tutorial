## 默認 layout

執行 `npm run dev` 後，會生成一個默認的 html 模板 `.nuxt/views/app.template.html`

```html
<!DOCTYPE html>
<html {{ HTML_ATTRS }}>
  <head {{ HEAD_ATTRS }}>
    {{ HEAD }}
  </head>
  <body {{ BODY_ATTRS }}>
    {{ APP }}
  </body>
</html>
```

> {{ APP }} 渲染的是主體內容，就是 pages 下的頁面元件

頁面中的表達式引用的是 nuxt.config.js 文件中 `head` 屬性值：

```js
head: {
  htmlAttrs: { // 對應 {{ HTML_ATTRS }} ，html 標籤屬性
  lang: 'en',
  },
  headAttrs: { // 對應 {{ HEAD_ATTRS }}, head 標籤屬性
    // 一般沒有什麼可設置的，隨便設置個
    name: 'headname'
  },
  bodyAttrs: { // 對應 {{ BODY_ATTRS }} body 標籤屬性
    style: 'background-color: green'
  },
  // 對應 {{ HEAD }} 即 head 標籤體內容----start
  title: "Nuxt-test", // process.env.npm_package_name || '', // package.js 的 name
  meta: [
    { charset: 'utf-8' },
    { name: 'viewport', content: 'width=device-width, initial-scale=1' },
    { hid: 'description', name: 'description', content:
    process.env.npm_package_description || '' }
  ],
  link: [
    { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
    { rel: 'stylesheet', href: '//static.aliyin.com/service/css/element.css?v=123' }
  ],
// 對應 {{ HEAD }}即 head 標籤體內容----end
},
```

## 自訂模板

我們也可以訂製客製化模板頁面，只需要在應用根目錄下創建一個 app.html 的文件，就可以覆蓋 Nuxt.js 默認模板。

但是一般不會這樣寫。因為主要使用 .vue 元件

```html
<!DOCTYPE html>
<html {{ HTML_ATTRS }}>
  <head {{ HEAD_ATTRS }}>
    {{ HEAD }}
  </head>
  <body {{ BODY_ATTRS }}>
    <h2>這是導航欄</h2>
    <!-- 主體內容 -->
    {{ APP }}
  </body>
</html>
```

## layout 布局

Nuxt.js 可通過添加 layouts/default.vue 文件來擴展應用的默認布局。

Nuxt 默認布局，以 `<Nuxt />` 顯示主頁內容

```vue
<template>
  <div>
    <Nuxt />
  </div>
</template>
```

在 layouts 目錄中的創建 .vue 文件來自訂布局， 然後通過頁面組件（pages 目錄下的文件）中的 layout
屬性來引用自訂布局。

```vue
<template>
  <div>
    <!-- 頭部導航 -->
    <div>
      <h1>部落格導航欄</h1>
    </div>
    <!-- 主體內容 -->
    <Nuxt />
  </div>
</template>
```

在 pages 目錄下面的頁面組件通過 layout 屬性引用自訂組件布局，如 pages/article.vue

> layout 預設是 default.vue

```vue
<template>
  <div>
    <h3>這是主體內容-文章列表</h3>
  </div>
</template>

<script>
export default {
  // 引用的是 /layouts/blog.vue 布局
  // layout: 'blog',
  // 或者
  layout(context) {
    console.log("context", context);
    return "blog";
  },
};
</script>
```

> 注意不寫 layout ，則頁面組件默認採用的是 default.vue 布局

> 上面參數 context 上下文對象可獲取的屬性參考：https://zh.nuxtjs.org/api/context/

> 刪除根目錄下創建的 app.html 頁面布局文件，組件布局才會生效。

!>如果是 layouts/blog 目錄下放了 index.vue ，直接通過 layout: 'blog' 是找不到這個頁面的，需
要指定檔案名 index 才找得到，即 layout: 'blog/index' 。如下圖：

```vue
<template>
  <div>
    <h3>這是主體內容-文章列表</h3>
  </div>
</template>
<script>
export default {
  // 引用的是 /layouts/blog.vue 布局
  // layout: 'blog',
  // 或者
  layout(context) {
    console.log("context", context);
    // 要寫完整路徑，加上 index, 才可引用 blog/index.vue 布局
    return "blog/index";
  },
};
</script>
```

## 錯誤頁面

通過編輯 layouts/error.vue 文件來訂製化錯誤頁面（404，500 等）。

!> 雖然此文件放在 layouts 文件夾中, 但應該將它看作是一個頁面(page) ，而不是布局使用。

```vue
<template>
  <div>
    <h1 v-if="error.statusCode === 404">
      您訪問的頁面不存在：{{ error.message }}
    </h1>
    <h1 v-else>請求介面失敗或超時！請刷新重試</h1>
  </div>
</template>
<script>
export default {
  props: ["error"], // 直接引用Error錯誤對象
  // 可只為 blog 布局訂製的錯誤頁面, 默認 default 所有布局的錯誤頁
  // layout: 'blog'
};
</script>
```

## 設置當前頁面 head 標籤

使用 head 方法設置當前頁面的頭部標籤。
在 head 方法裡可通過 `this` 關鍵字來獲取組件的數據，你可以利用頁面組件的數據來設置個性化的 `meta` 標
簽。

```vue
<template>
  <div>
    <h3>這是主體內容-首頁</h3>
  </div>
</template>
<script>
export default {
  data() {
    return {
      title: "部落格文章列表頁",
    };
  },
  // 使用 head 方法設置當前頁面的頭部標籤。
  // 在 head 方法裡可通過 this 關鍵字來獲取組件的數據，你可以利用頁面組件的數據來設置個性化的 meta 標籤
  head() {
    return {
      bodyAttrs: {
        // 對應 {{ HOBDY_ATTRS }} body標籤屬性
        style: "background-color: #fff",
      },
      title: this.title,
      meta: [
        // hid 指定標識，防止不能覆蓋父組件的 <meta name="description" > 標籤，
        // hid: 'description' 就會覆蓋父組件中的屬性為 name="description" 的meta標籤
        { hid: "description", name: "description", content: "nuxt.js技術棧" },
      ],
    };
  },
};
</script>
```

!> 注意：為了避免子組件中的 `meta` 標籤，不能正確覆蓋父組件中相同的 name 名稱的 `meta` 標籤，而產生重複 `meta` 的
現象，可使用 hid 鍵指定要覆蓋的父組件 name 值與 hid 相同的 `meta` 標籤。請閱讀關於 [vue-meta](https://vue-meta.nuxtjs.org/) 的更多訊息。
如上面 hid: 'description' 就會覆蓋父組件中的屬性為 name="description" 的 `meta` 標籤，不會出現多個的 `<meta name="description">`
