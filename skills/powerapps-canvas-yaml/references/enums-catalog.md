---
ontology: true
type: reference
domain: powerapps-yaml
summary: Exhaustive catalog of Power Apps enums in YAML, 673 lines copied literally from describe_control for 116 controls and 550 properties
tags: [powerapps, pa-yaml, powerfx, enums, identite, reference]
status: active
verified: true
discovered: 2026-11-13
workspace: powerappsyaml
sources:
  - ../../01-controls-types.json
  - ../../02-properties-detail.json
  - ../../04-validation-rules.json
related:
  - controls-reference
  - properties-matrix
  - validation-rules
  - ../SKILL
---

# Power Apps enums in YAML — exhaustive catalog

673 lines generated mechanically (`gen-enums.js`, agent 6a) from the `enums` field of the 117 controls of `02-properties-detail.json` : one line per (control, property, enumeration element). Sorted by family (React → FluentV9 → Classic → MixedReality), then the canonical order of `01-controls-types.json` for the controls, then the order of appearance in the `describe_control` output for the properties. The accuracy rule reproduced at the head comes from `04-validation-rules.json` (`enum_identity_syntax`, 5 patterns), copied without retouching.

**Copy, not rewording.** `identity_raw`, `enum_name_raw`, `values_raw` and `scope_raw` are copied character for character. The only transformations applied are those required by markdown : removal of backticks internal to the strings, escaping of the `|`, line ends replaced by spaces. No Power Fx identity was renamed, translated, requalified nor recomposed. `—` = `null` in the source.

**The identity name is spread over two columns.** Depending on the extraction batch, `describe_control` was captured in two ways : 374 lines have only `identity_raw` (qualified default `Enum.Membre` read from the `Default:` line), 218 lines have both columns, 81 lines have only `enum_name_raw` (the `Enum name:` line) — on these 81, only the default member is missing, the enumeration name remains known. **Before concluding an identity is unknown, read the other column.**

**The catalog copies, the author qualifies.** `values_raw` gives raw members (`Open Sans`, `Outline`, `Solid`). When writing the YAML, the member takes the form imposed by the 5 patterns below : `Font: =Font.'Open Sans'` (pattern 3, spaced member), `Appearance: ='ButtonCanvas.Appearance'.Outline` (pattern 1, dotted identity — that is exactly what the `Button` / `Appearance` line of this catalog gives, `enum_name_raw` = `ButtonCanvas.Appearance`), whereas `ModernButton.Appearance` provides `ButtonAppearance` and would fall under pattern 2. A never-qualified member fails (`ThemeColor: =Subtle`, pattern 4). The identity name changes per control for the same property : never borrow another control's identity.

**Scope.** The last column is the literal section of the output from which the property was extracted : `Input Properties`, "child of a vertical/horizontal GroupContainer", or "for variant `X`" (124 lines, all `TypedDataCard`). A property written outside its condition leaves the scope of the control type, and `04-validation-rules.json` documents the diagnostic for this case : `Unknown property 'Default' for control type 'TextInput@0.0.54'.` (probe p2-bad-property).

## Accuracy rule for enumeration identities

_Source : `04-validation-rules.json`, table `enum_identity_syntax` (5 patterns), instructions copied literally. A Power Fx identity is never built : it is copied from the `describe_control` output._

### 1. `='EnumIdentity'.Value`

Identite d'enumeration POINTEE ou non-simple : guillemeter le NOM COMPLET en apostrophes comme identifiant Power Fx, puis ajouter .Member. Exemple exact : Appearance: ='ButtonCanvas.Appearance'.Outline (jamais =ButtonCanvas.Appearance.Primary sans apostrophes).

_Source : hermes:skills/power-platform/canvas-apps-shared\references\CreateBuilderCore.md line 26 (QACHK-ENUM-LITERAL) + canvas-apps-canvas-app\SKILL.md line 83 + EditConformance.md line 28 + canvas-apps-shared\agents\canvas-screen-builder.md line 39 + real usage in canvas-apps-ui-gen\templates\form-template-1.yaml lines 362-364_

### 2. `=SimpleEnumName.Value`

Nom d'enumeration simple (sans point/espace/caractere special) : pas de guillemetage. Exemples : Appearance: =Appearance.Underline ; =BadgeAppearance.Tint ; =ButtonAppearance.Primary UNIQUEMENT si ce nom exact est renvoye par describe_control. Ne JAMAIS construire le nom d'enumeration : le copier depuis la ligne 'Enum name:' de describe_control (4 types differents pour la propriete Appearance selon le controle).

_Source : github:microsoft/power-platform-skills/plugins\canvas-apps\references\ControlGuide.md (sections Enum type names, lines 268-306)_

### 3. `=EnumName.'Member'`

Membre non simple (commencant par un chiffre ou contenant espace/caracteres speciaux) : guillemeter le MEMBRE separement. Exemples : Precision: =DecimalPrecision.'1' (jamais .1) ; Font: =Font.'Open Sans' ; =Font.'Lato Light'. Le membre numerique non guillemete produit la paire 'Expected operator' + 'Expected an operand' sans jamais nommer l'enumeration.

_Source : ControlGuide.md (Enum member values, lines 312-336 + Troubleshooting lines 461-464) + form-template-1.yaml + QACHK-ENUM-LITERAL_

### 4. `=EnumName.Member`

Un membre n'est JAMAIS un identifiant nu : ThemeColor: =Subtle echoue 'Name isn't recognized'. Corriger le qualifiant, jamais le supprimer.

_Source : ControlGuide.md lines 308-310_

### 5. `='Option Set'.'Value'`

