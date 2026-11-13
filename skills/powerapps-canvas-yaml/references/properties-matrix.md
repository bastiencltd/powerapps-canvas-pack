---
ontology: true
type: reference
domain: powerapps-yaml
summary: Matrix of cross-cutting input and output properties of the 117 Power Apps controls (React 31, FluentV9 10, Classic 60, MixedReality 16)
tags: [powerapps, yaml, properties, matrix, reference]
status: active
related:
  - ../../01-controls-types
  - ../../02-properties-detail
---

# Cross-cutting properties matrix

Skill reference for Power Apps YAML: 17 cross-cutting input properties over the 117 controls, plus the frequent output properties. Everything is generated from `02-properties-detail.json` (the only data source), row order per `01-controls-types.json`. No property name is invented or reworded, `—` means null or absence, and default values are copied verbatim when the service provides them.

## 1. Frequency statistics (preamble)

Source: `02-properties-detail.json`. 117 controls (React 31, FluentV9 10, Classic 60, MixedReality 16), 3919 input entries for 623 distinct names, 2795 output entries for 468 distinct names.

Frequency of each input property over the 117 controls, top 25 :

| Rank | Property | Controls (out of 117) |
|---:|---|---:|
| 1 | `Visible` | 117 |
| 2 | `ContentLanguage` | 116 |
| 3 | `Width` | 116 |
| 4 | `X` | 114 |
| 5 | `Height` | 113 |
| 6 | `Y` | 113 |
| 7 | `DisplayMode` | 104 |
| 8 | `BorderStyle` | 89 |
| 9 | `BorderThickness` | 88 |
| 10 | `BorderColor` | 84 |
| 11 | `Tooltip` | 73 |
| 12 | `AccessibleLabel` | 67 |
| 13 | `Fill` | 62 |
| 14 | `OnSelect` | 61 |
| 15 | `TabIndex` | 57 |
| 16 | `Font` | 56 |
| 17 | `PaddingBottom` | 49 |
| 18 | `PaddingLeft` | 49 |
| 19 | `PaddingRight` | 49 |
| 20 | `PaddingTop` | 49 |
| 21 | `Color` | 48 |
| 22 | `FontWeight` | 47 |
| 23 | `Size` | 45 |
| 24 | `OnChange` | 43 |
| 25 | `Italic` | 41 |

**Retained columns : 17** (target ~15). Retained first on measured frequency (from `Visible` 117/117 to `OnChange` 43/117), then three semantically central properties, verified present in 02 despite a lower frequency : Items (27), Text (26), Default (25).

Instruction candidates rejected because absent from 02 (measured frequency zero, no name was imposed) : `OnVisible` = 0, `DisabledDisplayMode` = 0.

## 2. Presence matrix

Cell = `oui` if the property appears in the control's inputs, `—` otherwise. Rows in the canonical order of `01-controls-types.json`, grouped by family React > FluentV9 > Classic > MixedReality.

Legend : real types observed for each column, by family (`—` = property absent from the family) :

| Column | Property | /117 | React (types) | FluentV9 (types) | Classic (types) | MixedReality (types) |
|---|---|---:|---|---|---|---|
| 1 | `Visible` | 117 | Boolean | Boolean | Boolean | Boolean |
| 2 | `ContentLanguage` | 116 | Text | Text | Text | Text |
| 3 | `Width` | 116 | Number | Number | Number | Number |
| 4 | `X` | 114 | Number | Number | Number | Number |
| 5 | `Height` | 113 | Number | Number | Number | Number |
| 6 | `Y` | 113 | Number | Number | Number | Number |
| 7 | `DisplayMode` | 104 | Enum | Enum | Enum | Enum |
| 8 | `Tooltip` | 73 | Text | Text | Text | Text |
| 9 | `AccessibleLabel` | 67 | Text | Text | Text | — |
| 10 | `Fill` | 62 | Color | Color | Color | — |
| 11 | `OnSelect` | 61 | Boolean | Boolean | Boolean | Boolean |
| 12 | `Color` | 48 | Color | — | Color | — |
| 13 | `Size` | 45 | Enum, Number | — | Number | — |
| 14 | `OnChange` | 43 | Boolean | Boolean | Boolean | Boolean |
| 15 | `Items` | 27 | Table, Table (Attachment) | Table | Table | Table |
| 16 | `Text` | 26 | Text | Text | Text | Text |
| 17 | `Default` | 25 | Boolean, Number, Record, Text | — | Boolean, Number, Record, Text | Text |

