---
{"dg-publish":true,"permalink":"/web-performance/","tags":["web-vitals","frontend"]}
---



請解釋一下你做過哪些前端的效能優化？
# 目標：在時間內做最少的事情
## Web Vitals 指標

> [Core Web Vitals](https://web.dev/vitals/?hl=zh-tw#core-web-vitals) 是一組衡量實際使用者體驗的指標，包括網頁的載入效能、互動性和視覺穩定性；我們強烈建議網站擁有者在 Google 搜尋中獲得良好的 Core Web Vitals，並確保使用者通常皆能享有良好的使用體驗。這與網頁體驗的其他層面一樣，都是我們的核心排名系統決定排名的考量要素。詳情請參閱「[瞭解 Google 搜尋結果中的網頁體驗](https://developers.google.com/search/docs/appearance/page-experience?hl=zh-tw)」。 ———— [Google 搜尋中心](https://developers.google.com/search/docs/appearance/core-web-vitals?hl=zh-tw)

# 指標簡介
## LCP（Largest Contentful Paint）

> 它專注於 **網頁上最大的文本塊、圖片或視訊元素** 首次渲染完成的時間。簡單來說，LCP 告訴我們，使用者在什麼時候可以看到網頁上最大的那個元素。

- **大小：** 元素的大小（寬度乘以高度）最大。 => Size < 100%、（顯示大小/總大小） > 0.05
- **視覺穩定性：** 元素的位置在渲染過程中相對穩定。 
- **在視窗中：** 元素至少有一部分在視窗中可見。 => Opacity > 0
## CLS（Cumulative Layout Shift）

> 當網頁在載入過程中，頁面上的元素位置發生了「非預期」的移動，導致使用者體驗不佳的情況。這種移動可能會造成使用者誤觸、影響操作，降低網站的可用性。

- 圖片：未指定圖片尺寸、Lazy Loading
- 字體：延遲載入
- 第三方套件：如廣告造成的位移
## INP（Interaction to Next Paint）

> 直到下一個畫面 Render 的時間。使用者在網頁中進行的所有互動（如：點擊、鍵盤輸入等）都會被計算入 INP。INP 越低代表使用者感受到的延遲越低，且使用體驗較為良好。

> 參考資料：[與下一個顯示的內容互動 (INP)](https://web.dev/articles/inp?hl=zh-tw)


## FCP（First Contentful Paint）

> 它代表著 **瀏覽器第一次在螢幕上渲染出任何內容的時間點**。這個「內容」可以是文字、圖片、SVG 或非白色的畫布元素。**簡單來說，FCP 就是使用者第一次看到網頁上有東西出現的那一刻。**

### ✅ How to improve

1️⃣ 避免 `CSS` and `Font` 的 Render Blocking

```css
@import '/output.css'

@import url('https://fonts.googleapis.com/css2?family=Chokokutai&family=Outfit:wght@100..900&family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap');
```

> 來源：[Google Font](https://fonts.google.com)

使用 Module Bundlers
- `Webpack`
- `Rollup`
- `Vite`

2️⃣ Pre-connect to src（OS：為什麼對於 LCP 來說會比較快？）

```
<link rel="preconnect" href="https://fonts/googleapis.com">

<link rel="preload" as="font" crossorigin
	href="...
/>
```

- style
- script
- image
- font
- fetch

3️⃣ 決定 javascript 執行的時間

> 延伸閱讀：[`<script> 的 async 與 defer 有什麼不同？`](https://www.explainthis.io/zh-hant/swe/fe-script-async-defer-difference)

- `defer` vs `async` attributes are explained:
    - Plain `script`: Blocks parsing immediately
    - `async`: Downloads when convenient but executes immediately when downloaded (creates race conditions)
    - `defer`: Downloads when convenient but waits to execute until just before DOMContentLoaded (recommended for most cases)
- Type="module" scripts are automatically deferred

# Legacy 指標（2024 後不再採用）
## FID（First Input Delay ）

> 它測量的是使用者第一次與網頁互動（例如點擊按鈕、輸入文字）到瀏覽器能夠響應的時間。

> 參考資料：[與下一個顯示的內容互動 (INP)](https://web.dev/articles/inp?hl=zh-tw)
### 為什麼捨棄FID？

- FID只有評估第一次的互動，沒有辦法完整表示使用者的體驗。
- INP 可以捕捉到一些 FID 所無法涵蓋的性能問題，例如 JavaScript 執行時間過長、樣式計算複雜等。

# 不計入SEO、但是會影響使用體驗

## TTFB（Time to First Byte）

> 代表的是從瀏覽器向伺服器發出請求到接收到第一個位元組的回應所花費的時間。簡單來說，就是使用者點擊連結後，要等多久才會收到伺服器傳來的第一筆資料。

> 參考資料：[最佳化首次位元組的時間](https://web.dev/articles/optimize-ttfb?hl=zh-tw)

- Not a SEO Metric
### ✅ How to improve

1️⃣ Using gzip

**Gzip** 是一種常見的文件壓縮格式，特別適用於文本類型的數據。在網頁開發中，Gzip 可以有效地壓縮 HTML、CSS 和 JavaScript 等靜態資源，使其在傳輸過程中佔用更少的頻寬。

### Gzip 如何影響 TTFB？

**TTFB** (Time to First Byte) 指的是從瀏覽器發出請求到接收到第一個字節的響應時間。Gzip 對 TTFB 的影響主要體現在以下幾個方面：

- **減少傳輸數據量：** Gzip 可以將文件壓縮成更小的尺寸，這意味著瀏覽器需要下載的數據更少。
- **加快下載速度：** 更小的文件尺寸意味著更快的下載速度，這直接縮短了 TTFB。
- **降低伺服器負擔：** 減少傳輸數據量可以降低伺服器的負擔，提高伺服器的響應速度。

2️⃣ 通訊協定：HTTP/1、HTTP/2、HTTP/3。

- HTTP/1 所需的時間顯著 > HTTP/2、HTTP/3

3️⃣ Distance of CDN：如果主機離使用者的距離越近，則獲得 response 的速度就越短，可以參考 AWS CloudFront 的[服務說明](https://aws.amazon.com/tw/cloudfront/)。

# Reference

1. [Web Performance Fundamentals, v2]()