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
- **換個角度潤稿**：選擇「心理師督導／社工師督導／演講教練」，AI 依該專業的判準修整用字遣詞，另外產生一份改善版逐字稿加上三到五條督導提醒。原稿不會被動到，要按「採用」才會取代。可選 OpenAI、Claude 或 Gemini。
- **朗讀**：用瀏覽器內建語音唸逐字稿，可選聲音、調語速、暫停；選取部分文字就只唸那一段

## 瀏覽器需求

| 功能 | 需求 |
| --- | --- |
| 錄音 | Chrome、Edge、Safari、Firefox 都可以 |
| 即時語音辨識 | **只有 Chrome 和 Edge 支援**（Web Speech API），Firefox 不支援 |
| 朗讀 | 主流瀏覽器都支援，聲音清單依系統而定 |

## AI 金鑰（選用）

金鑰只存在瀏覽器的 localStorage，填哪一家就用哪一家。

| 功能 | 可用服務 | 金鑰去哪裡拿 |
| --- | --- | --- |
| Whisper 轉寫 | 只支援 OpenAI | https://platform.openai.com/api-keys |
| AI 潤稿 | OpenAI／Claude／Gemini 三選一 | Claude: https://console.anthropic.com ／ Gemini: https://aistudio.google.com/apikey |

三家都支援從瀏覽器直接呼叫（Claude 需要送出 `anthropic-dangerous-direct-browser-access` 標頭，程式裡已經加了）。

模型名稱改版很快，所以模型欄位是可以直接打字的，旁邊的「取得清單」會用你的金鑰去問該服務目前有哪些模型，選一個就好。預設值分別是 `gpt-5.6-sol`、`claude-sonnet-5`、`gemini-3.5-flash`，如果報錯說模型不存在，按一下「取得清單」換一個即可。

潤稿的提示詞會明確要求模型只改表達方式，不得增加原本沒說過的事實、人名或數字；逐字稿超過約 3500 字會自動分段處理再接回來。

- 金鑰只存在瀏覽器的 localStorage，不會上傳到這個網站
- 錄音檔是從瀏覽器直接送到 OpenAI，會依 OpenAI 的費率計費

## 資料保存

錄音和逐字稿存在瀏覽器的 IndexedDB，綁在「這台裝置的這個瀏覽器」。
換裝置、換瀏覽器或清除網站資料就會不見，重要的內容記得按下載存成檔案。

## 逐字稿沒有出現時

錄音區下方有一行「辨識狀態」，它會直接說明卡在哪裡：

| 顯示 | 意思 |
| --- | --- |
| 這個瀏覽器不支援即時辨識 | 換 Chrome 或 Edge；iPhone／iPad 上所有瀏覽器都是 WebKit，多半不支援 |
| 辨識拿不到麥克風 | 麥克風被錄音或其他程式佔用，錄音仍會完成，結束後用 Whisper 轉寫 |
| 麥克風權限被拒絕 | 到瀏覽器網站設定把麥克風改成允許 |
| 辨識服務連不上 | 即時辨識需要連網，離線不能用 |
| 12 秒沒有辨識到文字 | 先播放錄音確認有聲音，有聲音就是辨識沒生效，改用 Whisper |

注意：即時辨識必須在使用者按下按鈕的那個動作裡啟動，而且網頁要用 https 開啟，用 file:// 直接開本機檔案不會動。