### React (31 controls)

| Control | Visible | ContentLanguage | Width | X | Height | Y | DisplayMode | Tooltip | AccessibleLabel | Fill | OnSelect | Color | Size | OnChange | Items | Text | Default |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| DataWedgeScanner | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — | — | — |
| Geolocation | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — |
| ModernAttachments | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — |
| ModernAvatar | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — |
| ModernAvatarGroup | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | oui | oui | — | oui |
| ModernBadge | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — |
| ModernButton | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | — | — | oui | — |
| ModernCard | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | — | — | — | — | — | — |
| ModernCardGallery | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | oui | — | oui |
| ModernCheckbox | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | oui | — | — | — | oui |
| ModernCombobox | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | oui | — | — |
| ModernDataGrid | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | oui | — | — |
| ModernDataGridColumn | oui | oui | oui | — | — | — | — | — | oui | — | oui | — | — | — | — | oui | — |
| ModernDatePicker | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | — | — | — |
| ModernDropdown | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | oui | — | oui |
| ModernIcon | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| ModernInformationButton | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | oui | oui | — | — | — | — |
| ModernLink | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | — | — | oui | — |
| ModernNumberInput | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | — | — | oui |
| ModernPersona | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | — | — | — | — |
| ModernProgressBar | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — |
| ModernRadio | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | oui | — | oui |
| ModernRating | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | oui | — | — | oui |
| ModernSlider | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | oui | — | — | oui |
| ModernSpinner | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | oui | — | — | — | — |
| ModernTabList | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | oui | oui | oui | oui | — | oui |
| ModernText | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | oui | — | — | oui | — |
| ModernTextInput | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | oui | oui | — | — | oui |
| ModernToggle | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | oui | — | — | — | oui |
| NativePdfViewer | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — |
| NativePenInput | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — |

### FluentV9 (10 controls)

| Control | Visible | ContentLanguage | Width | X | Height | Y | DisplayMode | Tooltip | AccessibleLabel | Fill | OnSelect | Color | Size | OnChange | Items | Text | Default |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| Breadcrumb | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | — | — | oui | oui | — | — |
| Button | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | — | — | — | — | oui | — |
| Header | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | — | — | — | — | — |
| InteractiveText | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | oui | — |
| IoTDeviceManager | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | — | — |
| PowerVirtualAgentsChat | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | — | — |
| SpinButton | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | — | — | oui | — | — | — |
| StreamVideo | oui | — | oui | oui | oui | oui | — | — | — | — | — | — | — | — | — | — | — |
| Switch | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | — | — | oui | — | — | — |
| Toolbar | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | — | — | — | oui | — | — |

### Classic (60 controls)

| Control | Visible | ContentLanguage | Width | X | Height | Y | DisplayMode | Tooltip | AccessibleLabel | Fill | OnSelect | Color | Size | OnChange | Items | Text | Default |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| AddMedia | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — |
| Arrow | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| Attachments | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | — | oui | — | oui |
| Audio | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — |
| BarChart | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | oui | oui | — | oui | — | — |
| Barcode | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — |
| BarcodeScanner | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | oui | — | — | oui | — |
| Camera | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | — | — | — | — |
| Circle | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| Classic/Button | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui | — |
| Classic/CheckBox | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui | oui |
| Classic/ComboBox | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui |
| Classic/DatePicker | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — |
| Classic/DropDown | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui |
| Classic/Icon | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — |
| Classic/Radio | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui |
| Classic/Slider | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | oui | — | — | oui |
| Classic/StreamVideo | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | — | — | — |
| Classic/TextInput | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui |
| Classic/Toggle | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui |
| DataCard | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | — | — | — | — | — |
| DataGrid | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | oui | oui | — | oui | — | — |
| DataTable | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | oui | oui | — | oui | — | — |
| DataTableColumn | oui | oui | oui | — | — | — | — | — | — | — | oui | — | — | — | — | oui | — |
| EntityForm | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — | — |
| Export | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui | — |
| FluidGrid | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — | — |
| Form | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — | — |
| FormsPro | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — |
| FormViewer | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — | — |
| Gallery | oui | oui | — | — | — | — | oui | — | — | oui | — | — | — | — | — | — | oui |
| GroupContainer | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — | — |
| Hexagon | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| HtmlViewer | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | — | — |
| Image | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| Import | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui | — |
| Label | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui | — |
| Legend | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | oui | oui | — | oui | — | — |
| LineChart | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | oui | oui | — | oui | — | — |
| ListBox | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | oui |
| Lookup | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui |
| Microphone | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — |
| Octagon | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| PartialCircle | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| PDFViewer | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | — | — | — | — | — |
| PenInput | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — |
| Pentagon | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| PieChart | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | oui | oui | — | oui | — | — |
| PowerBI | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | — | — | — | — |
| Rating | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | oui |
| Rectangle | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| RichTextEditor | oui | oui | oui | oui | oui | oui | oui | — | oui | — | — | — | oui | oui | — | — | oui |
| SalesNavigatorAccount | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — | — | — | — |
| SalesNavigatorLead | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — | — | — | — |
| SpeechControl | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | — | oui | — | — | — | oui | — |
| Star | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| Timer | oui | oui | oui | oui | oui | oui | oui | oui | — | oui | oui | oui | oui | — | — | oui | — |
| Triangle | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — |
| TypedDataCard | oui | oui | oui | oui | — | — | — | — | — | oui | — | — | — | — | — | — | — |
| Video | oui | oui | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — |

