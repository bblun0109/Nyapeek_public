
# Nyapeek

**Multi-engine OCR + multi-LLM screen translation overlay for Windows — real-time, hotkey-driven, built on Translumo.**

English | [繁體中文](README.zh-TW.md)

`License: Apache-2.0` · `Platform: Windows 10/11` · `.NET 8`

</div>

---

## Overview

Nyapeek is a Windows desktop application that captures any region of your screen, runs OCR on it, translates the recognized text through your choice of translation backend, and shows the result in a customizable floating overlay — all triggered by a single hotkey.

It is built for gamers playing imported titles, readers of foreign-language documents, and anyone who needs in-place translation without copy-pasting or switching apps. Unlike single-engine translators, Nyapeek lets you mix and match three OCR engines with four translation backends (including modern LLMs), so you can tune accuracy, speed, and cost for each use case.

Nyapeek is derived from [Translumo](https://github.com/Danily07/Translumo) and extends it with additional translation backends, translation history, an updated design system, and encrypted local key storage.

## Features

- **Real-time screen-region OCR + translation overlay** triggered by global hotkeys
- **3 OCR engines** — Windows OCR, EasyOCR, Tesseract — each tuned for different background/font conditions
- **4 translation backends** — Google Translate (built-in) plus three AI / LLM services: OpenRouter, Cerebras, Bai
- **35 target languages**, with native OCR support for English, Chinese (Simplified & Traditional), Japanese, Korean, and Russian
- **3 translation styles** — Literal, Natural, GameDialogue (tuned for in-game text)
- **Local translation history** — the last 50 translations are kept on-device
- **Encrypted API key storage** using Windows DPAPI — keys never leave your machine
- **Customizable overlay** — color, font, size, opacity, alignment
- **Windows Text-to-Speech** for the translated result
- **Tray icon** with quick access and minimize-to-tray
- **Settings panels** for Languages, Appearance, OCR, and Hotkeys

## System Requirements

- **OS:** Windows 10 (build 19041 / 21H2) or Windows 11
- **Runtime:** .NET 8 Desktop Runtime — bundled in published releases (self-contained)
- **Disk:** ~200 MB free (includes embedded Python 3.8 and OCR prediction models)
- **GPU (optional):** an NVIDIA GPU accelerates EasyOCR; CPU works fine for the other engines
- **API keys (optional):** required only if you use OpenRouter, Cerebras, or Bai. Google Translate works without a key.

## Installation

1. Download the latest release ZIP from the Releases page.
2. Extract it to any folder (e.g. `C:\Apps\Nyapeek`).
3. Run `Nyapeek.exe`.
4. On first launch:
   - Open **Settings** and choose your source/target languages.
   - Pick your preferred OCR engine and translation backend.
   - If using an AI / LLM backend, paste your API key (stored encrypted via DPAPI).
   - Assign hotkeys for screen capture and translate.

A release folder must contain `Nyapeek.exe`, the `python\` runtime (`python38.dll`, `python38.zip`, `vcruntime140.dll`), and the `models\Prediction\` folder for each language you plan to OCR (`eng`, `chi`, `jap`, `kor`, `rus`). If you build your own release, run `scripts\Verify-NyapeekRelease.ps1` against the output folder to confirm it is complete.

## Usage

1. Launch Nyapeek; it minimizes to the tray.
2. Press your configured **capture hotkey** and drag a rectangle over the text on screen.
3. Nyapeek runs OCR, sends the recognized text to your chosen translator, and displays the result in a floating overlay near the source region.
4. Use the tray menu or settings hotkey to open the configuration panel and adjust engines, languages, appearance, or hotkeys at any time.
5. Recent translations are available in the history view.

## Project Structure

```
Nyapeek/
├── src/
│   ├── Nyapeek/                  WPF main application (UI, MVVM, hotkeys)
│   ├── Nyapeek.Infrastructure/   DPAPI encryption, languages, Python interop
│   ├── Nyapeek.OCR/              Windows OCR, EasyOCR, Tesseract integrations
│   ├── Nyapeek.Processing/       Text processing, history, language prediction
│   ├── Nyapeek.Translation/      Google / OpenRouter / Cerebras / Bai backends
│   ├── Nyapeek.TTS/              Text-to-speech engines
│   └── Nyapeek.Utils/            Shared utilities
├── scripts/
│   ├── Publish-NyapeekRelease.ps1
│   └── Verify-NyapeekRelease.ps1
└── Nyapeek.sln
```

## Building from Source

Requirements: Windows 10/11, .NET 8 SDK, Python 3.8 (only required for EasyOCR development).

```powershell
# From the Nyapeek/ directory
dotnet build src\Nyapeek\Nyapeek.csproj -c Release

# To produce a full release folder (includes Python runtime + models):
powershell -NoProfile -ExecutionPolicy Bypass `
  -File scripts\Publish-NyapeekRelease.ps1 -Version v1.0.0

# To verify a release folder is runnable:
powershell -NoProfile -ExecutionPolicy Bypass `
  -File scripts\Verify-NyapeekRelease.ps1 -ReleasePath "<path-to-release>"
```

## Credits

Nyapeek stands on the shoulders of the [**Translumo**](https://github.com/Danily07/Translumo) project — original author **Danil Iushkov** and upstream maintainer **Aleksei Vekhov**. The core capture pipeline, OCR plumbing, and many UI patterns originate there.

Additional thanks to the open-source projects that make Nyapeek possible:

- [Tesseract OCR](https://github.com/tesseract-ocr/tesseract)
- [EasyOCR](https://github.com/JaidedAI/EasyOCR)
- Microsoft Windows OCR
- [MaterialDesignInXAML](https://github.com/MaterialDesignInXAML/MaterialDesignInXamlToolkit)
- [Serilog](https://serilog.net/), [Newtonsoft.Json](https://www.newtonsoft.com/json), [SharpDX](http://sharpdx.org/)

## License

This project is licensed under the Apache-2.0 license. See the LICENSE and NOTICE files.
---

<div align="center">

Made for the gaming and translation community.

</div>
