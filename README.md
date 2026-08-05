<p align="right">
  <a href="https://github.com/MaiML-Tools/MaiMLStudio/blob/main/README-en.md"><img alt="English README" src="https://img.shields.io/badge/📖_README-English-2b6cb0?style=for-the-badge"></a>
</p>

# MaiML Studio

**MaiML Studio** は、 [MaiML（Measurement Analysis Instrument Markup Language）](http://www.maiml.org/) に準拠したファイルをビジュアルにデザイン・エクスポートするWebアプリです。

HTMLファイルを Chrome / Edge で開くだけで動作し、追加のインストールは一切不要です。

---

## 特徴

- **インストール不要** — `.html` ファイルをダブルクリックするだけで起動
- **ブラウザ完結** — Chrome / Edge（最新版）があれば Windows / Mac / Linux で動作
- **マルチメソッド対応** — 複数の実験・分析メソッドを、縦積みレーンで並べた統合キャンバス上で一括編集
- **プロジェクト保存/復元** — JSON形式での保存・読み込みに対応
- **MaiML インポート** — エクスポート済みの `.maiml` / `.xml` ファイルを読み込み、再編集可能な状態に復元
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
4. ヘッダーの **Export MaiML** ボタンで `.maiml` ファイルを出力

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

実験・分析プロセスをペトリネットとして設計します。全メソッドが点線区切りの**レーン**として縦に並ぶ**統合キャンバス**で、1画面上でスクロールしながら複数メソッドを編集できます。

**ノード操作**

| 操作 | 方法 |
|------|------|
| Place（円）の追加 | ツールバーの Place ボタン選択後、キャンバスをクリック（クリックしたレーンに追加） |
| Transition（矩形）の追加 | ツールバーの Transition ボタン選択後、キャンバスをクリック（クリックしたレーンに追加） |
| Arc（矢印）の接続 | ツールバーの Arc ボタン選択後、始点ノードから終点ノードへドラッグ（同一レーン内のみ） |
| TemplateRef（青破線）の接続 | TemplateRef ボタン選択後、Place から Place へドラッグ（レーンを跨いで接続可） |
| InstanceRef（緑破線）の接続 | InstanceRef ボタン選択後、Place から Place へドラッグ（レーンを跨いで接続可） |
| ノードの移動 | Select モードでノードをドラッグ（同じレーン内でのみ移動） |
| ノードの削除 | Select モードでノードをクリック → 右パネルの Delete Node |
| Auto Layout | ツールバーのDagreアイコンで、選択中メソッドのPlace/Transitionのみを自動整列（キャンバスの表示位置は変わらない） |
| キャンバスのパン | キャンバス背景をドラッグ |
| ズーム | マウスホイール |

**統合キャンバス（レーン）**

- 各メソッドは点線境界の**レーン**として縦に並んで表示され、Place / Transition / Arc の追加・移動はレーン内に限定されます
- **TemplateRef / InstanceRef のみレーンの境界を跨いで**他メソッドのPlaceと直接接続できます
- レーン下端の点線をドラッグすると高さを手動で調整できます（ノードを配置していくと自動でも拡張されます）
- メソッドタブをクリックすると該当レーンが選択状態（青枠）になります

**マルチメソッド**

タブバーの `+ New Method` でメソッドを追加すると新しいレーンが追加されます。タブバー右端の **🐦 Overview** タブで全メソッドの俯瞰図を表示できます（閲覧専用）。

---

### 📋 Templates タブ

Petri Net Design で定義した Place ごとにカードが表示され、各カードの右上にあるボタンでその Place に紐づくデータテンプレート（スキーマ）を追加・編集します。

- **+ Add material** — 試料データのテンプレートを追加
- **+ Add condition** — 条件・パラメータデータのテンプレートを追加
- **+ Add result** — 結果・出力データのテンプレートを追加

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

**右パネル（Results Groups / インスタンス一覧）**

生成されたインスタンス・イベントが一覧形式で表示されます。各インスタンスカードで以下の操作が可能です。

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

各メソッドは独立した Results Groups を持ちます。グループを切り替えると、右パネルの表示内容もグループに対応した内容に切り替わります。

---

## ファイル操作

| 操作 | 説明 |
|------|------|
| **Save Project (JSON)** | 編集中の状態をJSONファイルとして保存（再編集可能） |
| **Load Project (JSON)** | 保存したJSONファイルを読み込み、状態を復元 |
| **Import MaiML** | エクスポート済みの `.maiml` / `.xml` ファイルを読み込み、再編集可能な状態に復元（詳細は次項） |
| **Export MaiML** | MaiML標準規格に準拠した `.maiml` XMLファイルを生成・ダウンロード |
| **Export MaiML(+Sign)** | ECDSA P-256によるXMLデジタル署名付きの `.maiml` ファイルを生成・ダウンロード |

---

## 📥 MaiML インポート機能 (Import MaiML)

エクスポート済みの `.maiml` / `.xml` ファイルを読み込み、再編集可能な状態に復元します。

| 項目 | 内容 |
|------|------|
| **対応範囲** | 単一メソッド（`<method>` 1件）のMaiMLファイルのみ対応。複数メソッドを含むファイルは現時点では非対応（インポート時にエラー表示） |
| **テンプレート収集** | `protocol` / `method` / `program` のいずれの階層下にあるテンプレートも収集 |
| **暗号化データ** | `<xenc:EncryptedData>` を検出した場合はパスワード入力を求めて一括復号（最大3回まで再試行、失敗時はインポート全体を中止） |
| **未対応構造の警告** | `propertyListType` 以外の型が子 property / content を持つ構造（スキーマ上は合法だがStudio未対応）を検出した場合、対象一覧を表示する確認モーダルが表示され、続行（該当の子要素を破棄）または中止を選択できる |
| **改訂履歴の自動登録** | インポートに成功すると、インポート元ファイルは自動的に `<parent key="revised">` として登録されます（確認なしの自動処理。「インポート＝改編する」という運用ルールに基づく）。インポート元ファイルが既に `<parent>` を持っていた場合、その情報は新しい parent の子要素として入れ子で保持され、改訂の連鎖が維持されます |
| **署名の扱い** | インポート元に `<ds:Signature>` があった場合は破棄されます（再編集により署名は無効になるため） |

### インポート手順

1. ヘッダーの **Import MaiML** ボタンをクリックし、`.maiml` / `.xml` ファイルを選択
2. 編集中の内容がある場合は、現在の内容を破棄してよいかの確認ダイアログが表示されます
3. 暗号化データがある場合はパスワード入力ダイアログが表示されます
4. 未対応のネスト構造が検出された場合は内容を確認し、続行するか中止するかを選択します
5. インポートが完了すると、Document Metadata・Petri Net Design・Templates・Data & Events の各タブに内容が復元されます

> **注意:** インポート元ファイルを直接上書きすることは想定されていません。「別名で保存し、改訂履歴（`<parent>`）で連鎖をたどる」という運用を前提としています。

---

## 🔒 XML 暗号化機能（セクション 6）

MaiML規格（JIS K0200）に準拠したXML暗号化機能です。`property`要素や`content`要素を選択的に秘匿できます。

| 項目 | 説明 |
|------|------|
| **暗号化アルゴリズム** | AES-256-GCM（認証付き暗号化・改ざん検知内蔵） |
| **鍵導出** | PBKDF2-SHA256（反復回数 100,000 回） |
| **暗号化方式** | コンテンツ暗号化（Type=#Content）— 親要素タグ・属性はそのまま残し、子要素のみ暗号化 |
| **準拠仕様** | W3C XML Encryption 1.1 / JIS K0200 |

### 暗号化手順

1. **Templates タブ**または **Data タブ**で、暗号化したい property / content 行の右端にあるロックアイコンをクリック
2. アイコンが🔐（アンバー色）に変わると「暗号化対象」としてマークされます
3. 通常通り **Export MaiML** ボタンをクリック
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

> **補足:** **Import MaiML** 機能を使ってファイルを読み込んだ場合は、インポート元ファイルがこの `<parent>` として自動的に登録されます（手動での **Set Parent** 操作は不要）。ここで説明する手動登録は、インポートを使わずに改訂元を明示したい場合に利用します。

---

## ✍️ XMLデジタル署名機能（Export MaiML(+Sign)）

MaiML規格（JIS K0200）に準拠したXMLデジタル署名機能です。**Export MaiML(+Sign)** ボタンで出力したMaiMLファイルには、W3C XML Signature仕様に準拠した署名が埋め込まれ、ファイルの改ざんを検知できます。

| 項目 | 説明 |
|------|------|
| **署名アルゴリズム** | ECDSA P-256（ecdsa-sha256） |
| **署名方式** | Enveloped Signature（署名要素がファイル内に埋め込まれる） |
| **正規化** | Canonical XML 1.0（C14N） |
| **鍵管理** | 署名のたびに新しい鍵ペアを生成（公開鍵は署名内に埋め込み） |
| **準拠仕様** | W3C XML Signature 1.1 / JIS K0200 |

### 使用手順

1. ヘッダーの **Export MaiML(+Sign)** ボタン（緑色）をクリック
2. 通常の **Export MaiML** と同様に `.maiml` ファイルが生成・ダウンロードされます
3. 出力ファイル内の `<ds:Signature>` 要素に署名値・公開鍵が格納されています

> **注意:** 鍵ペアはエクスポートのたびに新規生成されます。署名検証が必要な場合は、出力ファイル内の公開鍵（`<ds:KeyValue>`）を使用してください。