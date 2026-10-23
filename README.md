# 海后模擬器｜可遊玩網頁原型

一款以架空台灣都市「霧港市」為背景的手機戀愛模擬器。包含六位男主、三選項劇情、關係數值、手機 App 介面、購物與外送、社群動態及多重結局。所有角色與平台內容皆為虛構設定。

## 本機啟動

需要安裝 Node.js 20 或更新版本。

```bash
npm install
npm run dev
```

依終端機顯示的網址開啟遊戲。

## 建置

```bash
npm run build
```

建置完成的靜態網站會輸出到 `dist/`。

## 部署到 GitHub Pages

1. 在 GitHub 建立一個新的 repository，例如 `sea-queen-simulator`。
2. 將本專案中的檔案上傳至 repository 根目錄（不要只上傳 `dist`）。
3. 在 repository 的 **Settings → Pages**，選擇 **GitHub Actions** 作為部署來源。
4. 新增 `.github/workflows/deploy.yml`（可參考下方範例），提交後等待 Actions 完成。
5. 部署完成後，GitHub Pages 會顯示網站網址。

建議工作流程 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:
permissions:
  contents: read
  pages: write
  id-token: write
concurrency:
  group: pages
  cancel-in-progress: true
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 22
          cache: npm
      - run: npm install
      - run: npm run build
      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    needs: build
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

## 目前原型功能

- 粉色、擬真、暗黑三種手機外觀切換
- 主畫面、聊天、Instagram 風格動態、Threads 風格貼文、購物、外送、戀愛日記、行事曆
- 六位男主的好感度、懷疑值、信任度
- 三個選項的劇情節點
- 購物與外送會扣除遊戲金錢
- 自動使用瀏覽器 `localStorage` 儲存進度
- 初版結局觸發與重新開始

## v0.2 角色專屬劇情擴充

- 六位男主新增情感需求、觀察特徵、關係底線與專屬個人事件
- LINE 回覆會依照訊息語氣產生不同角色反應，調整好感、懷疑與信任
- 當單一角色好感度達 40、信任度達 45，可解鎖個人事件並在「戀愛日記」查看
- 舊版本機存檔會自動合併新角色欄位，盡量保留既有數值

## 下一階段

- 以正式角色設定與圖片替換暫用頭像
- 讓個人事件成為可選擇的三選項支線，而不只是閱讀事件文本
- 完善九種正式結局的路線鎖定與解鎖紀錄
- 增加摯友名單、限時動態可見對象、購物訂單紀錄與更完整的社群事件
