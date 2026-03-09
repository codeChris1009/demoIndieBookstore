# <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/paw-print-light.svg" width="32" height="32" align="center" /> Padi Bookstore Event Website

> 一個專為獨立書店、在地商家、優質出版社與寵物友善商家聯合舉辦的溫馨活動網站

**GitHub Page**：[https://codechris1009.github.io/demoIndieBookstore/](https://codechris1009.github.io/demoIndieBookstore/)

> **說明**：本 README 使用 [Phosphor Icons](https://phosphoricons.com/) 圖示庫，所有圖標透過 jsDelivr CDN 載入 SVG 格式。

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/info-light.svg" width="24" height="24" align="center" /> Project Introduction 專案簡介

Padi Bookstore Event 是一個結合**閱讀分享**、**夥伴陪伴**、**美好食物**與**在地商業連結**的活動平台。我們相信好書需要被分享，寵物是最溫暖的閱讀夥伴，而獨立書店與在地商家是社區文化的重要推手。

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/target-light.svg" width="20" height="20" align="center" /> Core Values 活動核心價值

- **<img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/books-light.svg" width="18" height="18" align="center" /> 分享書籍** - 精選獨立出版社的優質作品
- **<img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/dog-light.svg" width="18" height="18" align="center" /> 夥伴陪伴** - 寵物友善空間，與毛孩一起閱讀
- **<img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/fork-knife-light.svg" width="18" height="18" align="center" /> 美好食物** - 結合在地餐飲，創造完整體驗
- **<img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/handshake-light.svg" width="18" height="18" align="center" /> 商業連結** - 串聯獨立書店、出版社、在地商家

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/sparkle-light.svg" width="24" height="24" align="center" /> Core Features & Animation Principles 核心功能與動畫原理

### 1. Sticky Navigation 智能固定導航

**技術原理：**

```javascript
// 使用 IntersectionObserver API 監控 Hero Section
const obs = new IntersectionObserver(
  function (entries) {
    const ent = entries[0];
    if (ent.isIntersecting === false) {
      document.body.classList.add("sticky");
    } else {
      document.body.classList.remove("sticky");
    }
  },
  {
    root: null,
    threshold: 0,
    rootMargin: "-80px",
  },
);
```

**實作細節：**

- 當使用者向下滾動 80px 後，Hero Section 離開視窗時觸發
- 動態添加 `.sticky` class 到 `<body>`
- CSS 設定 `.sticky .header` 為 `position: fixed`，固定在頂部
- 背景色使用 `rgba(5, 44, 69, 0.97)` 提供半透明質感
- 添加 `box-shadow` 增加層次感

**優勢：**

- 比傳統 `scroll` 事件監聽更高效能
- 避免頻繁的 DOM 操作
- 瀏覽器原生優化，更流暢

---

### 2. Mobile Navigation 響應式漢堡選單

**技術原理：**

```css
/* 容器高度動畫 */
.main-nav {
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.5s ease-in-out;
}

.nav-open .main-nav {
  max-height: 50rem;
}

/* 內容滑入動畫 */
.main-nav-list {
  transform: translateX(100%);
  opacity: 0;
  transition:
    transform 0.5s,
    opacity 0.5s;
}

.nav-open .main-nav-list {
  transform: translateX(0);
  opacity: 1;
}
```

**實作細節：**

- **雙層動畫設計**：外層容器(`main-nav`)控制高度展開，內層列表(`main-nav-list`)控制滑入效果
- **推開式佈局**：使用 `flex-wrap: wrap` + `order: 3`，導航欄不覆蓋內容，而是將下方內容推開
- **圖示切換**：列表圖示與關閉圖示使用 `display` 控制顯示/隱藏
- **自動關閉**：點擊導航連結後自動 `removeClass("nav-open")`

**z-index 層級管理：**

- Mobile button: `z-index: 99`
- Main navigation: `z-index: 1000`
- Sticky header: `z-index: 9999`

---

### 3. Scroll Margin 錨點滾動偏移補償

**技術原理：**

```css
/* 桌面版 */
section {
  scroll-margin-top: 11rem;
}

/* 移動端 (≤944px) */
@media (max-width: 59em) {
  section {
    scroll-margin-top: 12rem;
  }
}

/* 手機 (≤544px) */
@media (max-width: 27.5em) {
  section {
    scroll-margin-top: 11rem;
  }
}
```

**實作細節：**

- 解決 fixed header 遮擋錨點目標的問題
- 不同斷點設定不同偏移值，因為 header 高度會變化
- 配合 `scroll-behavior: smooth` 提供流暢滾動體驗
- 純 CSS 解決方案，比 JavaScript 計算更高效

**優勢：**

- 自動適配所有錨點連結（`<a href="#section">`）
- 不需要額外 JavaScript 計算
- 瀏覽器原生支援，效能最佳

---

### 4. Responsive Book Cards 書籍卡片響應式佈局

**技術原理：**

```css
/* 桌面：3 欄 */
.grid--3-cols {
  grid-template-columns: 1fr 1fr 1fr;
}

/* 平板 (≤944px)：2 欄 */
@media (max-width: 59em) {
  .section-book .grid--3-cols {
    grid-template-columns: 1fr 1fr;
  }
  .book-img {
    aspect-ratio: 2 / 3;
  }
}

/* 手機 (≤610px)：1 欄居中 */
@media (max-width: 38.125em) {
  .section-book .grid--3-cols {
    grid-template-columns: 1fr;
    justify-items: center;
  }
  .book-card {
    max-width: 42rem;
  }
}
```

**圖片比例保持：**

- 使用 `aspect-ratio: 2/3` 維持書籍封面比例
- `height: auto` 配合 `width: 100%` 實現響應式
- 避免圖片變形或被裁切

---

### 5. Step Section Pseudo-elements 步驟區塊偽元素裝飾

**技術原理：**

```css
.step-img-box::before,
.step-img-box::after {
  content: "";
  display: block;
  border-radius: 50%;
  position: absolute;
}

.step-img-box::before {
  width: 60%;
  padding-bottom: 60%;
  background-color: #fdf2e9;
  z-index: -2;
}

.step-img-box::after {
  width: 45%;
  padding-bottom: 45%;
  background-color: #fae5d3;
  z-index: -1;
}
```

**響應式定位：**

```css
/* 手機 (≤544px)：居中對齊 */
@media (max-width: 27.5em) {
  .step-img-box img {
    width: 50%;
    margin: auto;
  }

  .step-img-box::before {
    left: 50%;
    transform: translateX(-50%) rotate(-12deg);
  }

  .step-img-box::after {
    left: 50%;
    transform: translateX(-50%) rotate(-24deg);
  }
}
```

**實作細節：**

- 使用 `::before` 和 `::after` 創建兩層圓形裝飾
- `padding-bottom` 技巧實現響應式圓形（百分比相對於寬度）
- `transform: rotate()` 製造傾斜效果
- 移動端使用 `translateX(-50%)` 實現絕對居中

---

### 6. Testimonial Layout 見證區塊水平佈局

**技術原理：**

```css
/* 手機 (≤544px)：圖片 + 文字橫向排列 */
@media (max-width: 27.5em) {
  .testimonial {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: 1.2rem;
    align-items: start;
  }

  .testimonial-img {
    width: 6.4rem;
    height: 6.4rem;
    grid-row: 1 / -1;
  }

  .testimonial-quote {
    grid-column: 2;
  }

  .testimonial-author {
    grid-column: 2;
  }
}
```

**實作細節：**

- 使用 CSS Grid 的 `auto 1fr` 實現左圖右文佈局
- `grid-row: 1 / -1` 使圖片跨越所有行
- `align-items: start` 保持圖片頂部對齊
- 文字內容自動佔據剩餘空間

---

### 7. Image Optimization 圖片效能優化

**技術實作：**

**壓縮與尺寸調整：**

```bash
# Hero 主圖：1920px 寬，品質 80%
sips -Z 1920 --setProperty formatOptions 80 hero-image.jpg

# Step 圖片：1200px 寬，品質 75%
sips -Z 1200 --setProperty formatOptions 75 step-image.jpg

# Gallery 圖片：800px 寬，品質 70%
sips -Z 800 --setProperty formatOptions 70 gallery-image.jpg
```

**HTML 屬性優化：**

```html
<!-- Hero 圖片：優先載入 -->
<img
  src="hero.jpg"
  width="1920"
  height="1280"
  fetchpriority="high"
  alt="Description"
/>

<!-- 非首屏圖片：延遲載入 -->
<img
  src="gallery.jpg"
  width="800"
  height="600"
  loading="lazy"
  alt="Description"
/>
```

**優化成果：**

- Hero 主圖：1.8MB → 464KB (**↓ 74%**)
- HowStep 圖片：~3MB → 608KB (**↓ 80%**)
- Gallery 圖片：~10MB → 1.5MB (**↓ 85%**)

**優勢：**

- `width` + `height` 避免 CLS (Cumulative Layout Shift)
- `loading="lazy"` 節省初始載入時間
- `fetchpriority="high"` 確保關鍵圖片優先顯示

---

### 8. Premium Plan Card 進階方案卡片動畫

Premium 方案卡片結合了**旋轉動畫外框**、**夕陽漸變配色**與**飄帶裝飾標籤**，創造出視覺上的層次感與吸引力。

#### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/circle-dashed-light.svg" width="20" height="20" align="center" /> 旋轉動畫外框

**技術原理：**

```css
.pricing-plan--premium {
  position: relative;
  padding: 0.4rem;
  overflow: hidden;
  border: #e85d75 0.16rem solid;
}

.pricing-plan--premium::before {
  content: "";
  position: absolute;
  inset: 0;
  background: conic-gradient(
    from 0deg,
    #ff6b35 0deg,
    /* 橘紅 */ #ff8c42 60deg,
    /* 橙色 */ #ffa94d 90deg,
    /* 淺橙 */ #ffc857 120deg,
    /* 金黃 */ #ffdd67 150deg,
    /* 亮黃 */ #ffc857 180deg,
    /* 金黃 */ #ffa94d 210deg,
    /* 淺橙 */ #ff8c42 240deg,
    /* 橙色 */ #ff6b35 300deg,
    /* 橘紅 */ #e85d75 330deg,
    /* 粉紅 */ #ff6b35 360deg /* 回到橘紅 */
  );
  z-index: 0;
  scale: 1.09;
  opacity: 0.8;
  animation: rotateBorder 6.6s linear infinite;
}

@keyframes rotateBorder {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
```

**實作細節：**

- **圓錐漸變** (`conic-gradient`): 建立圓形漸變色環，從中心點向外呈放射狀
- **色彩平滑過渡**: 每 30-60 度設定一個顏色節點，確保過渡自然
- **scale 放大**: `scale: 1.09` 讓漸變邊框稍微大於卡片，露出外框效果
- **持續旋轉**: `animation` 設定為 6.6 秒一圈，線性無限循環
- **inset: 0**: 等同於 `top: 0; right: 0; bottom: 0; left: 0;`，覆蓋整個父元素

**優勢：**

- GPU 加速的 `transform` 動畫，效能優異
- 使用偽元素避免額外 DOM 節點
- `conic-gradient` 比多個 `linear-gradient` 更有效率

---

#### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/sun-horizon-light.svg" width="20" height="20" align="center" /> 夕陽配色特效

**技術原理：**

```css
.pricing-plan--premium {
  box-shadow:
    /* 基礎陰影 - 深度層次 */
    0 1rem 1.6rem rgba(20, 10, 8, 0.3),
    0 1.8rem 3.2rem rgba(15, 8, 6, 0.25),
    0 3.4rem 6.4rem rgba(10, 5, 4, 0.2),
    0 5rem 9.6rem rgba(8, 4, 3, 0.15),
    /* 發光效果 - 暖色調光暈 */ 0 0 3.2rem rgba(139, 35, 20, 0.3),
    0 0 5.2rem rgba(180, 50, 25, 0.2),
    0 0 7.2rem rgba(200, 80, 40, 0.15);
  transform: translateY(-1.2rem);
  transition: all 0.4s;
}

.pricing-plan--premium:hover {
  transform: translateY(-1.2rem) scale(1.06);
}
```

**顏色語義：**

| 色碼      | 角度      | 意義     | 視覺效果     |
| --------- | --------- | -------- | ------------ |
| `#ff6b35` | 0°/300°   | 落日橘紅 | 主色調基底   |
| `#ff8c42` | 60°/240°  | 晚霞橙   | 溫暖過渡     |
| `#ffa94d` | 90°/210°  | 黃昏淺橙 | 柔和中間色   |
| `#ffc857` | 120°/180° | 金色光芒 | 亮點區域     |
| `#ffdd67` | 150°      | 陽光亮黃 | 最高光點     |
| `#e85d75` | 330°      | 玫瑰粉   | 收尾柔化色彩 |

**實作細節：**

- **四層深度陰影**: 模擬卡片浮起的立體感（1rem → 5rem 遞增）
- **三層光暈**: 使用 `0 0 [模糊半徑]` 創造無偏移的發光效果
- **暖色系 RGBA**: 陰影顏色選用紅棕色系，與夕陽主題一致
- **預設上浮**: `translateY(-1.2rem)` 讓卡片初始就高於基準線
- **Hover 放大**: 滑鼠懸停時 `scale(1.06)` 強化互動回饋

---

#### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/flag-banner-light.svg" width="20" height="20" align="center" /> 飄帶裝飾標籤

**技術原理：**

```css
.pricing-plan--premium::after {
  position: absolute;
  z-index: 2;
  content: "bookaholic";
  color: #fff;
  font-size: 1.6rem;
  font-weight: 700;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 0.4rem;

  padding: 0.8rem 3.2rem;
  top: 7.9%;
  right: -9.6%;

  background-image: linear-gradient(
    45deg,
    #ff6547 0%,
    #ffb144 51%,
    #ff7053 100%
  );
  filter: drop-shadow(0 0 0.2rem #ff6547) drop-shadow(0 0 0.3rem #ff8c42)
    drop-shadow(0 0 0.4rem #ffa94d) drop-shadow(0 0 0.5rem #ffc857)
    drop-shadow(0 0 0.6rem #ffdd67);
  transform: rotate(40deg);
}
```

**HTML 結構配合：**

```html
<div class="pricing-plan pricing-plan--premium">
  <!-- ::before - 旋轉外框 -->
  <div class="pricing-plan-inner">
    <!-- 實際內容 -->
  </div>
  <!-- ::after - 飄帶標籤 "bookaholic" -->
</div>
```

**實作細節：**

- **絕對定位**: `top: 7.9%` + `right: -9.6%` 讓標籤斜跨卡片右上角
- **旋轉角度**: `rotate(40deg)` 創造飄帶斜掛的視覺效果
- **漸變背景**: 45 度角線性漸變，從橘紅到金黃再到橘紅，呼應夕陽主題
- **多層光暈**: 5 層 `drop-shadow` 模擬霓虹燈發光效果（0.2rem → 0.6rem 遞增）
- **z-index 層級**: 設為 2，確保在內容層（0-1）之上顯示
- **字距放寬**: `letter-spacing: 0.4rem` 增強標籤的視覺張力

**層級結構：**

```
z-index 層級說明：
├─ .pricing-plan-inner (z-index: 自動)  - 內容層
├─ ::before (z-index: 0)                 - 旋轉外框
└─ ::after (z-index: 2)                  - 飄帶標籤（最上層）
```

**優勢：**

- 純 CSS 實現，無需圖片素材
- `drop-shadow` 比 `box-shadow` 更適合非矩形元素
- 飄帶超出父容器邊界，需確保父元素未設定 `overflow: hidden`

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/palette-light.svg" width="24" height="24" align="center" /> Responsive Design Breakpoints 響應式設計斷點

| 斷點         | 尺寸   | 目標設備 | 主要調整                 |
| ------------ | ------ | -------- | ------------------------ |
| **84em**     | 1344px | 小型桌面 | 縮小字體、調整間距       |
| **75em**     | 1200px | 橫向平板 | 字體 56.25%、欄距縮小    |
| **59em**     | 944px  | 直向平板 | 啟用漢堡選單、書籍 2 欄  |
| **53.125em** | 850px  | 價格區   | 價格卡片單欄居中         |
| **46.5em**   | 744px  | Footer   | Footer 單欄佈局          |
| **38.125em** | 610px  | 大手機   | 書籍 1 欄居中            |
| **27.5em**   | 544px  | 小手機   | 步驟圖縮小 50%、見證橫向 |

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/wrench-light.svg" width="24" height="24" align="center" /> Tech Stack 技術棧

### Frontend Core 前端核心

- **HTML5** - 語意化標籤
- **CSS3** - Grid、Flexbox、Animation
- **Vanilla JavaScript** - 無框架，原生 API

### CSS Architecture CSS 架構

- **模組化設計** - 依功能拆分 CSS 檔案
- **Mobile-First** - 優先考慮移動端體驗
- **Block-Element-Modifier (BEM) 命名** - 部分採用 BEM 規範

### Performance Optimization 效能優化

- **IntersectionObserver** - 高效能滾動監聽
- **CSS Animations** - GPU 加速動畫
- **Image Lazy Loading** - 延遲載入非首屏圖片
- **Responsive Images** - 不同尺寸使用不同圖片規格

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/folder-light.svg" width="24" height="24" align="center" /> Project Structure 專案結構

```
demoIndieBookstore/
├── index.html                 # 主要 HTML 檔案
├── css/
│   ├── general.css           # 全域樣式、工具類別
│   ├── header.css            # Header 樣式
│   ├── navigation.css        # 導航欄樣式（含 sticky）
│   ├── hero.css              # Hero 區塊樣式
│   ├── section-feature.css   # 特色區塊樣式
│   ├── section-how.css       # 步驟說明區塊
│   ├── section-book.css      # 書籍推薦區塊
│   ├── section-testimonial.css # 見證區塊
│   ├── section-price.css     # 價格方案區塊
│   ├── section-call-action.css # 行動呼籲區塊
│   ├── footer.css            # Footer 樣式
│   ├── mobile.css            # 移動端專用樣式
│   └── rwd.css               # 響應式斷點設定
├── js/
│   └── script.js             # 互動功能（導航、滾動）
└── img/
    ├── hero/                 # Hero 圖片與 Logo
    ├── books/                # 書籍封面
    ├── howStep/              # 步驟說明圖片
    ├── testimonial/          # 見證圖片
    ├── gallery/              # 畫廊圖片
    ├── publishLogo/          # 出版社 Logo
    └── callAction/           # CTA 區塊圖片
```

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/rocket-light.svg" width="24" height="24" align="center" /> Quick Start 快速開始

### Local Development 本地運行

1. **克隆專案**

```bash
git clone <repository-url>
cd demoIndieBookstore
```

2. **啟動本地伺服器**

使用 VS Code Live Server 或：

```bash
# Python 3
python -m http.server 8000

# Node.js
npx serve
```

3. **瀏覽網站**

```
http://localhost:8000
```

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/target-light.svg" width="24" height="24" align="center" /> Main Feature Sections 主要功能區塊

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/house-light.svg" width="20" height="20" align="center" /> Hero Section

- 吸睛標題與活動宣傳
- 讀者見證小頭像（1,000,000+ 參與者）
- 雙 CTA 按鈕（立即閱讀、更多資訊）
- 大型主視覺圖片

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/books-light.svg" width="20" height="20" align="center" /> Books Section

- 精選書籍卡片展示
- 詳細書籍資訊（作者、得獎紀錄、ISBN、頁數、尺寸）
- 標籤分類（文學小說、經典小說）
- 推薦書單快速連結

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/footprints-light.svg" width="20" height="20" align="center" /> How It Works Section

- 三步驟流程說明
- 圖文並茂的步驟說明
- 裝飾性偽元素增加視覺吸引力

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/chat-light.svg" width="20" height="20" align="center" /> Testimonial Section

- 真實讀者與毛孩分享
- 12 張活動照片畫廊
- 寵物友善氛圍展示

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/currency-dollar-light.svg" width="20" height="20" align="center" /> Pricing Section

- 兩種定價方案（入門版、完整版）
- 清晰的功能對比
- CTA 按鈕引導註冊

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/envelope-light.svg" width="20" height="20" align="center" /> Call to Action Section

- 活動註冊表單
- 收集姓名、Email、偏好資訊
- 背景圖片增加視覺吸引力

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/bug-light.svg" width="24" height="24" align="center" /> Known Issues & Solutions 已知問題與解決方案

### <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/check-light.svg" width="20" height="20" align="center" /> Resolved 已解決

1. **Sticky Header 遮擋錨點**
   - 使用 `scroll-margin-top` 解決
   - 不同斷點設定不同偏移值

2. **圖片比例跑掉**
   - 使用 `aspect-ratio: 2/3` 維持書籍封面比例

3. **移動端導航按鈕不顯示**
   - 調整 CSS 載入順序，確保 `rwd.css` 在 `mobile.css` 之後
   - 設定適當的 `z-index`

4. **JavaScript 錯誤 (null reference)**
   - 所有 DOM 查詢加上 null check
   - Script 標籤移至 `</body>` 之前

5. **Lighthouse 效能問題**
   - 壓縮所有圖片 (減少 70-85% 體積)
   - 添加 `width`、`height`、`loading="lazy"` 屬性

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/chart-bar-light.svg" width="24" height="24" align="center" /> Lighthouse Score Lighthouse 分數

優化後預期分數：

- **Performance**: 90+ <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/lightning-light.svg" width="18" height="18" align="center" />
- **Accessibility**: 95+ <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/user-focus-light.svg" width="18" height="18" align="center" />
- **Best Practices**: 95+ <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/check-circle-light.svg" width="18" height="18" align="center" />
- **SEO**: 100 <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/magnifying-glass-light.svg" width="18" height="18" align="center" />

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/handshake-light.svg" width="24" height="24" align="center" /> Partners 合作夥伴

> **免責聲明**：本專案為展示用途 (Demo)，所有出版社名稱已改為代名，並無實際商業合作關係。

### Publishers 出版社 (Demo 用途代名)

- Publisher A (獨立文學出版)
- Publisher B (詩歌與散文專門)
- Publisher C (經典書系重印)
- Publisher D (多元主題出版)
- Publisher E (綜合型出版社)

### Featured Books 特色書籍 (展示用途)

- **The Great Gatsby** by F. Scott Fitzgerald
- **A Happy Death** by Albert Camus

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/notebook-light.svg" width="24" height="24" align="center" /> Development Notes 開發筆記

### CSS Naming Convention CSS 命名規則

- 區塊級：`.section-*`, `.container`
- 元件級：`.book-card`, `.testimonial`
- 工具類：`.margin-bottom-md`, `.text-align-center`

### JavaScript Design Principles JavaScript 設計原則

- 無依賴：不使用 jQuery 或其他框架
- 漸進增強：沒有 JavaScript 也能瀏覽基本內容
- Null-safe：所有 DOM 操作前檢查元素存在

### Browser Support 瀏覽器支援

- Chrome/Edge 90+
- Firefox 88+
- Safari 14+
- Mobile Safari 14+

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/file-text-light.svg" width="24" height="24" align="center" /> License 授權

本專案為展示用途 (Demo)，圖片與素材來源：

- Unsplash (攝影圖片)
- Pexels (部分圖片)
- 公開圖庫素材
- 出版社官方素材 (非實際合作廠商)

**重要提示**：本專案僅供學習與作品集展示，所有商標、品牌名稱僅為範例，不代表實際商業合作或授權關係。

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/user-gear-light.svg" width="24" height="24" align="center" /> Developer 開發者

開發日期：2026 年 3 月
<br/>
susx alan

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/graduation-cap-light.svg" width="24" height="24" align="center" /> Reference Resources 參考資源

本專案開發過程中參考了以下優質教學資源：

### HTML & CSS Tutorial HTML & CSS 教程

- **[Jonas Schmedtmann - HTML & CSS Course](https://github.com/jonasschmedtmann/html-css-course)**
  完整的 HTML 與 CSS 教學課程，涵蓋響應式設計、Flexbox、Grid 等現代前端技術

---

## <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/star-light.svg" width="24" height="24" align="center" /> Special Thanks 特別感謝

感謝所有支持獨立書店、寵物友善商家、在地出版社的朋友們。讓我們一起創造更溫暖的閱讀與社區連結！

---

**<img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/house-line-light.svg" width="20" height="20" align="center" /> Padi Bookstore - Where Books Meet Buddies <img src="https://cdn.jsdelivr.net/gh/phosphor-icons/core@main/assets/light/books-light.svg" width="20" height="20" align="center" />**