### MixedReality (16 controls)

| Control | Visible | ContentLanguage | Width | X | Height | Y | DisplayMode | Tooltip | AccessibleLabel | Fill | OnSelect | Color | Size | OnChange | Items | Text | Default |
|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| AddressInput | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | — | oui |
| AnnotateInMR | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| BarcodeReader | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| BusinessCardReader | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| Chatbot | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | — | — | — | — |
| Copilot | oui | oui | oui | oui | oui | oui | oui | — | — | — | oui | — | — | oui | oui | — | — |
| CopilotAnswer | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | — | oui | — | — |
| FormProcessor | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| Map | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | oui | oui | — | — |
| MeasureInMR | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | oui | oui | oui | — |
| ObjectDetector | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| ReceiptProcessor | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| TextRecognizer | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |
| ViewIn3D | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | oui | oui | — | — |
| ViewInMR | oui | oui | oui | oui | oui | oui | oui | oui | — | — | oui | — | — | oui | oui | oui | — |
| ViewShapeInMR | oui | oui | oui | oui | oui | oui | oui | oui | — | — | — | — | — | oui | — | oui | — |

## 3. Exact type and literal defaults per retained property

Distributions expressed as control counts (a control is counted once per distinct type, total = frequency of column 2). Defaults copied verbatim when non-null ; `null` = default not provided by the service.

| Property | Type distribution (number of controls) | Dominant type | Most common literal defaults (number of controls) |
|---|---|---|---|
| `Visible` | Boolean (117) | `Boolean` (117 controls) | `true` (117) |
| `ContentLanguage` | Text (116) | `Text` (116 controls) | `""` (116) |
| `Width` | Number (116) | `Number` (116 controls) | `280` (13) ; `560` (13) ; `400` (11) ; `100` (9) ; `575` (9) ; `200` (7) ; +25 other distinct values |
| `X` | Number (114) | `Number` (114 controls) | `0` (114) |
| `Height` | Number (113) | `Number` (113 controls) | `70` (20) ; `100` (9) ; `64` (9) ; `400` (8) ; `500` (8) ; `300` (5) ; +36 other distinct values |
| `Y` | Number (113) | `Number` (113 controls) | `0` (113) |
| `DisplayMode` | Enum (104) | `Enum` (104 controls) | `DisplayMode.Edit` (104) |
| `Tooltip` | Text (73) | `Text` (73 controls) | `""` (73) |
| `AccessibleLabel` | Text (67) | `Text` (67 controls) | `""` (65) ; `"Progress bar"` (1) ; — (1) |
| `Fill` | Color (62) | `Color` (62 controls) | `App.Theme.Colors.Primary` (17) ; `RGBA(0, 0, 0, 0)` (17) ; — (16) ; `RGBA(255, 255, 255, 1)` (8) ; `Color.White` (2) ; `RGBA(0, 0, 0, 0.5)` (1) ; +1 other distinct values |
| `OnSelect` | Boolean (61) | `Boolean` (61 controls) | `false` (60) ; `Switch(Self.Selected.ItemKey,/* Action for Key 'new' */"new",Notify("New clicked"),/* Action for 'edit' */"edit",Notify("Edit clicked"),/* Action for 'delete' */ "delete",Notify("Delete clicked"),/* Action for 'info' */ "info",Notify("Info clicked"), /* Default action */  Notify("Unrecognized button clicked"))` (1) |
| `Color` | Color (48) | `Color` (48 controls) | `RGBA(0, 0, 0, 1)` (21) ; — (19) ; `App.Theme.Colors.PrimaryForeground` (6) ; `App.Theme.Colors.Darker30` (1) ; `Color.White` (1) |
| `Size` | Number (44) ; Enum (1) | `Number` (44 controls ; variety : 2 distinct types) | `24` (18) ; `21` (13) ; `26` (3) ; `0` (2) ; `10` (2) ; `11` (2) ; +5 other distinct values |
| `OnChange` | Boolean (43) | `Boolean` (43 controls) | `false` (43) |
| `Items` | Table (26) ; Table (Attachment) (1) | `Table` (26 controls ; variety : 2 distinct types) | — (11) ; `ComboBoxSample` (3) ; `RadioSample` (2) ; `["Item 1", "Item 2", "Item 3"]` (1) ; `ColumnChartSample` (1) ; `DropDownSample` (1) ; +8 other distinct values |
| `Text` | Text (26) | `Text` (26 controls) | `"Button"` (3) ; `"Text"` (3) ; — (3) ; `"+ New image"` (1) ; `"Analyze"` (1) ; `"Barcode Reader"` (1) ; +14 other distinct values |
| `Default` | Record (9) ; Text (7) ; Number (5) ; Boolean (4) | `Record` (9 controls ; variety : 4 distinct types) | — (9) ; `false` (4) ; `""` (3) ; `"1"` (2) ; `3` (2) ; `50` (2) ; +3 other distinct values |