Valeurs d'option set Dataverse : echapper le NOM de l'option set et la VALEUR avec des apostrophes si elles contiennent espaces, caracteres speciaux ou commencent par un chiffre. Exemples : 'Account Status' = 'Account Status'.Active ; 'Status (Assignments)'.Active.

_Source : ControlGuide.md (Option set values, lines 338-356)_

## Catalog contents

- Lines : **673** (one line per control / property / enumeration element pair found in `02-properties-detail.json`).
- Control-property pairs carrying an enumeration : **550**.
- Elements with non-null `identity_raw` : **592** ; without `identity_raw` (the qualified default `Enum.Membre` did not appear in the output) : **81**. On these 81 lines, `enum_name_raw` is present : the enumeration name remains known, only the default member is missing.
- Elements with non-null `enum_name_raw` : **299**.
- Controls present : **116** out of 117. Absent from the table, controls with no enumerated property in 02 : `DataTableColumn`.
- Sorting : family in the order React → FluentV9 → Classic → MixedReality, then the canonical order of `01-controls-types.json` for the controls, then the order of appearance in `02-properties-detail.json` for the properties.
- `—` = `null` field in `02-properties-detail.json`.

### Scopes (`scope_raw`) encountered

_Literal section of the `describe_control` output from which the property is extracted : it conditions the availability of the property (a conditional-section property refused outside its condition = « Unknown property » diagnostic)._

| Scope (literal `scope_raw`) | Lines |
| --- | --- |
| `Input Properties` | 136 |
| `Additional input properties if a child of a Vertical or Horizontal GroupContainer` | 39 |

- 124 more distinct scopes, all of the form `Additional input properties for variant X` (124 lines), on the controls : `TypedDataCard`. They are not expanded here : each line of the per-family tables carries its own in the last column.


### Table fields

| Column | Source field in `02-properties-detail.json` |
| --- | --- |
| Control | `control_name` |
| Property | key of the control's `enums` object |
| `enum_name_raw` | `enums[propriete][].enum_name_raw` |
| `identity_raw` | `enums[propriete][].identity_raw` |
| `values_raw` | `enums[propriete][].values_raw` |
| Scope | `enums[propriete][].scope_raw` |

Extraction notes of `02-properties-detail.json` (`extraction_notes`), copied literally :

> Enums: identity_raw= litteral de la ligne 'Enum name', values_raw= litteral de la ligne 'Enum values'; les identites Power Fx individuelles des valeurs ne sont pas detaillees dans la sortie (seules celles visibles dans les lignes Default, ex. NomValeur.Qualifiee).

> enums identity_raw: repris du Default quand il a une forme d'identifiant Power Fx (Ex.Type ou Parent.Type) ; null sinon.

> Proprietes d'entree conditionnelles: la description_raw reprise est l'en-tete litteral de la section Additional input properties; la condition s'applique a toutes les proprietes listees sous cette section.

Copy rule : `identity_raw` and `values_raw` are copied here character for character. Every line carries the enumeration identity name in at least one of the two columns (`identity_raw` or `enum_name_raw`) : if one is `—`, read the other before going back to `describe_control`, and never rebuild an identity by hand.

---

## React family — 189 lines, 31 controls

