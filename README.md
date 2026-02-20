# MaiML Studio

**MaiML Studio** は、 [MaiML（Measurement Analysis Instrument Markup Language）](http://www.maiml.org/) に準拠したファイルをビジュアルにデザイン・エクスポートするWebアプリです。

HTMLファイルを Chrome / Edge で開くだけで動作し、追加のインストールは一切不要です。

---

## 特徴

- **インストール不要** — `.html` ファイルをダブルクリックするだけで起動
- **ブラウザ完結** — Chrome / Edge（最新版）があれば Windows / Mac / Linux で動作
- **マルチメソッド対応** — 複数の実験・分析メソッドをタブで管理
- **プロジェクト保存/復元** — JSON形式での保存・読み込みに対応
- **MaiML エクスポート** — MaiML標準規格に準拠したXMLファイルを生成・ダウンロード
- **ダーク/ライトモード** — OSの設定を初期値として自動反映

---

## 使い方

1. `MaiMLStudio.html` をダウンロード
2. Chrome または Edge で開く
3. 各タブを順番に編集してMaiMLファイルをデザインする
4. ヘッダーの **Export MaiML (XML)** ボタンで `.maiml` ファイルを出力

---

## 画面構成

### 🗂 Document Metadata タブ

MaiMLファイル全体のメタデータを編集します。

| セクション | 内容 |
|-----------|------|
| **General Information** | Document ID・UUID・Document Name・Description を設定 |
| **Namespaces** | Property / Content の Key に使用する名前空間接頭辞（Prefix）とURIを登録。登録したPrefixはKey入力時の補完候補として表示される |
| **Global Properties** | ファイル全体に付与するプロパティ（Key / Type / Value / Description）を階層構造で追加・編集。`propertyListType` によるネスト構造に対応 |
| **Vendors** | 使用機器のメーカー情報を登録 |
| **Instruments** | 使用した分析機器を登録 |
| **Owners** | データオーナーを登録 |
| **Creators** | データ作成者を登録。登録済みの Vendor・Instrument とのリンクが可能 |

- Key フィールドはQName形式（`[a-zA-Z_][a-zA-Z0-9_.-]*` または `prefix:localName`）に準拠した入力チェック付き
- 禁止文字が入力された場合はフィールドが赤枠で警告表示

---

### 🔷 Petri Net Design タブ

実験・分析プロセスをペトリネットとして設計します。

**ノード操作**

| 操作 | 方法 |
|------|------|
| Place（円）の追加 | ツールバーの Place ボタン選択後、キャンバスをクリック |
| Transition（矩形）の追加 | ツールバーの Transition ボタン選択後、キャンバスをクリック |
| Arc（矢印）の接続 | ツールバーの Arc ボタン選択後、始点ノードから終点ノードへドラッグ |
| TemplateRef（青破線）の接続 | TemplateRef ボタン選択後、Place から Place へドラッグ |
| InstanceRef（緑破線）の接続 | InstanceRef ボタン選択後、Place から Place へドラッグ |
| ノードの移動 | Select モードでノードをドラッグ |
| ノードの削除 | Select モードでノードをクリック → 右パネルの Delete Node |
| キャンバスのパン | キャンバス背景をドラッグ |
| ズーム | マウスホイール |

**Place の種類**

- **通常 Place**（緑実線） — 試料・条件・結果などのデータロケーション
- **Proxy Place**（紫破線）— 他メソッドのPlaceを参照する外部参照ノード（EXT表示）

**マルチメソッド**

タブバーの `+ New Method` でメソッドを追加し、複数の実験・分析フローを独立して管理できます。タブバー右端の **🐦 Overview** タブで全メソッドの俯瞰図を表示できます（閲覧専用）。

---

### 📋 Templates タブ

Petri Net Design で定義した Place に紐づくデータテンプレート（スキーマ）を編集します。

- **+ Add Material Template** — 試料データのテンプレートを追加
- **+ Add Condition Template** — 条件・パラメータデータのテンプレートを追加
- **+ Add Result Template** — 結果・出力データのテンプレートを追加

各テンプレートには `property`（単一値）と `content`（リスト値）を追加でき、`propertyListType` によるネスト構造にも対応しています。他のメソッドのテンプレートをベースに継承（TemplateRef）したテンプレートは読み取り専用で表示されます。

---

### 📊 Data & Events タブ

Templates で定義したスキーマをもとに、実際の計測データ・イベントを入力します。

**左パネル（Source Templates）**

- **Data Templates** — 登録済みテンプレートの一覧。各カード下部の **+ Add Instance** ボタンでインスタンスを生成
- **Transitions (Events)** — ペトリネットで定義したトランジション（各メソッドに1つ）

**右パネル（Execution Timeline）**

生成されたインスタンス・イベントがタイムライン形式で表示されます。各インスタンスカードで以下の操作が可能です。

- テンプレート由来の Property / Content の値を直接編集
- インスタンス固有の Property / Content を追加
- 外部ファイル（測定データ等）の挿入情報（URI・UUID・Hash）を追加

---

## ファイル操作

| 操作 | 説明 |
|------|------|
| **Save Project (JSON)** | 編集中の状態をJSONファイルとして保存（再編集可能） |
| **Load Project (JSON)** | 保存したJSONファイルを読み込み、状態を復元 |
| **Export MaiML (XML)** | MaiML標準規格に準拠した `.maiml` XMLファイルを生成・ダウンロード |

---

## 動作環境

| ブラウザ | 対応状況 |
|---------|---------|
| Google Chrome（最新版） | ✅ 推奨 |
| Microsoft Edge（最新版） | ✅ 推奨 |
| Firefox | ⚠ 一部機能が動作しない場合があります |
| Safari | ⚠ 未確認 |

---

## 使用ライブラリ

- [Tailwind CSS](https://tailwindcss.com/)
- [Phosphor Icons](https://phosphoricons.com/)
- [SheetJS (xlsx)](https://sheetjs.com/)
- [CryptoJS](https://github.com/brix/crypto-js)

---

## ライセンス

Apache-2.0 license