## 4. Cross-cutting output properties

Most frequent outputs, all families combined (top 15) :

| Rank | Property | Controls (out of 117) | Also input of the same control |
|---:|---|---:|---:|
| 1 | `ContentLanguage` | 117 | 116 |
| 2 | `Visible` | 117 | 117 |
| 3 | `Width` | 117 | 116 |
| 4 | `Height` | 115 | 113 |
| 5 | `X` | 115 | 114 |
| 6 | `Y` | 115 | 113 |
| 7 | `DisplayMode` | 107 | 104 |
| 8 | `BorderColor` | 75 | 74 |
| 9 | `BorderStyle` | 74 | 74 |
| 10 | `BorderThickness` | 74 | 73 |
| 11 | `AccessibleLabel` | 62 | 61 |
| 12 | `Fill` | 61 | 61 |
| 13 | `Tooltip` | 57 | 57 |
| 14 | `Color` | 47 | 47 |
| 15 | `Font` | 45 | 45 |

So-called "clean" outputs : absent from the same control's inputs (201 distinct names), top 15. These are the values the control reports beyond its adjustable properties :

| Rank | Property | Controls (out of 117) | Types observed in output |
|---:|---|---:|---|
| 1 | `Selected` | 27 | Record |
| 2 | `SelectedItems` | 15 | Table |
| 3 | `Value` | 9 | Boolean, Number, Text |
| 4 | `Error` | 5 | Text |
| 5 | `OriginalImage` | 5 | Text |
| 6 | `ShouldReset` | 5 | Boolean |
| 7 | `Photos` | 4 | Record |
| 8 | `SelectedText` | 4 | Record |
| 9 | `DisplayMode` | 3 | Enum |
| 10 | `Results` | 3 | Record |
| 11 | `SearchText` | 3 | Text |
| 12 | `SeriesLabels` | 3 | Table |
| 13 | `Text` | 3 | Text |
| 14 | `Valid` | 3 | Boolean |
| 15 | `Attachments` | 2 | Table, Table (Attachment) |

Check of the names advanced in the instruction, computed on 02 without rewording : `Text` = 17 control(s) in output / 26 in input ; `Value` = 11 / 3 ; `Selected` = 27 / 0. The compound names that really exist (among the clean outputs above) are the ones copied into the tables.

## 5. Methodological note : the `required` field is never populated

Across the 3919 input entries analyzed in 02 : 3915 carry `required: null`, 0 carry `required: false`, 4 carry `required: true`. `required: null` means obligation unknown to the `describe_control` service, never "optional" ; do not convert it to `false` in a generated YAML.

Only 4 genuinely mandatory inputs in the whole dataset (exact copy family / control . property) :

- Classic / Classic/Icon . Icon
- Classic / DataCard . X
- Classic / DataCard . Y
- Classic / TypedDataCard . X

Reading convention for this entire document : `—` means null / absence ; property names, types and defaults are copied verbatim from `02-properties-detail.json`, without rewording.
