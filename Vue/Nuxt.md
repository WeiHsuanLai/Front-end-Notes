官方網站:https://nuxt.com/
中文網站:https://nuxt.com.cn/

Nuxt.js 是一個基於 Vue.js 的開源框架，旨在簡化構建服務端渲染 (Server-Side Rendering, SSR) 和靜態網站生成的過程。它通過封裝和擴展 Vue.js 的能力，讓開發者能夠輕鬆地構建功能強大且可擴展的應用程序，並同時兼顧前端和後端需求。Nuxt.js 的主要目標是提升開發效率，簡化 SSR 實現，並提供一個結構化的應用框架。以下將對 Nuxt.js 進行詳細介紹，探討其特性、優勢和應用場景。

## Nuxt.js 的核心概念

Nuxt.js 是在 Vue.js 的基礎上擴展而來的，但與單純的 Vue.js 項目不同，它專注於幫助開發者構建全棧應用。它的三大核心概念是：

- 服務端渲染 (SSR)：Nuxt.js 內建對服務端渲染的支持，使得頁面可以在伺服器端進行預渲染，然後將完整的 HTML 內容發送到客戶端，這大幅提高了首屏渲染速度，並且對 SEO 非常友好。這對於那些需要在搜索引擎中優化展示的網站尤為重要。
    
- 靜態網站生成 (SSG)：Nuxt.js 允許將應用程序編譯成靜態網站，即在編譯過程中將每個頁面預生成為靜態文件。這樣的靜態網站不僅運行速度極快，還能提供比傳統的 SSR 更好的擴展性，因為靜態網站可以直接部署在 CDN 上。
    
- 單頁應用程序 (SPA)：Nuxt.js 支持構建傳統的單頁應用，這意味著如果不需要 SSR 或 SSG，你仍然可以使用 Nuxt.js 的其他優點來構建純粹的前端應用。

渲染模式種類參考:[[網頁渲染模式（Rendering Patterns]]

### 基於檔案的路由

Nuxt 採用**基於檔案的路由（file-based routing）機制**，只要將 `.vue` 檔案放進 `pages/` 資料夾中，框架就會自動建立對應的網址路由，完全不需要額外手動設定 Vue Router。  
這樣的設計結合了「**高度自動化**」與「**約定優於配置（Convention over Configuration）**」的理念，不僅讓路由結構清晰明瞭、好閱讀，也大幅減少重複性的設定流程。

|`interactionHistory` 底下的頁面|對應路由|
|---|---|
|**index.vue**|`/interactionHistory`|
|**add.vue**|`/interactionHistory/add`|
|**[id].vue**|`/interactionHistory/:id`|

※ 補充：`:id` 位置可以放任何值，當作變數使用。

結構範例如下：

```
pages/interactionHistory/
|__ index.vue  // 列表
|__ add.vue    // 新增
|__ [id].vue     // 修改或查看
```

如果要製作動態嵌套路由，可以這樣建立：

```
pages/interactionHistory/
|__ [id]/
    |__ edit.vue   // 對應路由 /interactionHistory/:id/edit
```

### 自動引入機制

Nuxt 支援 `components`、`composables` 等自動引入（Auto Import）功能，開發者不需要手動寫 `import`，只要將檔案放在指定資料夾中就能直接使用。  
❗但要注意，根據專案規模不同，自動引入的適用性也不同，例如 自動引入對於小型專案可能很方便，但對大型專案可能較為複雜，以下簡單整理說明優勢與可能的風險：

| 專案規模 | 如果自動引入了的優勢與風險                                                                                                                                                       |
| ---- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 小型   | 開發快速，將元件放進 `components/` 就能使用，幾乎不需思考引入邏輯，超方便。                                                                                                                       |
| 大型   | 容易發生例如：<br>1. 不清楚哪些元件實際被使用到（不利於 tree-shaking）<br>2. 命名衝突難追蹤<br>3. 難以從某元件快速回推來源<br>4. IDE 會根據當前檔案的作用域推測變數來源，但因為自動引入的元件沒有明確的 `import` 定義，補全功能有時不夠準確，整體體驗也會因此下降。等這些問題。 |

