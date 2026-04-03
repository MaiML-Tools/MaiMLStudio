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
- **Excel インポート機能** — Excelファイルから定義を一括で読み込み
- **XML 暗号化機能** — AES-256-GCMによる機密データの秘匿
- **XMLデジタル署名機能** — ECDSA P-256による改ざん検知可能なMaiMLファイルの出力
- **ファイル連携機能** — 複数MaiMLファイルのハッシュベース連携（`<chain>` / `<parent>`）

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

#### 📊 Excel インポート機能

手動入力の代わりに、Excelファイルから一括で定義を読み込めます。対象のExcelファイルには、Placeと同じシート名が必要です。**Key列に`>`記号を使ってネスト構造を表現できます。**

| Element | Key | Type | Units | Description | Value |
|---------|-----|------|-------|-------------|-------|
| property | Temperature | propertyListType | | 温度設定 | |
| property | >Set_Temp | doubleType | °C | 設定温度 | 180 |
| property | >Control | propertyListType | | 制御パラメータ | |
| property | >>PID_P | doubleType | | P値 | 1.2 |

**ネスト構造のルール:** `>` = 1階層、`>>` = 2階層。親要素は必ず `propertyListType` である必要があります。

---

### 📊 Data & Events タブ

Templates で定義したスキーマをもとに、実際の計測データ・イベントを入力します。

**左パネル（Source Templates）**

- **Data Templates** — 登録済みテンプレートの一覧。各カード下部の **+ Add Instance** ボタンでインスタンスを生成
- **Transitions (Events)** — ペトリネットで定義したトランジション（各メソッドに1つ）。各トランジションカードで以下のフィールドを編集できます。

**Transition（eventLog）カードのフィールド**

| フィールド | 説明 |
|-----------|------|
| **lifecycle** | イベントのライフサイクル状態を選択（`started` / `completed` 等） |
| **timestamp** | ISO 8601形式（`YYYY-MM-DDThh:mm:ss.sssTZD`）で日時を入力。空白のままにするとエクスポート時に現在時刻が自動セットされる |
| **UUID** | イベントのUUID。手動入力またはボタンクリックで自動生成 |
| **creatorRef** | Document Metadata の **Creators** に登録済みのCreatorをドロップダウンで参照 |
| **ownerRef** | Document Metadata の **Owners** に登録済みのOwnerをドロップダウンで参照 |

`creatorRef` / `ownerRef` は任意項目です。選択すると、エクスポートされるMaiMLの `<eventLog>` 内に `<creatorRef>` / `<ownerRef>` 要素として出力されます。

**右パネル（Execution Timeline）**

生成されたインスタンス・イベントがタイムライン形式で表示されます。各インスタンスカードで以下の操作が可能です。

- テンプレート由来の Property / Content の値を直接編集
- インスタンス固有の Property / Content を追加
- 外部ファイル（測定データ等）の挿入情報（URI・UUID・Hash）を追加

**Results Groups（結果グループ）**

Data & Events タブの上部にあるタブバーで、インスタンスとイベントを複数のグループに分けて管理できます。

| 操作 | 方法 |
|------|------|
| グループの追加 | タブバー右端の **`+`** ボタンをクリック |
| グループ名の変更 | タブのペンアイコンをクリック |
| グループの削除 | タブの `×` ボタンをクリック（グループ内のインスタンス・イベントも同時に削除） |
| グループの切り替え | タブをクリック |

各メソッドは独立した Results Groups を持ちます。グループを切り替えると、タイムラインの表示内容もグループに対応した内容に切り替わります。

---

## ファイル操作

| 操作 | 説明 |
|------|------|
| **Save Project (JSON)** | 編集中の状態をJSONファイルとして保存（再編集可能） |
| **Load Project (JSON)** | 保存したJSONファイルを読み込み、状態を復元 |
| **Export MaiML (XML)** | MaiML標準規格に準拠した `.maiml` XMLファイルを生成・ダウンロード |
| **Export + Sign** | ECDSA P-256によるXMLデジタル署名付きの `.maiml` ファイルを生成・ダウンロード |

---

## 🔒 XML 暗号化機能（セクション 6）

MaiML規格（JIS K0200）に準拠したXML暗号化機能です。`property`要素や`content`要素を選択的に秘匿できます。

| 項目 | 説明 |
|------|------|
| **暗号化アルゴリズム** | AES-256-GCM（認証付き暗号化・改ざん検知内蔵） |
| **鍵導出** | PBKDF2-SHA256（反復回数 100,000 回） |
| **暗号化方式** | コンテンツ暗号化（Type=#Content）— 親要素タグ・属性はそのまま残し、子要素のみ暗号化 |
| **準拠仕様** | W3C XML Encryption 1.1 / JIS K0200 |

