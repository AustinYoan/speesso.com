# SPEESSO 官方網站

SPEESSO（Speed + Espresso）— 智慧咖啡取餐櫃品牌官網。
純靜態網站，直接部署到 GitHub Pages，綁定網域 `speesso.com`。

## 網站結構

| 檔案 | 說明 |
|---|---|
| `index.html` | 品牌首頁（即將上線 / 品牌概念 / 搶先體驗 / FAQ） |
| `order.html` | 線上點餐頁（含 LINE LIFF 與綠界金流接口，後端未接時自動用假資料展示） |
| `assets/style.css` | 共用設計系統（色彩 tokens、字體、按鈕、導覽列、頁尾） |
| `404.html` | 找不到頁面 |
| `CNAME` | GitHub Pages 自訂網域設定（`speesso.com`） |

## 視覺系統

所有顏色集中在 `assets/style.css` 的 `:root`：

- 森林綠底：`--speesso-forest: #041611`
- 品牌金：`--speesso-gold: #FCD34D`
- 品牌字體：Teko（Logo）＋ Noto Sans TC（內文）＋ Space Mono（數字/代碼）

要調整品牌色，只需要改 `:root` 這一段，兩個頁面會同步更新。

## 部署到 GitHub Pages

1. 在 GitHub 建立 repo（例如 `speesso.com`），把這個資料夾的內容推上去：

   ```bash
   git init
   git add .
   git commit -m "SPEESSO official site"
   git branch -M main
   git remote add origin https://github.com/<你的帳號>/speesso.com.git
   git push -u origin main
   ```

2. Repo → **Settings → Pages** → Source 選 `Deploy from a branch`，Branch 選 `main` / `(root)`，儲存。

3. **Settings → Pages → Custom domain** 填 `speesso.com`（repo 裡已有 `CNAME` 檔，通常會自動帶入），並勾選 **Enforce HTTPS**。

## DNS 設定（在你的網域註冊商後台）

| 類型 | 名稱 | 值 |
|---|---|---|
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |
| CNAME | `www` | `<你的帳號>.github.io` |

DNS 生效後（幾分鐘到 24 小時），`https://speesso.com` 就會指向這個網站。

## 點餐頁上線前要做的事

`order.html` 前端已完成，正式收單前需要：

1. 把 `CONFIG.liffId` 換成你在 LINE Developers 申請的 LIFF ID
2. 架設後端並實作兩支 API：
   - `GET /api/slots` → 回傳 `[{ time, capacity }]`
   - `POST /api/payment/create` → 建立 LINE Pay / 綠界訂單（金額與容量一律以後端為準）
3. 後端沒接上時，頁面會自動落到假資料模式，方便展示流程。
