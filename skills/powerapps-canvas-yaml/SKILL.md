---
ontology: true
name: powerapps-canvas-yaml
type: skill
domain: powerapps-canvas
status: active
discovered: 2026-11-13
description: >-
  Generate and fix .pa.yaml files for Power Apps Canvas apps with machine verification (compile_canvas).
  Declencheurs FR : pa.yaml, YAML Power Apps Canvas, ecran canvas en YAML, authoring MCP canvas, compilation canvas,
  Power Fx en YAML. Triggers EN : Power Apps canvas YAML, pa.yaml authoring, canvas app compile, canvas-authoring MCP.
summary: >-
  Generate and fix .pa.yaml files for Power Apps Canvas apps with machine verification (compile_canvas).
  Declencheurs FR : pa.yaml, YAML Power Apps Canvas, ecran canvas en YAML, authoring MCP canvas, compilation canvas,
  Power Fx en YAML. Triggers EN : Power Apps canvas YAML, pa.yaml authoring, canvas app compile, canvas-authoring MCP.
tags: [power-apps, pa-yaml, canvas, power-fx, mcp, skill]
related:
  - references/controls-reference
  - references/properties-matrix
  - references/enums-catalog
  - references/validation-rules
  - references/connectors-catalog
---

# Power Apps Canvas as YAML (.pa.yaml)

Decision body only. The detail lives in `references/` (5 files, machine MCP audit of 2026-11-13) and
`templates/` (4 YAML recompiled PASSED). This SKILL.md points to the references, it does not copy them.

## 1. When to use

- Create or modify `App.pa.yaml`, `<NomEcran>.pa.yaml`, `_EditorState.pa.yaml`, `Components/*.pa.yaml` of a Canvas app.
- Validate a folder of `.pa.yaml` files with `compile_canvas` before any delivery.
- Exclusions: model-driven apps, Dataverse rules/metadata, ALM solutions outside Canvas.

## 2. MCP prerequisites (canvas-authoring server)

- MCP server `canvas-authoring` configured and started (`.NET` command: `dnx Microsoft.PowerApps.CanvasAuthoring.McpServer --yes` (.NET 10 SDK required)).
- `connect(environment_id, app_id, environment_category, login_hint)`: **always pass `login_hint`** (account reuse without re-prompt, saved value in 00-manifest.json `mcp.login_hint_reuse`). Without `login_hint`, a 401/403 can force the account picker.
- Co-authoring session: **the Power Apps Studio tab must stay open** during the whole edit (00-manifest.json, `notes` field).
- Subagents: they share the connection of the common MCP process — **do not call `connect` again** (00-manifest.json, notes).
- Known call pitfall: `describe_control` expects the parameter `controlName`, NOT `controlTypeName` (00-manifest.json, `errors` field of 2026-11-13). Only the `describe_control` output is authoritative for the `Control:` keyword, the properties and the enums; `list_controls` only gives bare names.
- Verified tools: `list_controls`, `describe_control`, `list_apis`, `describe_api`, `list_data_sources`, `get_data_source_schema`, `compile_canvas`. Extras found: `sync_canvas`, `assess_edit_execution`, `get_appchecker_errors`, `get_accessibility_errors` (00-manifest.json, `mcp.tools_verified` / `tools_extra_discovered`).

## 3. Compact decision tree (need -> family -> control)

Five generic rules, then the exact pattern is looked up in the "Need -> control" table (28 patterns, literal
fields) of `references/controls-reference.md`:

1. **Any new instance: React family first** (`Modern*`). The only machine `do not use` directives point to
   `Button` -> `ModernButton`, `DataGrid`/`DataTable` -> `ModernDataGrid` (`references/controls-reference.md`, sections of the
   affected controls; 31 React controls, 10 FluentV9, 60 Classic, 16 MixedReality).
2. **Custom visual dressing** (radius, SVG, composite colors, overlay): `GroupContainer` + children;
   `ManualLayout` as soon as there is stacking or precise positioning; rounded corners on a control without
   Radius properties = pattern `radius_wrapper_conteneur_arrondi` (the container carries the radius).
