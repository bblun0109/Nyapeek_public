<div align="center">

<img src="nyapeek-logo.png" alt="Nyapeek" width="160" />

# Nyapeek

**多引擎 OCR ＋ 多家 LLM 即時螢幕翻譯浮動視窗（Windows） — 熱鍵驅動，基於 Translumo 衍生。**

[English](README.md) | 繁體中文

`授權：TBD` · `平台：Windows 10/11` · `.NET 8`

</div>

---

## 簡介

Nyapeek 是一款 Windows 桌面應用程式：你框選螢幕任意區域，它會對該區域做 OCR、把辨識到的文字送到你選擇的翻譯後端，並把結果以可自訂的浮動視窗顯示在原位 — 整個流程只需要一個熱鍵。

設計目標是為了玩外語遊戲的玩家、閱讀外文文件的人，以及任何需要「就地翻譯」而不想反覆複製貼上或切換視窗的使用者。與單一引擎的翻譯工具不同，Nyapeek 讓你**自由組合三種 OCR 引擎與四種翻譯後端**（包含當代 LLM），可依場景調整準確率、速度與成本。

Nyapeek 衍生自 [Translumo](https://github.com/Danily07/Translumo)，並擴充了更多翻譯後端、翻譯歷史、新版設計系統，以及本機加密的 API 金鑰儲存。

## 功能特色

- **即時螢幕區域 OCR ＋ 翻譯浮動視窗**，由全域熱鍵觸發
- **3 種 OCR 引擎** — Windows OCR、EasyOCR、Tesseract，分別適合不同背景與字型
- **4 種翻譯後端** — 內建 Google Translate，加上三家 AI / LLM 服務：OpenRouter、Cerebras、Bai
- **35 種目標語言**，OCR 原生支援英文、中文（簡體與繁體）、日文、韓文、俄文
- **3 種翻譯風格** — Literal（直譯）、Natural（自然）、GameDialogue（遊戲對話）
- **本機翻譯歷史** — 自動保留最近 50 筆翻譯
- **API 金鑰加密儲存**，採用 Windows DPAPI — 金鑰不會離開你的電腦
- **可自訂浮動視窗** — 顏色、字體、字級、透明度、對齊方式
- **Windows 文字轉語音**，可朗讀翻譯結果
- **系統匣圖示**，支援快速存取與縮到匣
- **設定面板** 分為語言、外觀、OCR、熱鍵四個分頁

## 系統需求

- **作業系統：** Windows 10（build 19041 / 21H2）或 Windows 11
- **執行環境：** .NET 8 Desktop Runtime — 發布版已自帶（self-contained）
- **硬碟空間：** 約 200 MB（含內嵌 Python 3.8 與 OCR 預測模型）
- **顯示卡（選配）：** NVIDIA GPU 可加速 EasyOCR；其他引擎使用 CPU 也能正常運作
- **API 金鑰（選配）：** 僅在使用 OpenRouter、Cerebras、Bai 時需要。Google Translate 不需要金鑰。

## 安裝方式

1. 從 Releases 頁面下載最新版的 ZIP 壓縮檔。
2. 解壓到任一資料夾（例如 `C:\Apps\Nyapeek`）。
3. 執行 `Nyapeek.exe`。
4. 首次啟動：
   - 打開 **設定**，選擇來源與目標語言。
   - 選擇偏好的 OCR 引擎與翻譯後端。
   - 若使用 AI / LLM 後端，請貼上 API 金鑰（會以 DPAPI 加密保存）。
   - 設定螢幕擷取與翻譯的熱鍵。

完整的發布資料夾應包含 `Nyapeek.exe`、`python\` 執行環境（`python38.dll`、`python38.zip`、`vcruntime140.dll`），以及 `models\Prediction\` 下各語言模型（`eng`、`chi`、`jap`、`kor`、`rus`）。如果你是自己打包發布版，可以執行 `scripts\Verify-NyapeekRelease.ps1` 驗證輸出資料夾完整。

## 使用方式

1. 啟動 Nyapeek 後，會自動縮到系統匣。
2. 按下你設定的 **擷取熱鍵**，在螢幕上拖曳框選要翻譯的文字區域。
3. Nyapeek 會執行 OCR、把文字送到你選的翻譯引擎，並在原區域附近顯示翻譯結果。
4. 透過系統匣選單或設定熱鍵，可隨時打開設定面板調整引擎、語言、外觀、熱鍵。
5. 最近的翻譯結果可在歷史記錄頁面檢視。

## 專案結構

```
Nyapeek/
├── src/
│   ├── Nyapeek/                  WPF 主程式（UI、MVVM、熱鍵）
│   ├── Nyapeek.Infrastructure/   DPAPI 加密、語言、Python interop
│   ├── Nyapeek.OCR/              Windows OCR、EasyOCR、Tesseract 整合
│   ├── Nyapeek.Processing/       文字處理、歷史、語言預測
│   ├── Nyapeek.Translation/      Google／OpenRouter／Cerebras／Bai 後端
│   ├── Nyapeek.TTS/              文字轉語音
│   └── Nyapeek.Utils/            共用工具
├── scripts/
│   ├── Publish-NyapeekRelease.ps1
│   └── Verify-NyapeekRelease.ps1
└── Nyapeek.sln
```

## 從原始碼建置

需求：Windows 10/11、.NET 8 SDK，以及 Python 3.8（僅當你要開發 EasyOCR 相關功能時需要）。

```powershell
# 在 Nyapeek/ 目錄下
dotnet build src\Nyapeek\Nyapeek.csproj -c Release

# 產生完整的發布資料夾（含 Python runtime 與模型）：
powershell -NoProfile -ExecutionPolicy Bypass `
  -File scripts\Publish-NyapeekRelease.ps1 -Version v1.0.0

# 驗證發布資料夾是否可執行：
powershell -NoProfile -ExecutionPolicy Bypass `
  -File scripts\Verify-NyapeekRelease.ps1 -ReleasePath "<發布資料夾路徑>"
```

## 致謝

Nyapeek 站在 [**Translumo**](https://github.com/Danily07/Translumo) 專案的肩膀上 — 原作者 **Danil Iushkov** 與上游維護者 **Aleksei Vekhov**。核心的螢幕擷取流程、OCR 管線、以及許多 UI 模式均源自該專案。

亦特別感謝下列開源專案：

- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [EasyOCR](https://github.com/JaidedAI/EasyOCR)
- Microsoft Windows OCR
- [MaterialDesignInXAML](https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit)
- [Serilog](https://serilog.net/)、[Newtonsoft.Json](https://www.newtonsoft.com/json)、[SharpDX](http://sharpdx.org/)

## 授權

`授權：TBD` — 正式的 LICENSE 檔案將於後續版本補上。

---

<div align="center">

獻給遊戲與翻譯社群。

</div>
