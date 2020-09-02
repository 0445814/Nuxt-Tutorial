## 配置

```js
// nuxt.config.js
export default {
  // universal 為 SSR，也能改成 SPA
  mode: "universal",
  /*
   ** Headers of the page
   */
  // 網頁的 head 標籤內容
  head: {
    // title 預設取自 package.json，也能自行設定
    title: process.env.npm_package_name || "",
    // meta 標籤
    meta: [
      { charset: "utf-8" },
      { name: "viewport", content: "width=device-width, initial-scale=1" },
      {
        hid: "description",
        name: "description",
        content: process.env.npm_package_description || "",
      },
    ],
    /**
     * 注意: 從 static 引入的檔案，路徑要使用 /
     */
    link: [{ rel: "icon", type: "image/x-icon", href: "/favicon.ico" }],
    // 引入 jQuery
    script: [
      { src: "/jquery-3.4.1.min.js" },
      // 也能使用 CDN
      {
        src:
          "https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.15/lodash.core.js",
      },
    ],
  },
  /*
   ** Customize the progress-bar color
   */
  // Nuxt 預設的 loading 顏色
  loading: { color: "#fff" },
  /*
   ** Global CSS
   */
  // 全域 CSS
  css: ["~/assets/scss/_custom.scss"],
  /*
   ** Plugins to load before mounting the App
   */
  plugins: [],
  /*
   ** Nuxt.js dev-modules
   */
  buildModules: [
    // Doc: https://github.com/nuxt-community/eslint-module
    "@nuxtjs/eslint-module",
  ],
  /*
   ** Nuxt.js modules
   */
  modules: [
    // Doc: https://axios.nuxtjs.org/usage
    "@nuxtjs/axios",
  ],
  /*
   ** Axios module configuration
   ** See https://axios.nuxtjs.org/options
   */
  axios: {},
  /*
   ** Build configuration
   */
  build: {
    /*
     ** You can extend webpack config here
     */
    extend(config, ctx) {},
  },
};
```
