---
ontology: true
type: best-practice
domain: powerapps-canvas
status: active
summary: Canvas .pa.yaml validation rules verified by the machine : 6 codes with raw diagnostics, 13 structure rules, 6 naming, 5 enum identity patterns, 9 hierarchy, 21 pitfalls of which 7 confirmed
tags: [power-apps, pa-yaml, validation, compile-canvas, diagnostics, power-fx]
related:
  - connectors-catalog
---

# .pa.yaml validation rules (machine-verified)

Source : `04-validation-rules.json` (extraction 2026-11-13, agent-4, status complete). MCP session : app `app_id of the audited session`, env `environment_id of the audited session`. Probes : `_scratch\agent-4\`. Versions bound in this session : **Label@2.5.1**, **TextInput@0.0.54** (accepted without warning = current version). The `hermes:` and `github:` prefixes of source paths designate the public upstream repositories of the references.

## MACHINE CORRECTIONS to memorize first (they replace the PROMPT-MASTER legend)

These 4 probe results **cancel or correct** the most common PROMPT-MASTER legend :

1. **GroupContainer + `Variant: GridLayout` = PASSED** (probe p4-gridlayout). The three variants `AutoLayout` / `GridLayout` / `ManualLayout` are ACCEPTED in MCP authoring, the `Control: GroupContainer` keyword **without @version suffix** is also accepted. The "AutoLayout/ManualLayout only" list in pa-yaml-rules.md is incomplete ; the YamlSyntax.md table is right.
2. **A fictive version HIGHER than the current one (`TextInput@99.99.99`) = PASSES with no diagnostic at all** (probe p3-bad-version). The real PA2105 trigger is NOT "any wrong @version suffix" : it is a **deviation from the current version bound in the session** — an **older** version (`Label@1.0.0` vs current `2.5.1` → warning) and/or **version conflict between instances of the same type** (`Label@1.0.0` + `Label@2.5.1` in the same screen → error, probe p3b-old-version-conflict).
3. **`Control: Screen` blocks, but the real diagnostics are not the PA2101 label** : at the file root `YamlInvalidSyntax` (probe p1) ; as a child of a valid `Screens:` `Unknown control type 'Screen'` + warning `Early Preview code detected` (probe p1b). **PA1001/PA2101/PA2105/PA2108 are Power Apps Studio / paste-code labels**, not codes emitted by compile_canvas : the MCP output only shows the diagnostic TEXT, except parser codes named literally (`YamlInvalidSyntax`, `DuplicateNameInSequence`).
4. **The machine "do not use" list contains only `Button`, `DataGrid`, `DataTable`.** The "NEVER Classic/Icon" rule is **documentation only** (ui-gen skill convention : icons = inline SVG via Image@2.2.3) — `Classic/Icon@2.5.0` remains a valid control for the compiler.

## The 6 diagnostic codes and their RAW diagnostics

### PA1001 — PaYaml parse failure (internal : YamlInvalidSyntax)

Triggers : property value without `=` prefix ; unquoted plain scalar containing `': '` ; key indented at the wrong level ; Children entry without `- ` indentation ; key without value ; duplicate key in a Properties block ; badly indented multiline string ; tabulation ; file root without a valid module keyword. **A file that does not parse surfaces NO other diagnostic.**
Fix : fix the parse first (add `=`, quote, align, remove tabs/duplicates), use the message's Line/Col, recompile.

```
Screen1.pa.yaml: error: An error occurred while parsing PaYaml. Error code: YamlInvalidSyntax; Reason: No node deserializer was able to deserialize the node into type Microsoft.PowerPlatform.PowerApps.Persistence.PaYaml.Models.SchemaV3.PaModule, Microsoft.PowerPlatform.PowerApps.Persistence, Version=0.9.3461.1, Culture=neutral, PublicKeyToken=31bf3856ad364e35
```

(probe p1-screen-control, root `- screenRoot:` without the Screens keyword. The literal `PA1001` label with exactly this text is documented in hermes_history line 127.)

### PA2101 — `Control: Screen`

Trigger : a screen treated as a control (child of `Children:`). Fix : always the top-level `Screens:` format with the screen name as a mapping, never `Control: Screen` nor a children list at the root.

```
Screen1.pa.yaml(6,9): error: Unknown control type 'Screen'. | Screen1.pa.yaml(6,9): warning: Early Preview code detected for control type 'Screen'.
```

(probe p1b-screen-child ; the literal PA2101 label does not appear in the MCP output.)

### PA2105 — obsolete control version / version conflict

Real trigger (probe p3b) : an instance of a type referenced with a version **older than the current version bound** + **two different versions for the same type in the app**. Documented as an auto-upgrade warning in pa-yaml-rules.md ; in MCP it is error + warning. NUANCE : made-up higher version (99.99.99) = PASSES (see Machine Correction n°2). Fix : copy the exact `Control:` value from describe_control for EVERY instance of the same type ; "VERSION SELF-HEALING" rule : as soon as a PA2105 appears, update the version everywhere.

```
Screen1.pa.yaml(14,9): error: Another instance of control type 'Label' has already been referenced using a different version '2.5.1'. All control instances for the same type must currently reference the same version. | Screen1.pa.yaml(14,9): warning: Control type 'Label@1.0.0' has a version that is older than the current version of '2.5.1'. Using the current version, which may produce errors.
```

(probe p3b-old-version-conflict. Counter-probe p3-bad-version : `Control: TextInput@99.99.99` -> `Validation PASSED` with no diagnostic.)

### PA2108 — property nonexistent for the exact type

Trigger : properties not listed by describe_control for this exact type (the PA2108 table in controls-reference.md documents « Using any property in this table causes a PA2108 Unknown property error »). Fix : never transfer a property by analogy between controls (the Classic and Modern families diverge).

```
Screen1.pa.yaml(9,13): error: Unknown property 'Default' for control type 'TextInput@0.0.54'.
```

(probe p2-bad-property ; the literal PA2108 label does not appear in the MCP output, the `Unknown property` text matches the documented wording.)

### Missing Variant (mandatory keyword)

Trigger : a control whose `Variant` is mandatory (e.g. GroupContainer) without Variant or empty. Fix : `Variant: AutoLayout` | `GridLayout` | `ManualLayout` (all three machine ACCEPTED).

```
Screen1.pa.yaml(6,9): error: The keyword 'Variant' is required but is missing or empty.
```

(probe p4b-container-no-variant.)

### DuplicateNameInSequence

Trigger : the same control name twice in a `Children:` sequence. Parser code named **LITERALLY** by the MCP output (unlike the PA21xx). Fix : unique name in the sequence and app-wide ; rename the duplicate.

```
Screen1.pa.yaml(6,7): error: An error occurred while parsing PaYaml. Error code: DuplicateNameInSequence; Reason: Exception during deserialization
```

(probe p5-duplicate-name.)

## 13 file structure rules

1. Every `.pa.yaml` file contains **a single top-level object**, introduced by `App`, `EditorState`, `ComponentDefinitions` or `Screens`. Mandatory names : `App.pa.yaml`, `_EditorState.pa.yaml`, `<NomComposant>.pa.yaml`, `<NomEcran>.pa.yaml`. *(YamlSyntax.md, File structure)*
2. A root that is not one of these keywords (e.g. a `- screenRoot:` sequence at the root) fails : `YamlInvalidSyntax / No node deserializer ... PaModule`. A children list is NOT a valid root. *(probe p1 + YamlSyntax.md)*
3. Valid screen format : `Screens:` then the screen name as a mapping, `Properties:` (optional, mapping) + `Children:` (named sequence, each entry `- Nom:` with mandatory `Control:` and `Variant:` per type). **The screen has no `Control` keyword at its own level.** *(probe p0-base PASSED)*
4. `App.pa.yaml` : keyword `App:` + `Properties:` (mapping). Documented examples : `StartScreen: =Screen1` in CREATE ; `Formulas` (named formulas) in Properties. *(YamlSyntax.md + probe p0-base)*
5. `_EditorState.pa.yaml` : `EditorState:` with the `ScreensOrder:` and `ComponentDefinitionsOrder:` lists. Absent names are placed after ; list everything for a deterministic order ; keep existing names. *(YamlSyntax.md)*
6. The minimal valid workspace (App + Screen1 + _EditorState, one child Label) **compiles and PASSES**. An app is never empty : in CREATE the first screen overwrites `Screen1.pa.yaml` (never an empty Screen1 next to a Home.pa.yaml). *(probe p0-base + YamlSyntax "Reuse Screen1")*
7. **Every property value starts with `=`** (structural keywords Control/Variant/Children excluded). Without `=` : `YamlInvalidSyntax; Reason: Power Fx expressions must start with '='` for the whole file. *(YamlSyntax.md + pa-yaml-rules CRITICAL FORMAT RULES + QACHK-MISSING-FORMULA-PREFIX)*
8. Multiline formula : block scalar `|-`, the `=` goes on the **first line of content** (never on the `|-` line), semicolons between instructions. *(YamlSyntax.md)*
9. Any plain scalar containing `': '` must be quoted or in a `|-` block, otherwise `YamlInvalidSyntax; Reason: While scanning a plain scalar value, found invalid mapping` — **the diagnostic names neither control nor line**. The hermes adds `#` as dangerous (not probed). *(YamlSyntax.md + pa-yaml-rules PA1001)*
10. A **Power Fx record literal must be quoted at the YAML level** : `Default: ={Value: "Tab1"}` fails ; correct `Default: '={Value: "Tab1"}'`. Frequent case : ModernTabList.Default. *(YamlSyntax.md)*
11. Creation keywords : `Control` (mandatory = the ControlTypeId from describe_control), `ComponentName` (CanvasComponent/CodeComponent), `ComponentLibraryUniqueName` (Dataverse library components), `Variant` and `Layout` (optional, mandatory or forbidden **depending on the type**). Only the describe_control output is authoritative ; list_controls names are not the YAML contract. *(YamlSyntax.md + ControlGuide Trap 3 + QACHK-CONTROL-CREATION-KEYWORDS / QACHK-MISSING-VARIANT)*
12. Keywords forbidden per family : `CanvasComponent` → Variant and Layout forbidden ; `CodeComponent` → ComponentLibraryUniqueName, Variant and Layout forbidden. *(YamlSyntax.md)*
13. `App.StartScreen` designates the arrival screen (`=Screen1` in CREATE). Never `Navigate()` from `App.OnStart` nor the start screen's OnVisible : the compiler reports `Navigate is not permitted in OnStart`. *(YamlSyntax.md)*