3. **Repeated rows / lists**: `Gallery` (Variant mandatory Vertical / Horizontal / VariableHeight) +
   `GroupContainer AutoLayout` shell inside the template sized on `Parent.TemplateWidth/TemplateHeight`, columns via
   `FillPortions` — never X/Y inside the shell (pattern `gallery_ligne_responsive_via_shell_autolayout`).
4. **Data entry**: Modern controls carrying `Default` (check column 17 of
   `references/properties-matrix.md`); `Form` only with the mandatory `Layout:` keyword and with `DataCard`/`TypedDataCard`
   as its only children (`references/controls-reference.md`, Layout required).
5. **Classic vs Modern**: Modern by default; drop to Classic only for a property missing from the Modern
   control (e.g. `Radio` without `Default` -> `Classic/Radio@2.3.0`) — then the full keyword `Classic/...`, and never mix
   Classic/Modern properties on the same control.

The `Control:` keyword, the variant list and the `Variant:`/`Layout:` requirement of each control are read in its
block in `references/controls-reference.md` (117 controls, verbatim copies from `describe_control`).

## 4. Non-negotiable rules

Each rule = 1 statement + 1 proof (machine probe/date when it exists; otherwise documented reference).

1. **One `.pa.yaml` file = a single top-level object**: `App`, `EditorState`, `ComponentDefinitions` or `Screens`. A root-level children list does not parse. — Proof: probe p1, `YamlInvalidSyntax; ... PaModule` (references/validation-rules.md, structure 1-2, 2026-11-13).
2. **A screen is never a control**: top-level `Screens:` format + screen name as a mapping, with no `Control:` keyword at its level. — Proof: probes p1b, `Unknown control type 'Screen'` + `Early Preview` warning; the PA21xx labels are Studio labels, not compile_canvas codes (validation-rules.md, machine correction 3).
3. **Every property value starts with `=`** (structural keywords `Control`/`Variant`/`Children` excluded, validation-rules.md structure 7). — Without `=`: `Power Fx expressions must start with '='` on the whole file (validation-rules.md, structure 7; pattern copied into the 4 PASSED templates).
4. **A scalar containing `': '` gets quoted** (or block `|-`, with the `=` on the first line of content); **a Power Fx record literal gets quoted at the YAML level**: `Default: '={Value: "Tab1"}'`, never `Default: ={Value: "Tab1"}`. — The `invalid mapping` diagnostic names neither control nor line number (validation-rules.md, structures 8-10).
5. **A property exists only if `describe_control` lists it for that exact type**; no transfer by analogy, no property inherited from another control or the other family. — Proof: probe p2, `Unknown property 'Default' for control type 'TextInput@0.0.54'.` (validation-rules.md, Confirmed pitfalls).
6. **`Variant:` is a mandatory keyword for 6 controls** (`DataTableColumn`, `Gallery`, `GroupContainer`, `PartialCircle`, `Star`, `TypedDataCard`); **`Layout:` mandatory for `Form` and `FormViewer`**; Variant/Layout forbidden on `CanvasComponent`/`CodeComponent`. — Proof: probe p4b, `The keyword 'Variant' is required but is missing or empty.` (controls-reference.md, Mandatory keywords; validation-rules.md, structure 11).
7. **GroupContainer accepts the 3 variants `AutoLayout`, `GridLayout`, `ManualLayout`** (the YamlSyntax table is right, the legend "AutoLayout/ManualLayout only" was wrong); the bare form without `@version` is accepted. — Proof: probe p4, `Validation PASSED` with `Variant: GridLayout` (validation-rules.md, machine correction 1, 2026-11-13).
8. **Never add or remove an `@version` suffix yourself**: every instance of a given type carries the exact version bound in the session; at the first version warning, update everything ("self-healing"). — Proofs: probe p3b (`Label@1.0.0` vs current `2.5.1` = error "Another instance ... different version" + warning "older than the current version"); counter-probe p3: made-up HIGHER version `TextInput@99.99.99` = PASSED with no diagnostic (validation-rules.md, machine correction 2).
9. **Control names unique within the `Children:` sequence and app-wide**; in EDIT mode existing names are kept. — Proof: probe p5, code rendered literally `DuplicateNameInSequence` (validation-rules.md, Confirmed pitfalls; naming 1-3).
10. **`Children` is a named sequence whose order is significant** (Z-index / layout order). A responsive screen has exactly **ONE full-viewport `GroupContainer AutoLayout` root** (`Width/Height: =Parent...`, `LayoutMinWidth/Height: =0`), with every conditional surface under that root. — Proof: probe p0-base PASSED; pattern `racine_ecran_responsive` + QACHK-ROOT-CONTAINMENT (controls-reference.md, need->control table; validation-rules.md, hierarchy 1 and 4).
11. **No stacking in AutoLayout**: overlay = `ManualLayout` + X/Y, and the clickable overlay is placed **as the last child** (last = on top). An overlay in AutoLayout consumes the full height and makes its siblings invisible. — Patterns `overlay_bouton_impose_manuallayout`, hierarchy 8 (controls-reference.md; validation-rules.md).
12. **In CREATE, the first real screen overwrites `Screen1.pa.yaml`** (`Screen1:` key = file name), `App.StartScreen: =Screen1`, screens declared in `_EditorState.pa.yaml`; **never `Navigate()` from `App.OnStart` nor the arrival screen's OnVisible**. — The compiler reports `Navigate is not permitted in OnStart` (validation-rules.md, structure 13; pattern `ecran1_reutilise_premier_ecran`).
13. **An enum identity is not built, it is copied** from the `describe_control` output, then qualified per the 5 patterns (section "Power Fx enum identity syntax" below). The name changes per control for the same property (4 different enums for `Appearance`): never borrow another control's identity. — enums-catalog.md, Accuracy rule; validation-rules.md, 5 patterns.
14. **Fix in tier order**: `YamlInvalidSyntax` and `DuplicateNameInSequence` first — a file that does not parse masks ALL other diagnostics; unknown properties and versions come after. — Proof: behavior observed in probes, CreateConformance tiering (validation-rules.md, Fix order).
15. **`TemplateSize` is write-only**: never `Self.TemplateSize` in a formula; read `Self.TemplateHeight` (vertical) / `Self.TemplateWidth` (horizontal). — Documented pitfall n°21 (validation-rules.md); justified in templates/gallery-list.yaml, PITFALLS RESPECTED.
16. **Responsive before hard pixels**: absolute `X`/`Y`/`Width`/`Height` are the last resort (overlay, precise positioning, conservative numeric Gallery Height); otherwise AutoLayout + proportional shares. — Sources: patterns `racine_ecran_responsive`, `conteneur_autolayout_flux_responsive`, `gallery_ligne_responsive_via_shell_autolayout` (controls-reference.md, need->control table); rules 10 and 11 above.