### 暗号化手��

1. **Templates タブ**または **Data タブ**で、暗号化したい property / content 行の右端にあるロックアイコンをクリック
2. アイコンが🔐（アンバー色）に変わると「暗号化対象」としてマークされます
3. 通常通り **Export MaiML (XML)** ボタンをクリック
4. 暗号化パスワードの設定ダイアログが表示されます。4文字以上のパスワードを2回入力
5. 出力されたMaiMLファイル内で、対象要素の子要素が `<xenc:EncryptedData>` に置き換えられます

⚠️ **注意:** パスワードを忘れると復号できません。安全な場所に保管してください。

---

## 🔗 ファイル連携機能 `<chain>`（セクション 7）

MaiML規格（JIS K0200）の `<chain>` 要素を使ったファイル連携機能です。複数のMaiMLファイルを関連付け、改ざん検知（ハッシュ値による整合性確認）を実現します。ブロックチェーンのように「このファイルは別のファイルの内容を受け継いでいる」という時系列の連鎖を記録できます。

| 項目 | 説明 |
|------|------|
| **ハッシュ方式** | SHA-256（ファイル全体のバイト列） |
| **出力位置** | `<document>` 要素内の `<date>` 要素の直後 |
| **key属性** | 規定値は `key="chain"` |

### 登録手順

1. **Document Metadata** タブの一番下にある **Chained Files** セクションを表示
2. **Add Chain** ボタンをクリックし、連携先の `.maiml` ファイルを選択
3. 選択したファイルの `//document/uuid` 値と SHA-256 ハッシュ値が自動的に取得・表示
4. 通常通り **Export MaiML** ボタンをクリックすると、`<chain>` 要素がMaiMLファイルに書き出されます

### 出力されるXML例

```xml
<document id="...">
  ...
  <date>2025-06-01T10:00:00+09:00</date>
  <chain id="chain_001" key="chain">
    <uuid>0bce8354-55f1-4047-b221-988e00c87c79</uuid>
    <hash method="SHA-256">B1sDhiK3...</hash>
  </chain>
</document>
```

---

## 🔗 改訂元ファイル連携機能 `<parent>`（セクション 7）

MaiML規格（JIS K0200）の `<parent>` 要素を使った改訂元ファイルの登録機能です。あるMaiMLファイルが別のMaiMLファイルを改訂・派生させたものであることを記録し、ハッシュ値による改ざん検知を実現します。

| 項目 | 説明 |
|------|------|
| **ハッシュ方式** | SHA-256（署名付きファイルはDigestValue、署名なしはファイル全体） |
| **key属性** | 固定値 `key="revised"` |
| **登録上限** | 1件のみ（1ファイルにつき改訂元は1つ） |

### 登録手順

1. **Document Metadata** タブ下部の **Parent File** セクションを表示
2. **Set Parent** ボタンをクリックし、改訂元の `.maiml` ファイルを選択
3. 選択したファイルの `//document/uuid` 値と SHA-256 ハッシュ値が自動的に取得・表示
4. 通常通り **Export MaiML** ボタンをクリックすると、`<parent>` 要素がMaiMLファイルに書き出されます

---

## ✍️ XMLデジタル署名機能（Export + Sign）

MaiML規格（JIS K0200）に準拠したXMLデジタル署名機能です。**Export + Sign** ボタンで出力したMaiMLファイルには、W3C XML Signature仕様に準拠した署名が埋め込まれ、ファイルの改ざんを検知できます。

| 項目 | 説明 |
|------|------|
| **署名アルゴリズム** | ECDSA P-256（ecdsa-sha256） |
| **署名方式** | Enveloped Signature（署名要素がファイル内に埋め込まれる） |
| **正規化** | Canonical XML 1.0（C14N） |
| **鍵管理** | 署名のたびに新しい鍵ペアを生成（公開鍵は署名内に埋め込み） |
| **準拠仕様** | W3C XML Signature 1.1 / JIS K0200 |

### 使用手順

1. ヘッダーの **Export + Sign** ボタン（緑色）をクリック
2. 通常の **Export MaiML** と同様に `.maiml` ファイルが生成・ダウンロードされます
3. 出力ファイル内の `<ds:Signature>` 要素に署名値・公開鍵が格納されています

> **注意:** 鍵ペアはエクスポートのたびに新規生成されます。署名検証が必要な場合は、出力ファイル内の公開鍵（`<ds:KeyValue>`）を使用してください。