## 6 naming rules

1. Control names **unique app-wide** ; duplicate within a sequence = `DuplicateNameInSequence` (probe p5). *(canvas-app SKILL.md 84 + CreateConformance level 3)*
2. Convention : control-type abbreviation + assigned screen prefix (e.g. `conDiscNavBar`), a prefix globally unique per builder. *(SKILL.md 84 + QACHK-DUPLICATE-PROPERTY-KEY)*
3. In Modify/EDIT **keep existing names** : a rename breaks consumers elsewhere. *(SKILL.md 84 Preservation)*
4. Never duplicate : mapping keys in a Properties block, children names in a Children, scaffold roots. *(QACHK-DUPLICATE-PROPERTY-KEY + YamlSyntax « Duplicate name X used at ... »)*
5. The screen file name carries the screen name ; `_EditorState.ScreensOrder` lists the exact names ; **the order in Children IS significant** (Z-index / layout-container order). *(YamlSyntax.md)*
6. Active-item global variables : prefix `Current` + semantic name (`CurrentNavID`, `CurrentTabID`), never `var` — a ui-gen skill convention, **not a machine rule**. *(SKILL.md 248)*

## 5 enum identity patterns (syntax)

| Pattern | When | Exact example |
|---|---|---|
| `='EnumIdentity'.Member` | **dotted / non-simple** enum identity : quote the FULL NAME in apostrophes then `.Member` | `Appearance: ='ButtonCanvas.Appearance'.Outline` (never `=ButtonCanvas.Appearance.Primary` without apostrophes) |
| `=SimpleEnumName.Value` | simple enum name (no dot/space) : no quoting. **NEVER build the name** : copy it from the `Enum name:` line of describe_control (4 different enums for Appearance depending on the control) | `=Appearance.Underline` ; `=BadgeAppearance.Tint` ; `=ButtonAppearance.Primary` if that exact name is returned |
| `=EnumName.'Member'` | non-simple member (leading digit, space, special characters) : quote the MEMBER | `=DecimalPrecision.'1'` (never bare `.1` — produces the « Expected operator » + « Expected an operand » pair without naming the enum) ; `=Font.'Open Sans'` |
| `=EnumName.Member` | a member is NEVER a bare identifier | `ThemeColor: =Subtle` fails « Name isn't recognized » — fix the qualifier, never delete it |
| `='Option Set'.'Value'` | Dataverse option sets : escape NAME and VALUE if spaces/special characters/leading digit | `'Account Status'.Active` ; `'Status (Assignments)'.Active` |

