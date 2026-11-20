# 日常 Dayflow

手機優先的個人行程與任務管理工具。此專案以 GitHub Pages 發佈靜態頁面，使用 Firebase Authentication 的 Google 登入，並將每位使用者的資料存入 Cloud Firestore。

## Firebase 設定

1. 在 Firebase Console 建立或選取一個 Firebase 專案，新增 Web app，複製 app 的設定物件。
2. 在 **Authentication → Sign-in method** 啟用 **Google**，並在 **Authentication → Settings → Authorized domains** 加入 GitHub Pages 網域：`bacon0502.github.io`。Firebase 的 authorized domains 不含 `https://` 或路徑。
3. 建立 Cloud Firestore 資料庫。此原型將整份個人資料存於 `users/{uid}/state/app`，每位 Google 使用者只能讀寫自己的文件。
4. 將 Web app 設定填入根目錄的 `firebase-config.js`。`apiKey`、`authDomain`、`projectId`、`appId` 均可從 Firebase Console 的 Web app 設定取得。這是瀏覽器公開設定，不可將服務帳戶私鑰或 Admin SDK 憑證放入前端。
5. 發佈 `firestore.rules`：在 Firebase Console 的 **Firestore Database → Rules** 貼上並發佈，或於已登入 Firebase CLI 的環境執行 `firebase deploy --only firestore:rules`。

Firestore 規則目前是單文件快照原型：要求登入、限制 UID 擁有者、預設拒絕其他路徑，並限制各資料清單長度。正式上線前請檢視規則；若要逐筆嚴格驗證每項任務、行程等欄位，應改為各實體分開存成文件。

## GitHub Pages

workflow 位於 `.github/workflows/deploy-pages.yml`。在 GitHub repo 開啟 **Settings → Pages**，將 **Build and deployment → Source** 設為 **GitHub Actions**。推送至 `main` 後，Actions 會部署 repo 根目錄；網站網址為 `https://bacon0502.github.io/1120/`。

## 本機預覽

由於使用 JavaScript modules，請透過本機 HTTP server 開啟（例如 VS Code Live Server），不要直接用 `file://` 開啟 `index.html`。

