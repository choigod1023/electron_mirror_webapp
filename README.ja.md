# electron_mirror_webapp

[한국어](README.md) · **日本語** · [English](README.en.md)

> Raspberry Pi で作るスマートミラー用の UI — Electron.js ベースの時計・天気ダッシュボード

## 紹介

**electron_mirror_webapp** は、Raspberry Pi ベースのスマートミラー画面に表示する UI を Electron.js で実装したプロジェクトです。黒背景に白文字のミラー向けダッシュボードで、現在時刻と日付、現在の天気、以降の時間帯別予報を 1 画面にまとめて表示します。ウィンドウサイズはミラーのディスプレイに合わせて 1024×600 に固定されています。

## ✨ 主な機能（コードベース）

- **時計 & 日付**: 12 時間表記（AM/PM）の時刻と「M月D日 ○曜日」（韓国語の曜日）を右側に大きく表示します。
- **現在の天気**: OpenWeatherMap のデータを取得し、現在の天気アイコン・気温（°）・湿度（%）を表示します。
- **時間帯別予報**: 以降 5 つの時間帯の時刻（午前/午後）、天気アイコン、気温、湿度をリスト表示します。
- **状況に応じた天気アイコンのマッピング**: 天気状態（Rain/Clouds/Snow/Thunderstorm/Mist・Haze・Fog/Clear）と雲量・日の出／日の入り時刻を組み合わせ、`img/` フォルダの適切なアイコン（晴れ／曇り／夜／雪／嵐／霧など）を選びます。(`return_img()`)
- **ミラー向けの UI**: 黒背景・白テキスト・Nanum Gothic フォントで構成され、鏡に反射して見やすいデザインです。

## 🛠 技術スタック

![Electron](https://img.shields.io/badge/Electron-12-47848F?logo=electron&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)
![OpenWeatherMap](https://img.shields.io/badge/OpenWeatherMap-API-EB6E4B?logo=openweathermap&logoColor=white)

- **Electron.js** (`^12.0.5`) — デスクトップシェル / ブラウザウィンドウ
- **JavaScript / HTML / CSS**（Vanilla）
- **request** — OpenWeatherMap One Call API の呼び出し
- **nodemon**（devDependency）

## 🏗 動作の仕組み / アーキテクチャ

- **メインプロセス**（`main.js`）: `app.whenReady()` の後に `BrowserWindow`（1024×600, `nodeIntegration: true`, `contextIsolation: false`）を生成し、`index.html` を読み込みます。macOS の慣習に沿った `activate` / `window-all-closed` のハンドリングを含みます。
- **レンダラープロセス**（`index.html` + `renderer.js`）: ページ読み込み時に現在時刻／日付を計算して `#clock` に描画し、OpenWeatherMap One Call API（`api.openweathermap.org/data/2.5/onecall`）を呼び出して現在の天気（`#current`）と時間帯別予報（`#hours`）を埋めます。
- **アセット**（`img/`）: 天気状態別のアイコン PNG（`sun_clear`, `sun_cloud`, `cloud`, `night_clear`, `night_cloud`, `rainy`, `snow`, `storm`, `fog`）。

## 🚀 はじめかた

### 前提条件

- [Node.js](https://nodejs.org)（npm 同梱）
- [OpenWeatherMap](https://openweathermap.org/api) の API キー

### インストール

```bash
git clone https://github.com/choigod1023/electron_mirror_webapp.git
cd electron_mirror_webapp
npm install
```

### 設定（天気 API）

天気データは `renderer.js` の OpenWeatherMap One Call API 呼び出しで取得します。現在のコードには API キー（`appid`）と位置座標（`lat`, `lon`）がハードコードされているため、利用前に **ご自身の OpenWeatherMap API キーと目的の地域の座標へ差し替えて** ください。

```js
// renderer.js
request('https://api.openweathermap.org/data/2.5/onecall?lat=<緯度>&lon=<経度>&appid=<自分の_API_KEY>&units=metric', ...)
```

> セキュリティ上の注意: リポジトリにコミットされているサンプル API キーは、直ちに差し替え（再発行）することを推奨します。

### 実行

```bash
npx electron .
```

> `package.json` の `start` スクリプトは `electromon .` と定義されています。標準の起動方法は上記のように Electron を直接実行することで、開発中に自動再起動が必要なら `nodemon` を利用できます。

## 📁 構成

```
electron_mirror_webapp/
├── main.js          # Electron メインプロセス (BrowserWindow 1024x600)
├── preload.js       # プリロードスクリプト（現在はほぼコメントアウト）
├── index.html       # ミラー UI のレイアウト・スタイル
├── renderer.js      # 時計・天気ロジック（OpenWeatherMap 呼び出し、アイコンのマッピング）
├── img/             # 天気状態別のアイコン PNG
└── package.json
```

## 📝 ライセンス

[CC0 1.0 (Public Domain)](LICENSE.md)

---

## 👤 コントリビューションと開発環境

| 項目 | 内容 |
|---|---|
| **貢献比率** | 本人のコミット **2 件** — `electron-quick-start` テンプレートを履歴ごと取り込んで開始したリポジトリのため、コミット比率（1.4%）には意味がありません |
| **参加人数** | 1 名（残りのコミットはテンプレートの原作者たち） |

<sub>貢献比率はコミットの author メールアドレス基準で集計し、ボット・自動化コミットは除外しています。</sub>