### Responsive priority (operational)

- Screen root: a single full-viewport scrollable **AutoLayout** GroupContainer; the screen's `Children` contain ONLY this root (pattern `racine_ecran_responsive`).
- Flow and sizing: `FillPortions` between siblings (proportional values; table columns = IDENTICAL header/gallery FillPortions, pattern `table_de_donnees_header_plus_gallery`); never place X/Y INSIDE a Gallery row — the row is a GroupContainer AutoLayout sized on the template (pattern `gallery_ligne_responsive_via_shell_autolayout`).
- Parent-gated properties: a child of an AutoLayout or Grid GroupContainer may receive `AlignInContainer`, `FillPortions`, `LayoutMax*`, `LayoutMin*` (and `LayoutGrid*` in a grid) — this is the official responsive lever, documented in controls-reference.md (conditional properties, 39 controls).
- Gallery: `FillPortions` and `Height` exclude each other (patterns A/B, controls-reference.md); conservative numeric Height + explicit positive `TemplateSize` (pattern `gallery_viewport_borne`).
- `SizeRules` does NOT appear in `describe_control` outputs (verified: 0 occurrences across 117 controls) — never write it.
- `ui-*` templates (and ui-gen's ManualLayout patterns): designed in X/Y pixels; on takeover, rewrite in AutoLayout/FillPortions **then recompile** — the original PASSED guarantee does not cover the takeover.
- Relative formulas where the machine accepts them: dimensions derived from `Self.Width`/`App.Width` rather than constants; the PASSED templates' formulas are the source of safe shapes, do not invent `Min()`/`Max()` expressions without running them through compile_canvas.

## 8 deadly pitfalls (table)

| Pitfall | What the machine returns / fix | Evidence |
|---|---|---|
| `Default` on `TextInput@0.0.54` | `Unknown property 'Default' for control type 'TextInput@0.0.54'.` — omit the property, or go through `Classic/TextInput@2.3.2` | probe p2 (validation-rules.md) |
| `Value` on `ModernTextInput` | the control's output is **`Text`**: write `Trim(txtNom.Text)`. `Value` fails `Name isn't valid. 'Value' isn't recognized.` | 1 compile_canvas iteration on templates/form-input.yaml (template header) |
| Forgotten `Variant:`/`Layout:` | 6 controls require `Variant:` (DataTableColumn, Gallery, GroupContainer, PartialCircle, Star, TypedDataCard); `Form`/`FormViewer` require `Layout:` | controls-reference.md, Mandatory keywords; probe p4b |
| Overlay button in AutoLayout | the overlay (`Classic/Button` transparent) consumes the full height, siblings invisible → **switch the parent to `ManualLayout`**, overlay as last child | pattern `overlay_bouton_impose_manuallayout` (controls-reference.md) |
| `Control: Screen` | rejected: at the root `YamlInvalidSyntax`, as a child `Unknown control type 'Screen'` + `Early Preview code detected`. The PA2101 label is a Studio label, not MCP output | probes p1/p1b (validation-rules.md, machine correction 3) |
| `Label@1.0.0` + `Label@2.5.1` mixed | real error "Another instance of control type 'Label' has already been referenced using a different version" + obsolete version warning; **a made-up HIGHER version alone passes with no diagnostic** (p3) | probes p3b vs p3 (validation-rules.md, machine correction 2) |
| GroupContainer without `Variant` | `The keyword 'Variant' is required but is missing or empty.` — set `AutoLayout`, `GridLayout` or `ManualLayout` (all 3 PASS) | probes p4b/p4 (validation-rules.md, machine correction 1) |
| "NEVER Classic/Icon" | **documentation convention only** (ui-gen skill: icons = inline SVG via `Image@2.2.3`); the machine "do not use" list contains ONLY `Button`, `DataGrid`, `DataTable` | machine correction 4 (validation-rules.md) |

## Power Fx enum identity syntax

5 full patterns in `references/enums-catalog.md` (Accuracy rule) and `references/validation-rules.md`
(5 enum identity patterns). The two literal cases to remember:

- **Dotted identity** (name containing a dot): quote the FULL NAME in apostrophes, then the member —
  `Appearance: ='ButtonCanvas.Appearance'.Outline` (never `=ButtonCanvas.Appearance.Outline` without apostrophes).
- **Non-simple member** (leading digit, space): quote the MEMBER —
  `Precision: =DecimalPrecision.'1'` (bare `.1` yields "Expected operator" + "Expected an operand" without naming the enum); `Font: =Font.'Lato Light'`.

Other patterns, the 673-line catalog and per-control values: `references/enums-catalog.md`.
An unqualified member (`ThemeColor: =Subtle`) fails "Name isn't recognized": fix the qualifier, never remove it.

## The 15 templates (all recompiled PASSED by compile_canvas)

| Template | When to use | Machine evidence | Where to insert |
|---|---|---|---|
| `templates/base-screen.yaml` | any basic modern screen: full-viewport GroupContainer AutoLayout root + ModernText + ModernButton | `App.pa.yaml + Screen1.pa.yaml + _EditorState.pa.yaml` -> `Validation PASSED, Files validated: 3` (proof folder `_scratch\agent-6c\base-screen\`) | in CREATE, overwrite the workspace's `Screen1.pa.yaml`; `Screen1:` key = file name; declare the screen in `_EditorState.pa.yaml` |
| `templates/gallery-list.yaml` | any data list: Gallery Vertical + Items + AutoLayout shell inside the template + row labels | 3 files -> `Validation PASSED, Files validated: 3` (`_scratch\agent-6c\gallery-list\`) | full screen in CREATE, or a new screen key to declare in `ScreensOrder`; replace the inline `Table()` with a collection/source |
| `templates/form-input.yaml` | input form mutating a local collection (`Patch()`), card Label+ModernTextInput+ModernButton | 3 files -> PASSED after 1 iteration (`txtName.Value` -> `txtName.Text`, raw diagnostic quoted in its header) (`_scratch\agent-6c\form-input\`) | full screen in CREATE; **also copy its `App.pa.yaml`** (it initializes `colRecords`/`varSelectedCategory`); for a real `Form`: `Layout:` mandatory, children `DataCard`/`TypedDataCard` only |
| `templates/navigation.yaml` | full lateral navigation: 2 screens, sidebar menu Gallery (`Table()` + `PageNavigation`), fixed header, sidebar collapsible via `varMenuExpanded` | 4-file app (App + Screen1 + DetailsScreen + _EditorState) -> `Validation PASSED, Files validated: 4` (`_scratch\agent-6c\navigation\`) | start screen in CREATE; **also copy `DetailsScreen.pa.yaml`** from the proof folder and declare BOTH screens in `ScreensOrder` |

### The 11 ui-* templates (imported from `canvas-apps-ui-gen`, machine-verified on 2026-11-13: 11/11 PASSED, 0 rejects)

Content = the PASSED `Screen1.pa.yaml` copied verbatim; each file's `#` header = proof folder + mandatory App DEPENDENCIES (recopy the `OnStart` from the quoted scratch: `colTabs` for ui-tab-bar, `Tasks` + `TaskTypeOption` record for ui-screen-table-template-1). Verdict details: `_scratch\megapack\result.json`. For the three templates marked *, the corrected ui-* version IS AUTHORITATIVE, never go back to the ui-gen original:
`ui-card-template-1`* (resource names `'BG 1'` → literals `"BG 1"`) · `ui-card-template-2` · `ui-card-template-3` · `ui-custom-button` · `ui-data-table`* (`Direction`/`Gap` → `LayoutDirection`/`LayoutGap`; invalid `ThisItem.ItemIndex` → `RowNum` column in the `Table()` + `Mod(ThisItem.RowNum,2)`; probes: bare `ItemIndex`, `Parent.ItemIndex` and `UpTo` rejected by the validator) · `ui-form-template-1` · `ui-kpi-cards` · `ui-screen-table-template-1` · `ui-sidebar-collapsible`* (`'AppLogoFull'`/`'AppLogoIcon'` → literals `"…"`) · `ui-tab-bar` · `ui-table-template-1`

## 3 cross-source tensions settled

**Tension A — standard button.** The machine directive is literal: "Do not use Button unless it's already present
in this app. Use the updated ModernButton instead" (guidance copied into `references/controls-reference.md` for
`Button`, `DataGrid`, `DataTable`). In parallel, the hermes/05 sources describe the production button as
`GroupContainer ManualLayout` + `Label` + `Image`(SVG) + transparent `Classic/Button` as last child, because the
GroupContainer has no OnSelect (pattern `bouton_custom_overlay_transparent`, same references). **Settled**:
ModernButton by default for any simple action; Classic/Button overlay only when the custom dressing
(SVG + radius + composite colors) is required, in `ManualLayout`, assuming the classic nature; NEVER a new
`Button` (non-classic) nor `DataGrid`/`DataTable`.

**Tension B — `Self.LayoutGap` / `Self.PaddingTop`.** pa-yaml-rules recommends these forms; QAChecks Check 11
forbids the "no Self.LayoutGap" form (contradiction flagged by agent-5, 00-manifest.json, agents field).
`compile_canvas` does not settle it: neither form was probed. **Settled**: advise the numeric literal
(safer) as long as the machine does not settle it — the PASSED templates do `LayoutGap: =8` hardcoded;
pick one form and do not mix.

**Tension C — Gallery height.** One source computes the height as `AllItemsCount * TemplateHeight`; LayoutGuide/
QAChecks 30 forbid live autosizing via `CountRows`/`Self.AllItems`/`Self.TemplateHeight`
(pattern `gallery_viewport_borne`: conservative numeric Height, explicit positive `TemplateSize`;
`references/controls-reference.md`). **Settled**: advise the explicit computed value by default — height
computed at generation time and written as a literal value, never the live formula; in a sidebar without scroll, stay
on pattern A `FillPortions: =1` without `Height` (the two exclude each other, pattern `gallery_navigation_sidebar_patternes_A_B`).

## Upstream pipeline (screenshot / mockup → YAML)

This skill is downstream of `canvas-apps-ui-gen` (installed skill, same skills folder): it produces raw YAML
from a screenshot, a mockup or a description (multi-agent pipeline: layout, controls, styling, assembly + QA),
plus 11 production templates.

Handoff rules:
1. ui-gen's output is a CANDIDATE, not a delivery: it fully goes through the 4-step review path
   (previous section), without exception.
2. The machine is authoritative: ui-gen's rules and PA1001/2105/2108 legends yield to this skill's machine
   references — in particular the do-not-use list (Button/DataGrid/DataTable), the 3 GroupContainer variants, the real
   PA2105 trigger and the version mixing.
3. Machine verification state of the 11 ui-gen templates: `_scratch\megapack\result.json`
   (workshop finished on 2026-11-13: 11/11 PASSED, 0 rejects, verdicts and diffs in `_scratch\megapack\result.json`).
   The 11 are integrated right here under the `ui-` prefix (PASSED Screen1 + proof header); for the three fixed (*),
   the ui- version is authoritative, not the original still in the ui-gen skill.

## Honest limits of this audit

- **1 audited app only** (`app_id of the audited session`, env `environment_id of the audited session`, prod): everything else is extrapolation.
- **0 data source** in the app: `get_data_source_schema` never tested under real conditions; the `Form` + `SubmitForm()/ResetForm()` pair has **no traced literal formula** in the sources (00-manifest.json, agent-3 and agent-5).
- **1 API inventoried only** (Office365Users, 19 operations): `references/connectors-catalog.md` is NOT the complete Power Platform catalog.
- **14 pitfalls out of 21 are documented but unprobed**; the `required` nature of properties/enums is almost unknown to the service (`required: true` appears only 4 times in entries, `describe_control` emits no `Required:` line) — references/validation-rules.md and controls-reference.md, Mandatory nature section.
- **Session state as of 2026-11-13**: bound versions `Label@2.5.1`, `TextInput@0.0.54`. PA2105 being relative to the current bound version, **re-audit the session (describe_control + one probe) as soon as the app version differs** from the audit's.
- The PA1001/PA2101/PA2105/PA2108 labels are Power Apps Studio / paste-code labels; the MCP output only shows raw text, except `YamlInvalidSyntax` and `DuplicateNameInSequence` (machine correction 3).

## 4-step review path before delivering

1. **Does the control exist?** exact `Control:` keyword + `Variant:`/`Layout:` requirement + containment, in the control's block in `references/controls-reference.md`; absent from the 117 = stop, run `list_controls`/`describe_control`.
2. **Is each property of the exact type?** the matching column of `references/properties-matrix.md` (17 cross-cutting properties, real typings per family); outside the matrix = the control block's entry list; a property seen on another control proves nothing (the `Unknown property` trap).
3. **Is each enum in the catalog?** value copied from `references/enums-catalog.md` for THIS control and THIS property, qualified per one of the 5 patterns; unknown member = go back to `describe_control`, never invent.
4. **Compile.** `compile_canvas` on the complete folder (App + screens + `_EditorState`); repair by tiers (rule 14) until `Validation PASSED, Files validated: N`; re-read the RAW diagnostics of `references/validation-rules.md` to match the error you got.

## Where to look for what

| Question | File |
|---|---|
| Control, family, Variant/Layout, containment, need->control (28 patterns) | `references/controls-reference.md` |
| Cross-cutting property does it exist, real type, frequency | `references/properties-matrix.md` |
| Enum values and identities per control (673 lines) | `references/enums-catalog.md` |
| Raw diagnostics, 6 codes, 21 pitfalls, repair order | `references/validation-rules.md` |
| Office365Users API operations and their schemas | `references/connectors-catalog.md` |
| YAML skeletons proven PASSED | `templates/base-screen.yaml`, `gallery-list.yaml`, `form-input.yaml`, `navigation.yaml` |
| Project state, incidents, MCP state | the audit project's `00-manifest.json` (not shipped alongside the skill) |
