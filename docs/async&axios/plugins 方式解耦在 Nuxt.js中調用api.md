# plugins 方式解耦在 Nuxt.js中調用api

創建 api/test.js

```js
export default ({ $axios }, inject) => {
  inject("test", (data) => $axios.$get(`/test`));
};
```

引用插件

```js
plugins: ["~/api/test.js"];
```

組件中調用

```vue
<script>
export default {
  // 在 asyncData 中以 app 使用插件調用 API
  async asyncData({ app }) {
    const response = await app.$test();
    console.log(response);
  },
  methods: {
    // // 其他則以 this 調用
    testApiPlugin() {
      const response = this.$test();
    },
  },
};
</script>
```
