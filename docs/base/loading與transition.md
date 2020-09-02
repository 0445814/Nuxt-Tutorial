Nuxt 預設提供了 loading 效果，相當實用，當然也可以對這個 loading 進度條樣式做調整

## loading

```js
// nuxt.config.js
export default {
  loading: {
    color: "blue",
    height: "5px",
    duration: 5000, // 預設 5000 毫秒
  },
};
```

!> Nuxt 的 loading 組件並不知道當前頁面到底需要多少時間才能加載完成，所以無法準確將進度條顯示與實際加載同步，這部分可以用 duration 或 continuous 來解決。

`duration`: 自定義加載時間，如果頁面加載時間超過設定的 duration，那麼進度條就會維持 100 %。

`continuous`: 設為 true 改變默認行為，在達到 100%後，進度條將在 duration 時間內再次收縮回 0 %。當達到 0 % 後仍未完成加載時，它將再次從 0 %開始增長到 100 %，這將重複直到加載完成。

官網示意圖:

![](https://zh.nuxtjs.org/api-continuous-loading.gif)

## transition 過渡效果

Nuxt 中設置 transition 也相當快速。

這裡僅說明全域 transition，個別組件使用 transition 可參考文檔。

首先，在 assets/scss 中新增 transition.scss，並在 nuxt.config.js 全域引入並啟用它

```scss
// transition.scss
.layout-enter-active,
.layout-leave-active {
  transition: opacity 0.5s;
}
.layout-enter,
.layout-leave-active {
  opacity: 0
```

```js
// nuxt.config.js
export default {
  layoutTransition: 'layout'
  // or
  transition: {
    name: 'layout',
    mode: 'out-in'
  },
 css: [
    '~/assets/scss/transition.scss'
  ],
}
```

!>  只有 layout 的 `<Nuxt />` 才有過渡效果