| Control | Property | `enum_name_raw` | `identity_raw` | `values_raw` | Scope |
| --- | --- | --- | --- | --- | --- |
| `DataWedgeScanner` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `DataWedgeScanner` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `Geolocation` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Geolocation` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernAttachments` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernAttachments` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernAttachments` | `DropShadow` | — | `DropShadow` | `Bold, ExtraBold, Light, None, Regular, Semibold, Semilight` | — |
| `ModernAttachments` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernAttachments` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernAvatar` | `Appearance` | — | `AvatarColor` | `Anchor, Beige, Blue, Brand, Brass, Brown, Cornflower, Cranberry, DarkGreen, DarkRed, Forest, Gold, Grape, Lavender, LightTeal, Lilac, Magenta, Marigold, Mink, Navy, Peach, Pink, Platinum, Plum, Pumpkin, Purple, Red, RoyalBlue, Seafoam, Steel, Teal` | — |
| `ModernAvatar` | `Badge` | — | `PresenceBadge` | `Available, Away, Blocked, Busy, Do not disturb, None, Offline, Out of office, Unknown` | — |
| `ModernAvatar` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernAvatar` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernAvatar` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernAvatar` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernAvatar` | `Shape` | — | `AvatarShape` | `Circular, Rounded, Square` | — |
| `ModernAvatar` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernAvatarGroup` | `Appearance` | — | `AvatarColor` | `Anchor, Beige, Blue, Brand, Brass, Brown, Cornflower, Cranberry, DarkGreen, DarkRed, Forest, Gold, Grape, Lavender, LightTeal, Lilac, Magenta, Marigold, Mink, Navy, Peach, Pink, Platinum, Plum, Pumpkin, Purple, Red, RoyalBlue, Seafoam, Steel, Teal` | — |
| `ModernAvatarGroup` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernAvatarGroup` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernAvatarGroup` | `Layout` | — | `AvatarGroupLayout` | `Spread, Stack` | — |
| `ModernAvatarGroup` | `OverflowIndicator` | — | `AvatarGroupOverflowIndicator` | `Auto, Count, Icon` | — |
| `ModernAvatarGroup` | `Shape` | — | `AvatarShape` | `Circular, Rounded, Square` | — |
| `ModernAvatarGroup` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernBadge` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernBadge` | `Appearance` | — | `BadgeAppearance` | `Filled, Ghost, Outline, Tint` | — |
| `ModernBadge` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernBadge` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernBadge` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernBadge` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernBadge` | `IconPosition` | — | `BadgeIconPosition` | `After, Before` | — |
| `ModernBadge` | `Intent` | — | `BadgeIntent` | `Attention, Caution, Critical, Highlight, Informative, Neutral, Positive, Urgent` | — |
| `ModernBadge` | `Shape` | — | `BadgeShape` | `Circular, Rounded, Square` | — |
| `ModernBadge` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `ModernBadge` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernButton` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernButton` | `Appearance` | — | `ButtonAppearance` | `Outline, Primary, Secondary, Subtle, Transparent` | — |
| `ModernButton` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernButton` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernButton` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernButton` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernButton` | `IconStyle` | — | `IconStyle` | `Filled, Outline` | — |
| `ModernButton` | `Layout` | — | `ButtonLayout` | `IconAfter, IconBefore, IconOnly, TextOnly` | — |
| `ModernButton` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `ModernButton` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernCard` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernCard` | `DropShadow` | — | `DropShadow` | `Bold, ExtraBold, Light, None, Regular, Semibold, Semilight` | — |
| `ModernCard` | `ImagePlacement` | — | `ImagePlacement` | `AfterHeader, BeforeHeader` | — |
| `ModernCard` | `ImagePosition` | — | `ImagePosition` | `Center, Fill, Fit, Stretch, Tile` | — |
| `ModernCard` | `LayoutDirection` | — | `LayoutDirection` | `Horizontal, Vertical` | — |
| `ModernCard` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernCardGallery` | `CardDropShadow` | — | `DropShadow` | `Bold, ExtraBold, Light, None, Regular, Semibold, Semilight` | — |
| `ModernCardGallery` | `CardImagePlacement` | — | `ImagePlacement` | `AfterHeader, BeforeHeader` | — |
| `ModernCardGallery` | `CardImagePosition` | — | `ImagePosition` | `Center, Fill, Fit, Stretch, Tile` | — |
| `ModernCardGallery` | `CardLayoutDirection` | — | `LayoutDirection` | `Horizontal, Vertical` | — |
| `ModernCardGallery` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernCardGallery` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernCheckbox` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernCheckbox` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernCheckbox` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernCheckbox` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernCombobox` | `Appearance` | — | `Appearance` | `FilledDarker, FilledLighter, Outline` | — |
| `ModernCombobox` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernCombobox` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernCombobox` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernCombobox` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernCombobox` | `ValidationState` | — | `ValidationState` | `Error, None` | — |
| `ModernCombobox` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernDataGrid` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernDataGrid` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernDataGrid` | `DropShadow` | — | `DropShadow` | `Bold, ExtraBold, Light, None, Regular, Semibold, Semilight` | — |
| `ModernDataGrid` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernDataGrid` | `Pagination` | — | `Pagination` | `Auto, None, Number` | — |
| `ModernDataGrid` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernDataGridColumn` | `DisplayType` | — | `BooleanDisplay` | `Checkbox, Text, Toggle` | — |
| `ModernDataGridColumn` | `Appearance` | — | `ButtonAppearance` | `Outline, Primary, Secondary, Subtle, Transparent` | — |
| `ModernDataGridColumn` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernDatePicker` | `Appearance` | — | `Appearance` | `FilledDarker, FilledLighter, Outline` | — |
| `ModernDatePicker` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernDatePicker` | `DateTimeZone` | — | `DateTimeZone` | `Local, UTC` | — |
| `ModernDatePicker` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernDatePicker` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernDatePicker` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernDatePicker` | `Format` | — | `DatePickerFormat` | `LongAbbreviated, Short, YearMonth` | — |
| `ModernDatePicker` | `StartOfWeek` | — | `StartOfWeek` | `Friday, Monday, MondayZero, Saturday, Sunday, Thursday, Tuesday, Wednesday` | — |
| `ModernDatePicker` | `ValidationState` | — | `ValidationState` | `Error, None` | — |
| `ModernDatePicker` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernDropdown` | `Appearance` | — | `Appearance` | `FilledDarker, FilledLighter, Outline` | — |
| `ModernDropdown` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernDropdown` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernDropdown` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernDropdown` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernDropdown` | `ValidationState` | — | `ValidationState` | `Error, None` | — |
| `ModernDropdown` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernIcon` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernIcon` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernIcon` | `IconStyle` | — | `IconStyle` | `Filled, Outline` | — |
| `ModernIcon` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernInformationButton` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernInformationButton` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernInformationButton` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernInformationButton` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernLink` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernLink` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernLink` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernLink` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernLink` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernLink` | `Type` | — | `LinkAppearance` | `Default, Subtle` | — |
| `ModernLink` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `ModernLink` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernNumberInput` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernNumberInput` | `Appearance` | — | `Appearance` | `FilledDarker, FilledLighter, Outline` | — |
| `ModernNumberInput` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernNumberInput` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernNumberInput` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernNumberInput` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernNumberInput` | `Precision` | — | `DecimalPrecision` | `0, 1, 2, 3, 4, 5, Auto` | — |
| `ModernNumberInput` | `ValidationState` | — | `ValidationState` | `Error, None` | — |
| `ModernNumberInput` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernPersona` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernPersona` | `Appearance` | — | `AvatarColor` | `Anchor, Beige, Blue, Brand, Brass, Brown, Cornflower, Cranberry, DarkGreen, DarkRed, Forest, Gold, Grape, Lavender, LightTeal, Lilac, Magenta, Marigold, Mink, Navy, Peach, Pink, Platinum, Plum, Pumpkin, Purple, Red, RoyalBlue, Seafoam, Steel, Teal` | — |
| `ModernPersona` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernPersona` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernPersona` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernPersona` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernPersona` | `Presence` | — | `PresenceBadge` | `Available, Away, Blocked, Busy, Do not disturb, None, Offline, Out of office, Unknown` | — |
| `ModernPersona` | `Shape` | — | `AvatarShape` | `Circular, Rounded, Square` | — |
| `ModernPersona` | `TextAlignment` | — | `PersonaTextAlignment` | `Center, Start` | — |
| `ModernPersona` | `TextPosition` | — | `PersonaTextPosition` | `After, Before, Below` | — |
| `ModernPersona` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `ModernPersona` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernProgressBar` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernProgressBar` | `ProgressColor` | — | `ProgressColor` | `Brand, Error, Success, Warning` | — |
| `ModernProgressBar` | `Shape` | — | `ProgressShape` | `Rounded, Square` | — |
| `ModernProgressBar` | `Thickness` | — | `ProgressThickness` | `Large, Medium` | — |
| `ModernProgressBar` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernRadio` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernRadio` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernRadio` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernRadio` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernRadio` | `Layout` | — | `OptionLayout` | `Horizontal, HorizontalStacked, Vertical` | — |
| `ModernRadio` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernRating` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernRating` | `Step` | — | `RatingStep` | `Full, Half` | — |
| `ModernRating` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernSlider` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernSlider` | `LayoutDirection` | — | `LayoutDirection` | `Horizontal, Vertical` | — |
| `ModernSlider` | `Size` | — | `SliderSize` | `Medium, Small` | — |
| `ModernSlider` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernSpinner` | `Appearance` | — | `SpinnerAppearance` | `Inverted, Primary` | — |
| `ModernSpinner` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernSpinner` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernSpinner` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernSpinner` | `LabelPosition` | — | `SpinnerLabelPosition` | `Above, After, Before, Below` | — |
| `ModernSpinner` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernTabList` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernTabList` | `Alignment` | — | `LayoutDirection` | `Horizontal, Vertical` | — |
| `ModernTabList` | `Appearance` | — | `TabListAppearance` | `FilledCircular, Subtle, SubtleCircular, Transparent` | — |
| `ModernTabList` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernTabList` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernTabList` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernTabList` | `TabSize` | — | `TabSize` | `Large, Medium, Small` | — |
| `ModernTabList` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernText` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernText` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernText` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernText` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernText` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernText` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `ModernText` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernTextInput` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `ModernTextInput` | `Appearance` | — | `Appearance` | `FilledDarker, FilledLighter, Outline` | — |
| `ModernTextInput` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ModernTextInput` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernTextInput` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernTextInput` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernTextInput` | `TriggerOutput` | — | `TriggerOutput` | `Delayed, FocusOut, Keypress` | — |
| `ModernTextInput` | `Type` | — | `TextInputType` | `Multiline, Password, Search, SingleLine` | — |
| `ModernTextInput` | `ValidationState` | — | `ValidationState` | `Error, None` | — |
| `ModernTextInput` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `ModernToggle` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ModernToggle` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ModernToggle` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `ModernToggle` | `LabelPosition` | — | `ToggleLabelPosition` | `Above, After, Before` | — |
| `ModernToggle` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `NativePdfViewer` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `NativePdfViewer` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `NativePenInput` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `NativePenInput` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |

---

## FluentV9 family — 44 lines, 10 controls

| Control | Property | `enum_name_raw` | `identity_raw` | `values_raw` | Scope |
| --- | --- | --- | --- | --- | --- |
| `Breadcrumb` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Breadcrumb` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Button` | `Align` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `Button` | `Appearance` | `ButtonCanvas.Appearance` | — | `Outline, Primary, Secondary, Subtle, Transparent` | `Input Properties` |
| `Button` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Button` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Button` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `Button` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `Button` | `IconStyle` | `ButtonCanvas.IconStyle` | — | `Filled, Outline` | `Input Properties` |
| `Button` | `Layout` | `ButtonCanvas.Layout` | — | `IconAfter, IconBefore, IconOnly, TextOnly` | `Input Properties` |
| `Button` | `VerticalAlign` | `VerticalAlign` | — | `Bottom, Middle, Top` | `Input Properties` |
| `Button` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Header` | `Align` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `Header` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Header` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Header` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `Header` | `Style` | `Header.Style` | — | `Light, Neutral, Primary` | `Input Properties` |
| `Header` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `InteractiveText` | `Align` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `InteractiveText` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `InteractiveText` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `InteractiveText` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `InteractiveText` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `InteractiveText` | `VerticalAlign` | `VerticalAlign` | `VerticalAlign.Middle` | `Bottom, Middle, Top` | `Input Properties` |
| `InteractiveText` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `IoTDeviceManager` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `IoTDeviceManager` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `IoTDeviceManager` | `Layout` | `IoTDeviceManager.Layout` | — | `Horizontal, Vertical` | `Input Properties` |
| `IoTDeviceManager` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `PowerVirtualAgentsChat` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `PowerVirtualAgentsChat` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `SpinButton` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `SpinButton` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `StreamVideo` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Switch` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Switch` | `LabelPosition` | `Switch.LabelPosition` | — | `Above, After, Before` | `Input Properties` |
| `Switch` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Toolbar` | `Alignment` | `Toolbar.Alignment` | — | `Horizontal, Vertical` | `Input Properties` |
| `Toolbar` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Toolbar` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `Toolbar` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `Toolbar` | `Layout` | `Toolbar.Layout` | — | `IconAbove, IconAfter, IconBefore, IconOnly, TextOnly` | `Input Properties` |
| `Toolbar` | `Padding` | `Toolbar.Padding` | — | `Large, Medium, Small` | `Input Properties` |
| `Toolbar` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |

