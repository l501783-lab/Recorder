# 錄音逐字稿

一個單檔網頁工具：錄音、即時轉逐字稿、下載 .txt、用語音唸回來。
沒有伺服器，所有錄音和文字都留在使用者自己的瀏覽器裡。

## 放到 GitHub Pages

1. 在 GitHub 建一個新的 repository（Public），例如 `voice-notes`
2. 按 **Add file → Upload files**，把 `index.html` 拖進去，Commit
3. 進 **Settings → Pages**，Source 選 `Deploy from a branch`，分支選 `main`、目錄選 `/ (root)`，Save
4. 等一兩分鐘，網址是 `https://你的帳號.github.io/voice-notes/`

必須用 https 網址開啟，麥克風才會被允許。直接雙擊本機的 index.html 會被瀏覽器擋掉麥克風。

## 功能

- **錄音**：按中間的圓鈕開始／結束，有計時和音量顯示
- **即時逐字稿**：邊錄邊出字，可直接在框裡修改
- **時間戳記**：勾選後每句前面加上 `[01:23]`，可隨時開關
- **多段管理**：每次錄音自成一段，可開啟、改名、刪除、下載
- **下載**：音檔（.webm／.m4a）、單段 .txt、全部合併 .txt（含 BOM，Windows 記事本不會亂碼）
- **朗讀**：用瀏覽器內建語音唸逐字稿，可選聲音、調語速、暫停；選取部分文字就只唸那一段

## 瀏覽器需求

| 功能 | 需求 |
| --- | --- |
| 錄音 | Chrome、Edge、Safari、Firefox 都可以 |
| 即時語音辨識 | **只有 Chrome 和 Edge 支援**（Web Speech API），Firefox 不支援 |
| 朗讀 | 主流瀏覽器都支援，聲音清單依系統而定 |

## Whisper（選用）

覺得即時辨識不夠準時，在「Whisper 設定」貼上 OpenAI API 金鑰，就能把錄好的音檔重新轉寫一次，準確度和時間戳記都好很多。

- 金鑰只存在瀏覽器的 localStorage，不會上傳到這個網站
- 錄音檔是從瀏覽器直接送到 OpenAI，會依 OpenAI 的費率計費

## 資料保存

錄音和逐字稿存在瀏覽器的 IndexedDB，綁在「這台裝置的這個瀏覽器」。
換裝置、換瀏覽器或清除網站資料就會不見，重要的內容記得按下載存成檔案。
