## 使用

```vue
// 首頁
<template>
  <div>
    <p>nuxt-link</p>
    <nuxt-link to="/users">使用者介面</nuxt-link>
  </div>
</template>
```

> 別名: `<n-link>`、`<NuxtLink>` 和 `<NLink>`

nuxt-link 會預加載跳往頁面的資源，例如上面連結是 /users，那麼在當前頁面中，Nuxt 就已經會預先載好的 users 的資源，例如 users 的 CSS 或 JS

如果想取消預加載，可以使用 no-prefetch

```vue
<nuxt-link to="/users" no-prefetch >使用者介面</nuxt-link>
```

若加載資源愈多，那麼當前頁面 loading 時間會愈長，可依自身需求是否啟用預加載。

!> 在 Nuxt 中，router-link 也是可以使用的，但不建議，因為 router-link 不會預加載資源

