---
{"dg-publish":true,"permalink":"/xss/","tags":["gardenEntry"]}
---


簡報：https://app.heptabase.com/w/809d04acdf3a2e63b671cec9f2ce7bcf3b263462432fc348f6ce83bbea572ea9?id=62ffe53f-4f27-4550-978d-50e89fdd6c28
## Part I ：上週主題回顧：

Q: Sanitizer API 與第三方套件 DOMPurify 相比有什麼特點？

1. A: Sanitizer API 的特點是「不管你怎麼用怎麼設定，都不會有 XSS 的產生」。這是優點也是缺點，因為它的限制很嚴格，某些合法的標籤（如` <iframe>`）即使設定允許也會被移除，彈性較低。相較之下，DOMPurify 可以自訂允許哪些標籤跟屬性，較為彈性。

Q: 什麼是 Universal XSS (UXSS)？它與一般的 XSS 有什麼不同？

1. A: Universal XSS 是針對瀏覽器或內建 plugin 的漏洞進行攻擊，而不是針對網站本身。它的特點是即使網站本身沒有任何問題，甚至是純靜態網頁，都可能受到攻擊。UXSS 可以做到「無論在哪個網站都可以執行程式碼」，影響範圍比一般 XSS 更廣。

Q: CSP bypass 中，base-uri 的設定為什麼重要？

1. A: 如果沒有設定 base-uri，攻擊者可以透過插入 <base> 標籤改變所有相對路徑的參考位置，進而載入惡意的腳本。因此建議在 CSP 中加入 base-uri 'none' 來阻擋所有的 base 標籤。

Q: Trusted Types 的主要功能是什麼？

1. A: Trusted Types 的功能不是「確定你的 HTML 沒問題」，而是「強制在有可能出問題的 DOM API 使用 Trusted Types，不能使用字串」。當開發者忘記處理使用者輸入時，瀏覽器會直接拋出錯誤，而不會把未處理過的字串當作 HTML 直接 render。

Q: 什麼是 Mutation XSS (mXSS)？請解釋其原理。

1. A: Mutation XSS 是利用瀏覽器對 HTML 的自動修復機制所產生的攻擊。當 HTML 在瀏覽器中被解析時，某些元素的結構會被瀏覽器自動改變（mutation），這種變化可能導致原本看似安全的 HTML 變得不安全。例如 `<svg>` 中的元素可能會「跳出」到外層。

## 不直接執行 Javascript 的攻擊手法（3-1~3-3）

### 3-1 原型鏈的攻擊方式

- 什麼是 Firewall：WAF防禦的五個階段

- 什麼是 原型鏈？






### 3-2 DOM Clobbering

### 3-3 模板注入攻擊 CSTI