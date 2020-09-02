## 生命週期

![ ](https://i.imgur.com/1Q1Eanm.png ":size=500x")

1. 當一個用戶端請求進入的時候，伺服器端有通過 nuxtServerInit 這個命令執行在 Store 的 action ，在這裡
   接收到用戶端請求的時候，可以將一些用戶端訊息存儲到 Store 中，也就是說可以把在伺服器端存儲的一些客
   戶端登錄訊息存儲到 Store 中。

2. 接著使用了 中介軟體 機制，中介軟體其實就是一個函數，會在每個路由執行之前去執行，在這裡可以做很多事
   情，或者說可以理解為是路由器的攔截器的作用。

3. 再接著使用 validate 對用戶端攜帶的參數進行校驗。

4. 然後在 asyncData 與 fetch 進入正式的渲染週期， asyncData 向伺服器端獲取數據，把請求到的數據合併到
   Vue 中的 data 中。
   最後 Render 將渲染組件。

## 了解 process

1. process 是 node.js 一個全域物件，提供 node 執行時的相關資訊。
2. nuxt 前端的 process 不等於後端的 process。 → 知道一下就好
3. nuxt 使用 process.client、process.server 來判斷目前程式在那個環境運作

```vue
<script>
created () {
   if(process.client){
      console.log("process.client")
    }
   if(process.server){
      console.log("process.server")
    }
},
</script>
```

```js
// 判斷目前程式啟動的環境(生產或開發)
console.log(process.env.NODE_ENV);
```
