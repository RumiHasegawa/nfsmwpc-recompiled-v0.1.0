# Need for Speed Most Wanted (2005, PC) — Recompiled (v0.1.0 Custom Release)

Static recompilation of the **PC version** of *Need for Speed: Most Wanted* (2005, `speed.exe`, x86) to native code, first as a Windows validation host and then for **Android ARM64 and ARMv7**. This is the third game of the *NFS Android Evolved* family, following Underground 2 and Underground 1.

**English** · [日本語](#日本語)

This is **not** an emulator and **not** a port of the Xbox 360 version: every function of the original PC executable is translated ahead of time from x86 to C with `pcrecomp` (patched), then compiled by MSVC (Windows) or the Android NDK (ARM64 / ARMv7).

> No game files are included. You need your own copy of the PC game. Generated C code, analysis copies and logs are produced locally and are never committed.

---

### Status & Milestones

| Milestone | State | Details |
|---|---|---|
| **MW-M0** Executable analysis (`speed.exe`, SHA-256 `80774c2e…253c`) | ✅ | Direct3D 9 + D3DX effects, DirectInput 8, DirectSound, relocations recovered |
| **MW-M1** Function catalog | ✅ | 41,262 functions (data, switch-table, immediate and forced seeds) |
| **MW-M2** C generation | ✅ | 0 lift failures |
| **MW-M3** Windows host build and link | ✅ | Working executable validation |
| **MW-M4/M5** CRT, memory, original entry point | ✅ | Native CRT overrides and memory mapping |
| **MW-M6/M7** Win32, Direct3D 9, first frame | ✅ | D3DX effects loaded from the game's own resources |
| **MW-M8/M9** Intro, menus, world | ✅ | Cinematics, front end, career map and free roam; touch cursor, keyboard and gamepad |
| **MW-M10+** Racing, police, saving | ✅ | Fully playable; LAN and Online (EA Nation replacement server) |
| **Android ARM64** | ✅ | Playable on modern devices (Android 10+, Vulkan 1.1+) |
| **Android ARMv7** | ⚠️ | Builds for 32-bit devices |

---

### Features & Built-in Enhancements

* **Native Widescreen & FOV Fix:** Based on ThirteenAG's WidescreenFix. Adjusts viewport and proportional HUD anchors automatically for 16:9, 18:9, 19.5:9, and 20:9 smartphone aspect ratios without over-zooming the car camera.
* **Touch Controls & Custom Layouts:** Movable and resizable on-screen controls, with Classic, PlayStation, and Xbox themes, Gyro tilt steering, and Bluetooth/USB controller input.
* **Vulkan Graphics Engine:** Hardware acceleration via DXVK/Vulkan bridge with support for custom Turnip drivers on Adreno GPUs.
* **Audio Engine:** Dynamic low-latency audio buffer scaling to eliminate stutter on MediaTek, Mali, and Unisoc processors.
* **Mod Support (Asset Replacements):** Fully compatible with texture packs, HD car models, and environment overhauls from popular mods such as **Redux, Rework, and Ultimate Edition**.

---

### Playing on Android

1. Install the APK for your architecture (`NFSMW-AndroidEvolved-v0.1.0-arm64.apk` recommended).
2. Copy your PC installation into `Internal storage/nfsmw` so that `nfsmw/Speed.exe` exists next to:
   * Folders: `GLOBAL`, `TRACKS`, `CARS`, `FRONTEND`, `LANGUAGES`, `SOUND`, `MOVIES`, `NIS`.
   * Files: `d3dx9_26.dll` (DirectX runtime from Windows `SysWOW64`).
   * LAN / Server (Optional): `server.dll` and `server.cfg`.
3. **Mod Installation Instructions (Redux / Rework / Ultimate):**
   * **Allowed:** Asset replacement files (e.g. `CARS/`, `TRACKS/`, `FRONTEND/`, `GLOBAL/`, `SOUND/`).
   * **Do Not Copy:** PC-specific binary script files (`scripts/*.asi`, `dinput8.dll`, `NFSMWExtraOptions.asi`) because they are x86 Windows DLLs and will crash or break on ARM Android.
4. Launch the application, grant storage permissions, choose your display preferences, and press **PLAY**.

---

### Repository Layout

```text
android/   Android app (launcher, touch controls, D3D9→Vulkan bridge, guest runtime)
config/    reference executable, forced seeds, hooks, overrides (Most Wanted only)
docs/      binary report, bring-up documentation, audio frame pacing
scripts/   binary analysis, seed discovery, lift driver, Windows build/test
src/       Windows host, runtime adapters, Most Wanted shims
work/      (local only) analysis copy, catalog, logs
```

---

### Automated Cloud Build (GitHub Actions)

This repository includes a pre-configured CI/CD workflow located at `.github/workflows/build-apk.yml`.
When you push this repository to GitHub, the workflow automatically compiles the project using Android NDK r28 and generates the release APK artifact in the **Actions** tab.

---

### License, Credits & Copyright

* **Custom Release & Android Port Maintenance:** Copyright © 2026 **ShiroiYamato**. All rights reserved.
* **Original Static Recompilation Engine:** `pcrecomp` by sp00nznet & `nfsmwpc-recompiled` project by codepdbh.
* **Widescreen Fix & HUD Scaling:** Adapted from ThirteenAG's MIT WidescreenFix ([notice](THIRD_PARTY_NOTICES.md)).
* *Need for Speed* is a registered trademark of Electronic Arts Inc. This project is a fan-made, non-commercial static recompilation and is not affiliated with or endorsed by Electronic Arts.

---

## 日本語

『ニード・フォー・スピード モスト・ウォンテッド』（2005年、PC版 `speed.exe`、x86）を**Android（ARM64 / ARMv7）**およびWindows向けに静的再コンパイル（Static Recompilation）したプロジェクトです。『NFS Android Evolved』シリーズの第3作目となります。

これはエミュレーターではなく、PC版実行ファイルの全機械語命令を事前にC言語コードへと逆コンパイル・変換し、Android NDKを通じてARMネイティブコードとしてビルド・実行しています。

> ゲーム本体のデータファイルは含まれていません。正規のPC版ゲームディスクまたはインストールデータが必要です。

---

### 開発ステータス・マイルストーン

| マイルストーン | 状態 | 詳細 |
|---|---|---|
| **MW-M0** 実行ファイル解析 (`speed.exe`, SHA-256 `80774c2e…253c`) | ✅ | Direct3D 9、D3DXエフェクト、DirectInput 8、DirectSound、再配置テーブル復元 |
| **MW-M1** 関数カタログ化 | ✅ | 41,262関数を特定・登録 |
| **MW-M2** C言語コード生成 | ✅ | リフト（変換）エラー 0件 |
| **MW-M3** Windowsホストビルド・リンク | ✅ | Windowsネイティブ実行確認 |
| **MW-M4/M5** CRT・メモリ・エントリーポイント | ✅ | ネイティブCRTオーバーライドおよびメモリマッピング完了 |
| **MW-M6/M7** Win32 / Direct3D 9 描画 | ✅ | ゲームリソースからD3DXエフェクトロード完了 |
| **MW-M8/M9** オープニング・メニュー・ワールド | ✅ | ムービー、UI、キャリアマップ、フリー走行、タッチカーソル、キーボード、パッド操作確認 |
| **MW-M10+** レース・警察追走・セーブ | ✅ | 実機での完全プレイ可能。LANおよび代替オンライン対戦対応 |
| **Android ARM64** | ✅ | 動作確認済み（Android 10以上、Vulkan 1.1以上） |
| **Android ARMv7** | ⚠️ | 32bit向けビルド対応 |

---

### 主な機能および内蔵強化要素

* **ネイティブ・ワイドスクリーン＆視野角（FOV）補正:**
  ThirteenAGのWidescreenFixアルゴリズムをC言語で直接組み込み済み。18:9、19.5:9、20:9といったスマートフォンの細長画面でも、車が画面いっぱいにズームされず、自然な視野角とプロポーショナルなHUD表示を維持します。
* **高機能タッチ操作・UIレイアウト:**
  画面上のボタン配置・サイズを自由に変更可能。Classic、PlayStation、Xboxレイアウトの切り替え、ジャイロ（端末の傾き）操作、Bluetooth/USBゲームパッドに対応。
* **Vulkanグラフィックスエンジン:**
  DXVK/Vulkanブリッジにより高効率レンダリングを実現。Adreno GPU向けのTurnipカスタムドライバー読み込みに対応。
* **オーディオ最適化:**
  MediaTek、Mali、Unisocプロセッサでの音飛びやバッファ詰まりを防止する動的オーディオバッファ調整を実装。
* **MOD（アセット置き換え）対応:**
  **Redux、Rework、Ultimate Edition**などの高解像度テクスチャ、車体モデル、環境アセットをそのまま導入可能。

---

### Androidでの導入手順

1. 端末のCPUに合ったAPK（通常は `NFSMW-AndroidEvolved-v0.1.0-arm64.apk`）をインストールします。
2. スマホの内部ストレージ直下に `nfsmw` という名前のフォルダーを作成します:
   ```text
   内部ストレージ/nfsmw/
   ```
3. お手持ちのPC版ゲームから以下のフォルダーおよびファイルを `nfsmw` フォルダー直下へコピーします:
   * **フォルダー:** `GLOBAL`, `TRACKS`, `CARS`, `FRONTEND`, `LANGUAGES`, `SOUND`, `MOVIES`, `NIS`
   * **必須ファイル:** `Speed.exe` (v1.3)、`d3dx9_26.dll` (Windowsの `SysWOW64` から取得)
   * **LAN対戦ファイル（任意）:** `server.dll`, `server.cfg`
4. **MOD（Redux / Rework / Ultimate等）を導入する場合の注意点:**
   * **使用可能:** 車両データ（`CARS/`）、コース・テクスチャ（`TRACKS/`）、UI（`FRONTEND/`）、音声（`SOUND/`）などのアセットファイル。
   * **絶対に入れないでください:** `scripts/*.asi`、`dinput8.dll`、`NFSMWExtraOptions.asi` などのPC用スクリプト/DLLファイル（x86 Windows専用バイナリのため、Android ARMで読み込むとクラッシュやフリーズの原因となります）。
5. ランチャーアプリを開き、ストレージアクセスを許可して **PLAY** を押します。

---

### ディレクトリ構成

```text
android/   Androidアプリ本体（ランチャー、タッチ操作、D3D9→Vulkanブリッジ、ランタイム）
config/    実行ファイル参照データ、シード、フック、オーバーライド設定
docs/      バイナリ解析レポート、移植ドキュメント、フレームペーシング解説
scripts/   バイナリ解析・リフト用Pythonスクリプト群
src/       Windowsホストコード、ランタイムアダプター、Most Wanted用シム
work/      （ローカル作業用）解析キャッシュ、カタログ、ログ
```

---

### GitHub Actions による自動ビルド

本リポジトリには `.github/workflows/build-apk.yml` が同梱されています。GitHubにプッシュするだけで、クラウド上で自動的にAndroid NDK r28を用いたビルドが実行され、**Actions** タブからビルド済みAPKをダウンロードできます。

---

### ライセンス・著作権・クレジット

* **カスタムリリース＆Android移植保守:** Copyright © 2026 **ShiroiYamato**. All rights reserved.
* **静的再コンパイル（Static Recompilation）エンジン:** `pcrecomp` by sp00nznet & `nfsmwpc-recompiled` project by codepdbh.
* **ワイドスクリーン対応（Widescreen Fix & HUD）:** ThirteenAG 氏の MIT WidescreenFix 実装をベースに統合（[表記](THIRD_PARTY_NOTICES.md)）。
* 『Need for Speed』は Electronic Arts Inc. の登録商標です。本プロジェクトはファンによる非商用の静的再コンパイルプロジェクトであり、Electronic Arts とは一切関係ありません。