若想更有彈性地控制，也可以設定「**局部自動引入**」，只讓特定資料夾內的元件自動引入，其他則需手動引入。

透過在 `nuxt.config.ts` 裡設定，範例如下：  
這樣設定後，**只有 `components/ui/` 與 `components/brand/` 底下的元件會自動引入**，其他資料夾（例如 `components/test/`、`components/temp/` 等）將不會被掃描，也就是**不會被自動引入**。

```
export default defineNuxtConfig({
  components: [
    {
      path: '~/components/ui',
      pathPrefix: false
    },
    {
      path: '~/components/brand',
      prefix: 'Brand'
      // 自動為元件名稱加上 Brand 前綴，例如 <BrandBanner/>、  <BrandFooter/>
    },
  ]
})
```

這種設定方式的優點是：

- 減少不必要的掃描與降低打包體積
- 降低元件命名衝突的風險
- 提升專案架構可控性與維護性

### SEO 行銷工具支援

Nuxt 內建支援 SEO 設定，可透過 `useHead()` 或 `definePageMeta()` 設定標題、meta、Open Graph 等資訊，也支援整合 Google Analytics（`gtag`）與自動產生 sitemap，幫助網站在搜尋引擎中有更好的曝光與排名表現，這部分後面會有實作介紹，可以期待一下。

### API routes

可以直接在 `server/api/` 資料夾中放入 `.js` 或 `.ts` 檔案，Nuxt 會自動建立對應的後端 API 路由，每個檔案會對應一個API端口位址，支援 GET、POST、PUT、DELETE 等 HTTP 方法。

```
server/
|__ api/
    |__ search.js  // 對應 API 路由：/api/search
```

不需要額外安裝 Express 或設定後端框架，用 JavaScript 就能快速寫出後端邏輯&API。

> Nuxt 內建後端能力，讓我們在寫前端的同時，也能輕鬆寫 API。


### Nuxt DevTools

Nuxt DevTools 內建於 Nuxt 中且預設是開啟的，開發時能即時檢查元件結構、狀態、路由、meta 資訊與 SEO 配置 等。  
若開發完成後想關閉 DevTools，只需要在 `nuxt.config.ts` 中設定即可，範例如下：

```
export default defineNuxtConfig({
  devtools: { enabled: false },
})
```


### Nuxt Image

這是 Nuxt 提供的專用圖片處理工具，可針對圖片效能進行最佳化。當網站中含有大量圖片時，若一次性全部載入，會導致載入緩慢、體驗下降，甚至出現「圖片破圖」的狀況。而使用 Nuxt Image 時，**Lazy Loading（延遲載入）預設就會啟用**，尚未捲動到的圖片不會立即載入，大幅減少初始頁面重量、加快速度。  
此外，`<NuxtImg>` 元件比原生的 `<img>` 更強大，可直接設定裁切方式、背景填色、圖片品質等屬性，某些功能在原生 `<img>` 中通常需要搭配額外的 JavaScript 或第三方工具處理，但在 Nuxt Image 僅透過簡單設定即可完成。Nuxt Image 特別適合給電商網站、行銷著陸頁這類圖片密集的網站。

其他還有很多很棒的工具，可以至[NuxtLabs官網](https://nuxtlabs.com/)深入了解。

#### 總結

從自動整合 Vite 與 Nitro、自動路由與自動引入，到 SEO 優化與 API routes 支援，**Nuxt 幾乎幫你預設好所有開發架構中該考慮的事**。不需要反覆研究框架組裝、不必手動配置繁瑣的 SSR 環境，只要專注在寫畫面、處理資料、實現功能，讓我們可以**專注在開發功能本身**。