## 9 hierarchy rules

1. `Children` is always a **named-object-sequence** (`- Nom:` per item) ; the order IS significant (Z-index or layout order depending on the parent).
2. A control has Children **only if its type accepts children** ; respect the describe_control restrictions : immediate children vs descendants ; slots (ModernCard) are NOT child types ; an empty restrictions block does not mean universal nesting.
3. **A screen is never a child control** : `Screens:` format only (PA2101 / `Unknown control type 'Screen'`, probe p1b). Screen level = `Properties:`/`Children:` with no Control keyword.
4. Responsive screen = exactly **ONE full-viewport AutoLayout root** (`Width: =Parent.Width`, `Height: =Parent.Height`, `LayoutMinWidth/LayoutMinHeight = 0`) ; every conditional surface UNDER this root. *(QACHK-ROOT-CONTAINMENT)*
5. GroupContainer : allowed variants AutoLayout / GridLayout / ManualLayout — **all three PASS on the machine** (p4) ; missing Variant = error (p4b) ; the bare form without @version is accepted.
6. Gallery : variants Vertical / Horizontal / VariableHeight (+ layout presets) ; child templates define the row ; explicit `TemplateSize`/`TemplatePadding`.
7. `DisplayMode` **inherits from the parent** : an ancestor in View disables interactive descendants ; prefer `Selectable: =false` on a Gallery over View. *(ControlGuide 395-398)*
8. AutoLayout containers **cannot stack** children ; for Z-index/overlap switch to ManualLayout + X/Y ; no flex-wrap without explicit LayoutWrap. *(canvas-apps-limitations 116-141)*
9. Components : local definitions in `Components/*.pa.yaml` (ComponentDefinitions) ; instances `Control: CanvasComponent` + `ComponentName` or `CodeComponent` + `ComponentName` ; never Variant/Layout on these families.