---

## Classic family — 353 lines, 59 controls

| Control | Property | `enum_name_raw` | `identity_raw` | `values_raw` | Scope |
| --- | --- | --- | --- | --- | --- |
| `AddMedia` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `AddMedia` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `AddMedia` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `AddMedia` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `AddMedia` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `AddMedia` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `AddMedia` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `Arrow` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Arrow` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Arrow` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `Attachments` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Attachments` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Attachments` | `DropTargetBorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Attachments` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Attachments` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Attachments` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `Audio` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Audio` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Audio` | `ImagePosition` | — | `ImagePosition` | `Center, Fill, Fit, Stretch, Tile` | — |
| `Audio` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `BarChart` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `BarChart` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `BarChart` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `BarChart` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `BarChart` | `GridStyle` | — | `GridStyle` | `All, None, XOnly, YOnly` | — |
| `Barcode` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `Barcode` | `BarcodeType` | — | `BarcodeType` | `Auto, Aztec, Codabar, Code128, Code39, Code93, DataMatrix, Ean, I2of5, Pdf417, QRCode, Rss14, RssExpanded, Upc` | — |
| `Barcode` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Barcode` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `BarcodeScanner` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `BarcodeScanner` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `BarcodeScanner` | `BarcodeType` | — | `BarcodeType` | `Auto, Aztec, Codabar, Code128, Code39, Code93, DataMatrix, Ean, I2of5, Pdf417, QRCode, Rss14, RssExpanded, Upc` | — |
| `BarcodeScanner` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `BarcodeScanner` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `BarcodeScanner` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `BarcodeScanner` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `BarcodeScanner` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `Camera` | `AlignInContainer` | — | `AlignInContainer` | `Center, End, SetByContainer, Start, Stretch` | — |
| `Camera` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Camera` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Circle` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Circle` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/Button` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `Classic/Button` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/Button` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/Button` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/Button` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/Button` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `Classic/CheckBox` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/CheckBox` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/CheckBox` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/CheckBox` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/CheckBox` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `Classic/ComboBox` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/ComboBox` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/ComboBox` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/ComboBox` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/DatePicker` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/DatePicker` | `DateTimeZone` | — | `DateTimeZone` | `Local, UTC` | — |
| `Classic/DatePicker` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/DatePicker` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/DatePicker` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/DatePicker` | `StartOfWeek` | — | `StartOfWeek` | `Friday, Monday, MondayZero, Saturday, Sunday, Thursday, Tuesday, Wednesday` | — |
| `Classic/DropDown` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/DropDown` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/DropDown` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/DropDown` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/Icon` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/Icon` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/Icon` | `Icon` | — | `Icon` | `Add, AddDocument, AddLibrary, AddToCalendar, AddUser, Airplane, Alarm, ArrowDown, ArrowLeft, ArrowRight, ArrowUp, BackArrow, Bell, Blocked, Bookmark, BookmarkFilled, Bug, Bus, Calculator, CalendarBlank, Camera, CameraAperture, Cancel, CancelBadge, Cars, Check, CheckBadge, ChevronDown, ChevronLeft, ChevronRight, ChevronUp, ClearDrawing, Clock, CollapseView, ColorPicker, Compose, ComputerDesktop, Controller, Copy, Crop, Currency, Cut, Database, DetailList, Devices, Diamond, DockCheckProperties, DockLeft, DockRight, Document, DocumentPDF, DocumentWithContent, Download, Draw, Edit, EmojiFrown, EmojiHappy, EmojiNeutral, EmojiSad, EmojiSmile, Enhance, Erase, Error, ExpandView, Export, Filter, FilterFlat, FilterFlatFilled, Flag, Folder, ForkKnife, Globe, GlobeChangesPending, GlobeError, GlobeNotConnected, GlobeRefresh, GlobeWarning, HalfFilledCircle, Hamburger, Health, Heart, Help, Hide, History, Home, HorizontalLine, Hospital, Import, Information, Items, Journal, Key, Laptop, Leave, LevelsLayersItems, Lightbulb, LightningBolt, LikeDislike, LineWeight, Link, ListScrollEmpty, ListScrollWatchlist, ListWatchlistRemind, Location, Lock, LogJournal, Mail, Manufacture, Medical, Message, Microphone, Mobile, Money, More, Newspaper, NextArrow, Note, Notebook, OfficeBuilding, OpenInNewWindow, OptionsList, PaperClip, Paste, People, Person, Phone, PhoneHangUp, Phonebook, PhotosPictures, Pin, Post, Print, Printing3D, Publish, QuestionMark, RadarActivityMonitor, Redo, Reload, Reset, Ribbon, Save, Scan, Search, Send, Settings, Share, Shirt, Shop, ShoppingCart, Signal, Sort, Support, Sync, Tablet, Tag, Text, ThumbsDown, ThumbsDownFilled, ThumbsUp, ThumbsUpFilled, Tools, ToolsWrench, Train, Transportation, Trash, Tray, Trending, TrendingHashtag, TrendingUpwards, Undo, Unlock, VerticalLine, Video, View, Waffle, Warning, Waypoint, Weather, ZoomIn, ZoomOut` | — |
| `Classic/Radio` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `Classic/Radio` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/Radio` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/Radio` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/Radio` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/Radio` | `Layout` | — | `Layout` | `Horizontal, Vertical` | — |
| `Classic/Slider` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/Slider` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/Slider` | `Layout` | — | `Layout` | `Horizontal, Vertical` | — |
| `Classic/StreamVideo` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/StreamVideo` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/TextInput` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `Classic/TextInput` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/TextInput` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/TextInput` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/TextInput` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/TextInput` | `Format` | — | `TextFormat` | `Number, Text` | — |
| `Classic/TextInput` | `Mode` | — | `TextMode` | `MultiLine, Password, SingleLine` | — |
| `Classic/TextInput` | `VirtualKeyboardMode` | — | `VirtualKeyboardMode` | `Auto, Numeric, Text` | — |
| `Classic/Toggle` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Classic/Toggle` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Classic/Toggle` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Classic/Toggle` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Classic/Toggle` | `TextPosition` | — | `TextPosition` | `Left, Right` | — |
| `DataCard` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `DataCard` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `DataGrid` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `DataGrid` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `DataGrid` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `DataGrid` | `HeadingFont` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `DataGrid` | `HeadingFontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `DataTable` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `DataTable` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `DataTable` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `DataTable` | `HeadingFont` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `DataTable` | `HeadingFontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `EntityForm` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `EntityForm` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `EntityForm` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `EntityForm` | `Pattern` | — | `FormPattern` | `CardList, Details, List, None` | — |
| `Export` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `Export` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Export` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Export` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Export` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Export` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `FluidGrid` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Form` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Form` | `DefaultMode` | — | `FormMode` | `Edit, New, View` | — |
| `FormsPro` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `FormsPro` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `FormViewer` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Gallery` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Gallery` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Gallery` | `Transition` | — | `Transition` | `None, Pop, Push` | — |
| `GroupContainer` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `GroupContainer` | `DropShadow` | — | `DropShadow` | `Bold, ExtraBold, Light, None, Regular, Semibold, Semilight` | — |
| `GroupContainer` | `LayoutAlignItems` | — | `LayoutAlignItems` | `Center, End, Start, Stretch` | — |
| `GroupContainer` | `LayoutDirection` | — | `LayoutDirection` | `Horizontal, Vertical` | — |
| `GroupContainer` | `LayoutJustifyContent` | — | `LayoutJustifyContent` | `Center, End, SpaceBetween, Start` | — |
| `GroupContainer` | `LayoutOverflow` | — | `LayoutOverflow` | `Hide, Scroll` | — |
| `Hexagon` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Hexagon` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `HtmlViewer` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `HtmlViewer` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `HtmlViewer` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Image` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Image` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Image` | `ImagePosition` | — | `ImagePosition` | `Center, Fill, Fit, Stretch, Tile` | — |
| `Image` | `ImageRotation` | — | `ImageRotation` | `None, Rotate180, Rotate270, Rotate90` | — |
| `Import` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `Import` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Import` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Import` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Import` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Import` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `Label` | `Align` | — | `Align` | `Center, Justify, Left, Right` | — |
| `Label` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Label` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Label` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Label` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Label` | `Live` | — | `Live` | `Assertive, Off, Polite` | — |
| `Label` | `Overflow` | — | `Overflow` | `Hidden, Scroll` | — |
| `Label` | `Role` | — | `TextRole` | `Default, Heading1, Heading2, Heading3, Heading4` | — |
| `Label` | `VerticalAlign` | — | `VerticalAlign` | `Bottom, Middle, Top` | — |
| `Legend` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Legend` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Legend` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Legend` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `LineChart` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `LineChart` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `LineChart` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `LineChart` | `GridStyle` | — | `GridStyle` | `All, None, XOnly, YOnly` | — |
| `ListBox` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `ListBox` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `ListBox` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `ListBox` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Lookup` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Lookup` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Lookup` | `Font` | — | `Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | — |
| `Lookup` | `FontWeight` | — | `FontWeight` | `Bold, Lighter, Normal, Semibold` | — |
| `Microphone` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Microphone` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `Microphone` | `ImagePosition` | — | `ImagePosition` | `Center, Fill, Fit, Stretch, Tile` | — |
| `Octagon` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Octagon` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `PartialCircle` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `PartialCircle` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `PDFViewer` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `PDFViewer` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `PenInput` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `PenInput` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `PenInput` | `Mode` | — | `PenMode` | `Draw, Erase` | — |
| `Pentagon` | `BorderStyle` | — | `BorderStyle` | `Dashed, Dotted, None, Solid` | — |
| `Pentagon` | `DisplayMode` | — | `DisplayMode` | `Disabled, Edit, View` | — |
| `PieChart` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `PieChart` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `PieChart` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `PieChart` | `LabelPosition` | `LabelPosition` | `LabelPosition.Outside` | `Inside, Outside` | `Input Properties` |
| `PieChart` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `PowerBI` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `PowerBI` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `PowerBI` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Rating` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Rating` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Rating` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Rectangle` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Rectangle` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Rectangle` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `RichTextEditor` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `RichTextEditor` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `SalesNavigatorAccount` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `SalesNavigatorAccount` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `SalesNavigatorLead` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `SalesNavigatorLead` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `SpeechControl` | `BorderStyle` | `BorderStyle` | `BorderStyle.None` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `SpeechControl` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `SpeechControl` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `SpeechControl` | `VerticalAlign` | `VerticalAlign` | `VerticalAlign.Middle` | `Bottom, Middle, Top` | `Input Properties` |
| `SpeechControl` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Star` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Star` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Star` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Timer` | `Align` | `Align` | `Align.Center` | `Center, Justify, Left, Right` | `Input Properties` |
| `Timer` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Timer` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Timer` | `Font` | `Font` | `App.Theme.Font` | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `Timer` | `FontWeight` | `FontWeight` | `FontWeight.Normal` | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `Timer` | `VerticalAlign` | `VerticalAlign` | `VerticalAlign.Middle` | `Bottom, Middle, Top` | `Input Properties` |
| `Timer` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Triangle` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Triangle` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Triangle` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `TypedDataCard` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Additional input properties for variant BlankCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicAllowedValuesNumberEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicAllowedValuesStringEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicAttachmentsEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicAttachmentsView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicBlankPolymorphicEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxMultiSelectEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxMultiSelectView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxOptionSetMultiEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxOptionSetMultiView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxOptionSetSingleEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxOptionSetSingleView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicComboBoxView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicCurrencyEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicDateEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicDateTimeEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicEmailView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicFileEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicFileView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicImageView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicLargeImageView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicLargePicture` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicLookupEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicLookupView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicNotes` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicNumberEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicPercentageView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicPhoneView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicPicture` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicRatingEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicRatingView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicRichTextEditorEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicRichTextEditorView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicSliderEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicSmartEmailEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicTextualEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicTextualMultilineEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicTextualView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicTimeEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicToggleEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicToggleView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicUrlEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicUrlView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ClassicUSCurrencyView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ClassicYesNoRadioEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxMultiSelectEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxMultiSelectView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxOptionSetMultiEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxOptionSetMultiView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxOptionSetSingleEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxOptionSetSingleView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ComboBoxView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant CurrencyEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant DateEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant DateTimeEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant FileEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant FileView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ImageView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant LargeImageView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant LargePicture` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernAllowedValuesNumberEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernAllowedValuesStringEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernAttachmentsEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernAttachmentsView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxMultiSelectEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxMultiSelectView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxOptionSetMultiEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxOptionSetMultiView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxOptionSetSingleEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxOptionSetSingleView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernComboBoxView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernCurrencyEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernDateEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernDateTimeEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernEmailView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernNumberEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernPhoneView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernRatingEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernRatingView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernSliderEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernTextualEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernTextualMultilineEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernTextualView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernToggleEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernToggleView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernUrlEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernUrlView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ModernUSCurrencyView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ModernYesNoRadioEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant NumberEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxMultiSelectEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxMultiSelectViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxOptionSetMultiEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxOptionSetMultiViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxOptionSetSingleEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxOptionSetSingleViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreComboBoxViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreCurrencyEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreDateEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreDateTimeEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreNumberEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreRatingEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreRatingViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreSliderEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreTextualEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreTextualMultiLineEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreTextualViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreToggleEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreToggleViewCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant PcfCoreUrlEditCard` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant Picture` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant RatingEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant RatingView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant SliderEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant TextualEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant TextualMultilineEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant TextualView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant ToggleEdit` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `DisplayMode.View` | `Disabled, Edit, View` | `Additional input properties for variant ToggleView` |
| `TypedDataCard` | `DisplayMode` | `DisplayMode` | `Parent.DisplayMode` | `Disabled, Edit, View` | `Additional input properties for variant UrlEdit` |
| `TypedDataCard` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.Stretch` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Video` | `BorderStyle` | `BorderStyle` | `BorderStyle.Solid` | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Video` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Video` | `ImagePosition` | `ImagePosition` | `ImagePosition.Fill` | `Center, Fill, Fit, Stretch, Tile` | `Input Properties` |
| `Video` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |

---

## MixedReality family — 87 lines, 16 controls

| Control | Property | `enum_name_raw` | `identity_raw` | `values_raw` | Scope |
| --- | --- | --- | --- | --- | --- |
| `AddressInput` | `AddressDataSource` | `AddressInput.AddressDataSource` | — | `AzureMaps, BingMaps, Default` | `Input Properties` |
| `AddressInput` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `AddressInput` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `AddressInput` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `AddressInput` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `AddressInput` | `Language` | `AddressInput.Language` | — | `af-ZA, ar-SA, bg-BG, ca-ES, cs-CZ, da-DK, de-DE, el-GR, en-AU, en-GB, en-NZ, en-US, es-419, es-ES, et-EE, eu-ES, fi-FI, fr-CA, fr-FR, gl-ES, he-IL, hr-HR, hu-HU, id-ID, it-IT, kk-KZ, lt-LT, lv-LV, ms-MY, nb-NO, nl-BE, nl-NL, pl-PL, pt-BR, pt-PT, ro-RO, ru-RU, sk-SK, sl-SL, sr-RS, sv-SE, th-TH, tr-TR, uk-UA, vi-VN, zh-CN, zh-TW` | `Input Properties` |
| `AddressInput` | `TextAlignment` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `AddressInput` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `AnnotateInMR` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `AnnotateInMR` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `AnnotateInMR` | `DisplayType` | `AnnotateInMR.DisplayType` | — | `MSR_Icon, MSR_IconText, MSR_Text` | `Input Properties` |
| `AnnotateInMR` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `AnnotateInMR` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `AnnotateInMR` | `TextAlignment` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `AnnotateInMR` | `VerticalAlign` | `VerticalAlign` | — | `Bottom, Middle, Top` | `Input Properties` |
| `AnnotateInMR` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `BarcodeReader` | `BarcodeType` | `BarcodeReader.BarcodeType` | — | `Auto, Aztec, Codabar, Code128, Code39, Code93, DataMatrix, Ean, I2of5, MicroQRCode, Pdf417, QRCode, Rss14, RssExpanded, Upc` | `Input Properties` |
| `BarcodeReader` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `BarcodeReader` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `BarcodeReader` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `BarcodeReader` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `BarcodeReader` | `ScanningMode` | `BarcodeReader.ScanningMode` | — | `AutomaticallyScan, ScanInline, ScanMultiple, SelectToScan` | `Input Properties` |
| `BarcodeReader` | `ScanningQuality` | `BarcodeReader.ScanningQuality` | — | `Automatic, High, Low, Medium, VeryHigh` | `Input Properties` |
| `BarcodeReader` | `TextAlignment` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `BarcodeReader` | `VerticalAlign` | `VerticalAlign` | — | `Bottom, Middle, Top` | `Input Properties` |
| `BarcodeReader` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `BusinessCardReader` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `BusinessCardReader` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `BusinessCardReader` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Chatbot` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Chatbot` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Copilot` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Copilot` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `CopilotAnswer` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `CopilotAnswer` | `LoadingMode` | `CopilotAnswer.LoadingMode` | — | `Eager, Lazy` | `Input Properties` |
| `CopilotAnswer` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `FormProcessor` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `FormProcessor` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `FormProcessor` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `Map` | `AddressDataSource` | `Map.AddressDataSource` | — | `AzureMaps, BingMaps, Default` | `Input Properties` |
| `Map` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `Map` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `Map` | `InfoCards` | `Map.InfoCards` | — | `None, OnClick, OnHover` | `Input Properties` |
| `Map` | `MapStyle` | `Map.MapStyle` | — | `GrayscaleDark, GrayscaleLight, HighContrastDark, HighContrastLight, Night, Road, RoadShadedRelief, Satellite, SatelliteRoadLabels` | `Input Properties` |
| `Map` | `RouteOptimization` | `Map.RouteOptimization` | — | `Distance, None, Time` | `Input Properties` |
| `Map` | `RouteTravelMode` | `Map.RouteTravelMode` | — | `Car, Truck` | `Input Properties` |
| `Map` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `MeasureInMR` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `MeasureInMR` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `MeasureInMR` | `DisplayType` | `MeasureInMR.DisplayType` | — | `MSR_Icon, MSR_IconText, MSR_Text` | `Input Properties` |
| `MeasureInMR` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `MeasureInMR` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `MeasureInMR` | `MeasurementType` | `MeasureInMR.MeasurementType` | — | `Area, Distance, Freeform, Volume` | `Input Properties` |
| `MeasureInMR` | `TextAlignment` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `MeasureInMR` | `Units` | `MeasureInMR.Units` | — | `MSR_Centimeters, MSR_Feet, MSR_Inches, MSR_Meters` | `Input Properties` |
| `MeasureInMR` | `VerticalAlign` | `VerticalAlign` | — | `Bottom, Middle, Top` | `Input Properties` |
| `MeasureInMR` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `ObjectDetector` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `ObjectDetector` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `ObjectDetector` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `ReceiptProcessor` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `ReceiptProcessor` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `ReceiptProcessor` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `TextRecognizer` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `TextRecognizer` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `TextRecognizer` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `ViewIn3D` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `ViewIn3D` | `Units` | `ViewIn3D.Units` | — | `3DViewer_Centimeters, 3DViewer_Feet, 3DViewer_Inches, 3DViewer_Meters` | `Input Properties` |
| `ViewIn3D` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `ViewInMR` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `ViewInMR` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `ViewInMR` | `DisplayType` | `ViewInMR.DisplayType` | — | `ARV_Icon, ARV_IconText, ARV_Text` | `Input Properties` |
| `ViewInMR` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `ViewInMR` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `ViewInMR` | `TextAlignment` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `ViewInMR` | `Units` | `ViewInMR.Units` | — | `AR_Centimeters, AR_Feet, AR_Inches, AR_Meters` | `Input Properties` |
| `ViewInMR` | `VerticalAlign` | `VerticalAlign` | — | `Bottom, Middle, Top` | `Input Properties` |
| `ViewInMR` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |
| `ViewShapeInMR` | `BorderStyle` | `BorderStyle` | — | `Dashed, Dotted, None, Solid` | `Input Properties` |
| `ViewShapeInMR` | `DisplayMode` | `DisplayMode` | `DisplayMode.Edit` | `Disabled, Edit, View` | `Input Properties` |
| `ViewShapeInMR` | `DisplayType` | `ViewShapeInMR.DisplayType` | — | `SHP_Icon, SHP_IconText, SHP_Text` | `Input Properties` |
| `ViewShapeInMR` | `Font` | `Font` | — | `Arial, Courier New, Dancing Script, Georgia, Great Vibes, Lato, Lato Black, Lato Hairline, Lato Light, Open Sans, Open Sans Condensed, Patrick Hand, Segoe UI, Verdana` | `Input Properties` |
| `ViewShapeInMR` | `FontWeight` | `FontWeight` | — | `Bold, Lighter, Normal, Semibold` | `Input Properties` |
| `ViewShapeInMR` | `TextAlignment` | `Align` | — | `Center, Justify, Left, Right` | `Input Properties` |
| `ViewShapeInMR` | `Units` | `ViewShapeInMR.Units` | — | `SHP_Centimeters, SHP_Feet, SHP_Inches, SHP_Meters` | `Input Properties` |
| `ViewShapeInMR` | `VerticalAlign` | `VerticalAlign` | — | `Bottom, Middle, Top` | `Input Properties` |
| `ViewShapeInMR` | `AlignInContainer` | `AlignInContainer` | `AlignInContainer.SetByContainer` | `Center, End, SetByContainer, Start, Stretch` | `Additional input properties if a child of a Vertical or Horizontal GroupContainer` |

