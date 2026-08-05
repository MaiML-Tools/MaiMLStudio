# MaiML Studio

**MaiML Studio** is a web app for visually designing and exporting files that conform to [MaiML (Measurement Analysis Instrument Markup Language)](http://www.maiml.org/).

It runs simply by opening the HTML file in Chrome / Edge — no additional installation is required.

---

## Features

- **No installation required** — just double-click the `.html` file to launch
- **Runs entirely in the browser** — works on Windows / Mac / Linux with Chrome / Edge (latest version)
- **Multi-method support** — edit multiple experiment/analysis methods together on a unified canvas where each method is arranged as a vertically stacked lane
- **Project save/restore** — save and load in JSON format
- **MaiML import** — load an exported `.maiml` / `.xml` file and restore it to an editable state
- **MaiML export** — generate and download an XML file conforming to the MaiML standard
- **Dark/light mode** — automatically follows the OS setting as the initial value
- **Excel import feature** — bulk-load definitions from an Excel file
- **XML encryption feature** — protect confidential data with AES-256-GCM
- **XML digital signature feature** — output tamper-detectable MaiML files with ECDSA P-256
- **File linking feature** — hash-based linking between multiple MaiML files (`<chain>` / `<parent>`)

---

## How to Use

1. Download `MaiMLStudio.html`
2. Open it in Chrome or Edge
3. Edit each tab in turn to design your MaiML file
4. Use the **Export MaiML** button in the header to output a `.maiml` file

---

## Screen Layout

### 🗂 Document Metadata Tab

Edit the metadata for the entire MaiML file.

| Section | Content |
|-----------|------|
| **General Information** | Set the Document ID, UUID, Document Name, and Description |
| **Namespaces** | Register the namespace prefix and URI used for Property / Content keys. Registered prefixes appear as autocomplete suggestions when entering a Key |
| **Global Properties** | Add and edit properties (Key / Type / Value / Description) applied to the entire file, in a hierarchical structure. Supports nested structures via `propertyListType` |
| **Vendors** | Register manufacturer information for the instruments used |
| **Instruments** | Register the analytical instruments used |
| **Owners** | Register data owners |
| **Creators** | Register data creators. Can be linked to registered Vendors/Instruments |

- Key fields include input validation conforming to the QName format (`[a-zA-Z_][a-zA-Z0-9_.-]*` or `prefix:localName`)
- If a prohibited character is entered, the field is highlighted with a red warning border

---

### 🔷 Petri Net Design Tab

Design the experiment/analysis process as a Petri net. All methods are arranged vertically as **lanes** separated by dotted lines, on a **unified canvas** where you can edit multiple methods on a single screen by scrolling.

**Node Operations**

| Operation | Method |
|------|------|
| Add a Place (circle) | Select the Place button on the toolbar, then click the canvas (added to the lane you clicked) |
| Add a Transition (rectangle) | Select the Transition button on the toolbar, then click the canvas (added to the lane you clicked) |
| Connect an Arc (arrow) | Select the Arc button on the toolbar, then drag from the start node to the end node (within the same lane only) |
| Connect a TemplateRef (blue dashed line) | Select the TemplateRef button, then drag from a Place to a Place (can connect across lanes) |
| Connect an InstanceRef (green dashed line) | Select the InstanceRef button, then drag from a Place to a Place (can connect across lanes) |
| Move a node | In Select mode, drag the node (can only be moved within the same lane) |
| Delete a node | In Select mode, click the node → Delete Node in the right panel |
| Auto Layout | Use the Dagre icon on the toolbar to auto-arrange only the Places/Transitions of the currently selected method (the canvas view position does not change) |
| Pan the canvas | Drag the canvas background |
| Zoom | Mouse wheel |

**Unified Canvas (Lanes)**

- Each method is displayed as a **lane** with a dotted border, stacked vertically; adding/moving Places, Transitions, and Arcs is restricted to within a lane
- **Only TemplateRef / InstanceRef** can cross lane boundaries to connect directly to a Place in another method
- Drag the dotted line at the bottom of a lane to manually adjust its height (it also expands automatically as nodes are placed)
- Clicking a method tab puts the corresponding lane into a selected state (blue border)

**Multiple Methods**

Adding a method via `+ New Method` in the tab bar adds a new lane. The **🐦 Overview** tab at the right end of the tab bar shows a bird's-eye view of all methods (read-only).

---

### 📋 Templates Tab

A card is displayed for each Place defined in Petri Net Design, and the buttons at the top-right of each card let you add/edit the data templates (schemas) associated with that Place.

- **+ Add material** — add a template for sample data
- **+ Add condition** — add a template for condition/parameter data
- **+ Add result** — add a template for result/output data

Each template can have `property` (single value) and `content` (list value) added to it, and also supports nested structures via `propertyListType`. Templates inherited from another method's template (via TemplateRef) are displayed as read-only.

#### 📊 Excel Import Feature

Instead of manual entry, you can bulk-load definitions from an Excel file. The target Excel file must have a sheet name matching the Place. **You can express a nested structure in the Key column using the `>` symbol.**

| Element | Key | Type | Units | Description | Value |
|---------|-----|------|-------|-------------|-------|
| property | Temperature | propertyListType | | Temperature setting | |
| property | >Set_Temp | doubleType | °C | Set temperature | 180 |
| property | >Control | propertyListType | | Control parameters | |
| property | >>PID_P | doubleType | | P value | 1.2 |

**Nesting rules:** `>` = 1 level, `>>` = 2 levels. The parent element must always be `propertyListType`.

---

### 📊 Data & Events Tab

Enter the actual measurement data and events, based on the schemas defined in Templates.

**Left Panel (Source Templates)**

- **Data Templates** — a list of registered templates. The **+ Add Instance** button at the bottom of each card generates an instance
- **Transitions (Events)** — the transitions defined in the Petri net (one per method). The following fields can be edited in each transition card.

**Transition (eventLog) Card Fields**

| Field | Description |
|-----------|------|
| **lifecycle** | Select the lifecycle state of the event (`started` / `completed`, etc.) |
| **timestamp** | Enter the date/time in ISO 8601 format (`YYYY-MM-DDThh:mm:ss.sssTZD`). If left blank, the current time is automatically set at export |
| **UUID** | The UUID of the event. Enter manually or generate automatically with a button click |
| **creatorRef** | Reference a Creator already registered in Document Metadata's **Creators**, via a dropdown |
| **ownerRef** | Reference an Owner already registered in Document Metadata's **Owners**, via a dropdown |

`creatorRef` / `ownerRef` are optional fields. When selected, they are output as `<creatorRef>` / `<ownerRef>` elements within the exported MaiML's `<eventLog>`.

**Right Panel (Results Groups / Instance List)**

The generated instances and events are displayed as a list. The following operations are available on each instance card.

- Directly edit the values of Property / Content inherited from the template
- Add instance-specific Property / Content
- Add insertion information (URI, UUID, Hash) for external files (measurement data, etc.)

**Results Groups**

The tab bar at the top of the Data & Events tab lets you manage instances and events by dividing them into multiple groups.

| Operation | Method |
|------|------|
| Add a group | Click the **`+`** button at the right end of the tab bar |
| Rename a group | Click the pencil icon on the tab |
| Delete a group | Click the `×` button on the tab (also deletes the instances/events within the group) |
| Switch groups | Click the tab |

Each method has its own independent Results Groups. When you switch groups, the content displayed in the right panel switches to match the group.

---

## File Operations

| Operation | Description |
|------|------|
| **Save Project (JSON)** | Save the current editing state as a JSON file (can be re-edited later) |
| **Load Project (JSON)** | Load a saved JSON file and restore the state |
| **Import MaiML** | Load an exported `.maiml` / `.xml` file and restore it to an editable state (see the next section for details) |
| **Export MaiML** | Generate and download a `.maiml` XML file conforming to the MaiML standard |
| **Export MaiML(+Sign)** | Generate and download a `.maiml` file with an ECDSA P-256 XML digital signature |

---

## 📥 MaiML Import Feature (Import MaiML)

Loads an exported `.maiml` / `.xml` file and restores it to an editable state.

| Item | Content |
|------|------|
| **Supported scope** | Only MaiML files containing a single method (one `<method>`) are supported. Files containing multiple methods are not currently supported (an error is displayed on import) |
| **Template collection** | Templates located under any of the `protocol` / `method` / `program` hierarchy levels are collected |
| **Encrypted data** | If `<xenc:EncryptedData>` is detected, a password prompt is shown and all such elements are decrypted at once (up to 3 retries; the entire import is aborted on failure) |
| **Warning for unsupported structures** | If a structure with types other than `propertyListType` having child property/content elements is detected (valid per the schema but unsupported by Studio), a confirmation modal listing the affected items is shown, letting you choose to continue (discarding the affected child elements) or cancel |
| **Automatic revision history registration** | On successful import, the source file is automatically registered as `<parent key="revised">` (an automatic process with no confirmation, based on the operating rule that "import = revise"). If the source file already had a `<parent>`, that information is preserved nested as a child of the new parent, maintaining the chain of revisions |
| **Handling of signatures** | If the source file had a `<ds:Signature>`, it is discarded (since re-editing invalidates the signature) |

### Import Steps

1. Click the **Import MaiML** button in the header and select a `.maiml` / `.xml` file
2. If there is content currently being edited, a confirmation dialog asks whether it is okay to discard the current content
3. If there is encrypted data, a password input dialog is displayed
4. If an unsupported nested structure is detected, review the content and choose whether to continue or cancel
5. Once the import is complete, the content is restored across the Document Metadata, Petri Net Design, Templates, and Data & Events tabs

> **Note:** Directly overwriting the source file is not the intended workflow. The assumption is that you "save under a new name and trace the chain via revision history (`<parent>`)."

---

## 🔒 XML Encryption Feature (Section 6)

An XML encryption feature conforming to the MaiML standard (JIS K0200). `property` elements and `content` elements can be selectively protected.

| Item | Description |
|------|------|
| **Encryption algorithm** | AES-256-GCM (authenticated encryption with built-in tamper detection) |
| **Key derivation** | PBKDF2-SHA256 (100,000 iterations) |
| **Encryption method** | Content encryption (Type=#Content) — the parent element's tag and attributes are left as-is; only the child elements are encrypted |
| **Compliant specifications** | W3C XML Encryption 1.1 / JIS K0200 |

### Encryption Steps

1. On the **Templates tab** or **Data tab**, click the lock icon at the right end of the property/content row you want to encrypt
2. When the icon turns 🔐 (amber), it is marked as an "encryption target"
3. Click the **Export MaiML** button as usual
4. An encryption password setup dialog is displayed. Enter a password of 4 or more characters twice
5. In the exported MaiML file, the child elements of the target elements are replaced with `<xenc:EncryptedData>`

⚠️ **Note:** If you forget the password, decryption is not possible. Store it in a safe place.

---

## 🔗 File Linking Feature `<chain>` (Section 7)

A file linking feature using the `<chain>` element of the MaiML standard (JIS K0200). It links multiple MaiML files together and enables tamper detection (integrity verification via hash values). Like a blockchain, it can record a chronological chain indicating "this file inherits the content of another file."

| Item | Description |
|------|------|
| **Hash method** | SHA-256 (the byte sequence of the entire file) |
| **Output location** | Immediately after the `<date>` element within the `<document>` element |
| **key attribute** | The default value is `key="chain"` |

### Registration Steps

1. Show the **Chained Files** section at the bottom of the **Document Metadata** tab
2. Click the **Add Chain** button and select the `.maiml` file to link to
3. The `//document/uuid` value and SHA-256 hash value of the selected file are automatically retrieved and displayed
4. Click the **Export MaiML** button as usual, and the `<chain>` element is written to the MaiML file

### Example Output XML

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

## 🔗 Parent File Linking Feature `<parent>` (Section 7)

A registration feature for the revision-source file using the `<parent>` element of the MaiML standard (JIS K0200). It records that a given MaiML file is a revision/derivative of another MaiML file, and enables tamper detection via hash values.

| Item | Description |
|------|------|
| **Hash method** | SHA-256 (DigestValue for signed files; the entire file for unsigned files) |
| **key attribute** | Fixed value `key="revised"` |
| **Registration limit** | Only one entry (one revision source per file) |

### Registration Steps

1. Show the **Parent File** section at the bottom of the **Document Metadata** tab
2. Click the **Set Parent** button and select the revision-source `.maiml` file
3. The `//document/uuid` value and SHA-256 hash value of the selected file are automatically retrieved and displayed
4. Click the **Export MaiML** button as usual, and the `<parent>` element is written to the MaiML file

> **Note:** When a file is loaded using the **Import MaiML** feature, the source file is automatically registered as this `<parent>` (no manual **Set Parent** action is needed). The manual registration described here is for when you want to explicitly specify a revision source without using import.

---

## ✍️ XML Digital Signature Feature (Export MaiML(+Sign))

An XML digital signature feature conforming to the MaiML standard (JIS K0200). A MaiML file output via the **Export MaiML(+Sign)** button has a signature embedded that conforms to the W3C XML Signature specification, enabling detection of file tampering.

| Item | Description |
|------|------|
| **Signature algorithm** | ECDSA P-256 (ecdsa-sha256) |
| **Signature method** | Enveloped Signature (the signature element is embedded within the file) |
| **Canonicalization** | Canonical XML 1.0 (C14N) |
| **Key management** | A new key pair is generated for each signing operation (the public key is embedded within the signature) |
| **Compliant specifications** | W3C XML Signature 1.1 / JIS K0200 |

### Usage Steps

1. Click the **Export MaiML(+Sign)** button (green) in the header
2. The `.maiml` file is generated and downloaded, just as with the regular **Export MaiML**
3. The `<ds:Signature>` element in the output file contains the signature value and public key

> **Note:** A new key pair is generated each time you export. If you need to verify the signature, use the public key (`<ds:KeyValue>`) contained in the output file.