## 21 pitfalls — machine status : 7 CONFIRMED, 14 DOCUMENTED ONLY

### CONFIRMED by probe (7)

| Pitfall | Machine diagnostic / proof |
|---|---|
| `TextInput@0.0.54` has NO `Default` property | `Unknown property 'Default' for control type 'TextInput@0.0.54'.` (p2) ; documented fix : omit it or use `Classic/TextInput@2.3.2` |
| GroupContainer **without** `Variant` : MANDATORY keyword | `The keyword 'Variant' is required but is missing or empty.` (p4b) |
| GroupContainer + `Variant: GridLayout` VALID (the 3 variants + bare form ok) | `Validation PASSED` (p4) ; contradicts pa-yaml-rules.l2113 |
| Never add/remove an `@version` suffix yourself ; instances of the same type at the SAME version ; the bare form is accepted | exact p3b message ; empirical note p4/p4b |
| Made-up HIGHER version (TextInput@99.99.99) = PASSES, PA2105 only fires on a deviation from the current bound version | `Validation PASSED` (p3) vs p3b |
| Duplicate name in a Children | `Error code: DuplicateNameInSequence` (p5) ; app-wide duplicates = tier 3 |
| An unparsed file (YamlInvalidSyntax) **masks all other diagnostics** ; bare `': '` column message with neither control nor line | behavior observed in probes (CreateConformance tiering) |

### DOCUMENTED ONLY, not probed (14)

8. `ComboBox@0.0.51` has no `SearchField` (built-in search) ; `Classic/ComboBox@2.4.0` : singular `SearchField` = PA2108, the correct name is `SearchFields` (plural).
9. `Classic/ComboBox@2.4.0` : deprecated `Default` → `DefaultSelectedItems` ; `NoSelectionText` does not exist → `InputTextPlaceholder`.
10. `Radio@0.0.25` has no `Default` ; use `Classic/Radio@2.3.0` which supports it.
11. `DatePicker@0.0.46` has no `Default` ; `Classic/DatePicker@2.6.0` uses `DefaultDate` and not `IsReadOnly` (→ `IsEditable: =false`).
12. GroupContainer has **no OnSelect** (not clickable), no HoverFill, no focus properties (workaround : transparent Classic/Button overlay).
13. `Rectangle@2.3.0` and `Label@2.5.1` have NO radius property (RadiusTopLeft/Right/BottomLeft/Right = PA2108) → **Radius Wrapper Pattern** : wrap in a GroupContainer carrying the radius.
14. `Button@0.0.45` (Modern/Fluent) has no Fill/Color/BorderColor/Rotation : the style comes from `Appearance` (Primary/Secondary/Outline/Subtle/Transparent).
15. ModernCard fills undefined slots with placeholder (stock photo + Subtitle/Description) : rendering only, **nothing goes through compile_canvas** ; define Image/Title/Subtitle/Description.
16. NEVER mix Classic and Modern properties : support is by exact type (Color/Size vs TitleColor/TitleSize text on ModernCard ; ModernProgressBar without Fill/Color/Font).
17. `Classic/Icon` : NEVER used by the ui-gen skill (icons = inline SVG via Image@2.2.3) — **documentation convention**, the control is valid for the machine (see Machine Correction n°4).
18. Variant forbidden on `ModernTextInput` and `Button@0.0.45` (e.g. `Variant: SingleLine` → PA1003) ; `TriggerOutput` is a STRING property (`="Keypress"`, not the enum → PA1010) ; documented current version of ModernTextInput : 1.1.2.
19. `Mode: =TextInputMode.SingleLine` is a wrong enum prefix — the correct one is `=TextMode.SingleLine` / `.MultiLine` / `.Password` (enum error, not PA2108).
20. `Navigate is not permitted in OnStart` : set `App.StartScreen` instead of navigating from App.OnStart or the arrival screen's OnVisible.
21. `TemplateSize` is **write-only** : never `Self.TemplateSize` in a formula ; read `Self.TemplateHeight` (vertical) / `Self.TemplateWidth` (horizontal) ; do not autosize a Gallery via CountRows/Self.TemplateHeight (possible zero-row rendering).

## Repair order by tiers (CreateConformance)

Tier 1 : `YamlInvalidSyntax` + `DuplicateNameInSequence` → tier 3 : app-wide entity duplicates → tier 4 : unknown properties. Always fix in this order : the upper tier masks the lower tier's diagnostics.
