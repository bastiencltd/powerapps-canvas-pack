---
ontology: true
type: reference
domain: powerapps-yaml
summary: Reference of the 117 Power Apps .pa.yaml controls by family, with creation keyword, Variant/Layout requirement, containment and property counters
tags: [powerapps, pa-yaml, controls, react, fluentv9, classic, mixedreality, reference]
status: active
verified: true
discovered: 2026-11-13
workspace: powerappsyaml
sources:
  - ../../01-controls-types.json
  - ../../02-properties-detail.json
  - ../../04-validation-rules.json
  - ../../05-patterns-templates.json
related:
  - enums-catalog
  - properties-matrix
  - validation-rules
  - ../SKILL
---

# Power Apps controls in YAML — per-family reference

117 standard controls, described one by one from the consolidated `describe_control` outputs. This file is generated mechanically (`gen-controls.js`, agent 6a) from `01-controls-types.json` (canonical order), `02-properties-detail.json` (everything else), `05-patterns-templates.json` ("Need → control" table) and `04-validation-rules.json` (reminder of the missing `Variant` error). No MCP call was redone here : **what is `null` in the source is written `—`**, nothing is inferred.

**Family order.** React first : it is the family that the only `do not use` directives of the output designate as the replacement (`Button` → `ModernButton`, `DataGrid` and `DataTable` → `ModernDataGrid` ; both replacements are of the React family). Then FluentV9, then Classic, then MixedReality. The 60 Classic controls carry **no** literal prohibition : the output merely states the family (`guidance = null`), so they are not forbidden, they are simply superseded by the `Modern*`/React equivalents for any new instance.

**One block per control.** Creation keywords copied at the literal (multi-line YAML block as soon as `Variant:` or `Layout:` is involved, including `# (required)` / `# (optional)` comments) ; literal variant list ; `Containment` without the guard sentence repeated 117 times, stated once here and valid everywhere : « Omitted restrictions do not imply unrestricted composition; flags, variants and compilation still apply. Mapped identities may be structural, not independently creatable controls. »

**Counters and conditional properties.** "inputs" = size of `inputs`, "outputs" = size of `outputs`, "enumerated properties" = number of keys of `enums` (the values are in `enums-catalog.md`). The counters deliberately merge nothing, because the conditional properties are not stored in the same place depending on the extraction batches :

- 25 controls carry them **in** `inputs`, with `description_raw` = the section's literal header (285 properties, of the type « Additional input properties if a child of a Vertical or Horizontal GroupContainer ») ;
- 39 controls carry them in `container_additional_inputs`, under two keys : `vertical_or_horizontal_group` (6 properties : `AlignInContainer`, `FillPortions`, `LayoutMax*`, `LayoutMin*`) and `grid_group` (4 `LayoutGrid*` properties) ;
- only `TypedDataCard` carries `variant_additional_inputs` : 124 variants, 1 093 properties, of which `Y` marked `required: true` on each of the 124 variants ;
- `GroupContainer`, `Gallery` and `ModernDataGridColumn` have « Additional input properties for variant … » sections in the output but **no separate capture** : only `raw_describe_output` contains them. Starting with `GroupContainer` in `AutoLayout`, whose `LayoutDirection` carries the `Required: true` line — nowhere to be found in `inputs`, therefore absent from the counters above.

**Mandatory nature.** `required: true` appears only 4 times in `inputs` : `Classic/Icon.Icon`, `DataCard.X`, `DataCard.Y`, `TypedDataCard.X`. Everywhere else the field is `null` because `describe_control` emits no `Required:` line (extraction instruction copied into the `extraction_notes`), not because the property would be optional.

**Versions.** No `creation_keyword` of the source carries an `@x.y.z` suffix : the question of control versions (`Label@2.5.1`, `TextInput@0.0.54`, version collisions) is handled by `validation-rules.md` and `04-validation-rules.json`, not here.

## Need → control

_Mapping built mechanically from `05-patterns-templates.json` : 28 patterns, fields `name`, `use_case` and `controls_involved` copied literally. Entries that are not controls (`Screen`, `App`, `Collection`, `Variable`, `Table()`) are kept as-is._

| Need (literal use_case) | Pattern (literal name) | Controls involved (literal `controls_involved`) |
| --- | --- | --- |
| Politique ResponsiveRoot: tout ecran genere pour telephone/tablette/unknown a un unique conteneur racine AutoLayout plein viewport et scrollable; les Children de l'ecran ne contiennent QUE cette racine. | `racine_ecran_responsive` | `GroupContainer (AutoLayout)` |
| ManualLayout pour positionnement de precision (jeux, dashboards fixes); les enfants se positionnent en X/Y/Width/Height; formule de position classique (size*index)+(spacing*index). | `conteneur_manuallayout_positionnement_absolu` | `GroupContainer (ManualLayout)` · `ModernButton` |
| AutoLayout pour designs flexibles et responsives: direction, alignement, justification, gap; les enfants prennent une part proportionnelle via FillPortions. | `conteneur_autolayout_flux_responsive` | `GroupContainer (AutoLayout)` · `ModernButton` |
| Gallery est un controle Classic sans variant AutoLayout: donner a la gallery un enfant unique GroupContainer AutoLayout dimensionne sur le template, et construire la ligne a l interieur en FillPortions (jamais X/Y). | `gallery_ligne_responsive_via_shell_autolayout` | `Gallery (Vertical)` · `GroupContainer (AutoLayout)` |
| Pour une liste de donnees, donner a la Gallery un Height numerique conservateur, un TemplateSize positif explicite et un TemplatePadding numerique; ne jamais auto-dimensionner via CountRows + Self.TemplateHeight/TemplateSize/TemplatePadding ni piloter l etat vide depuis Self.AllItems/AllItemsCount. | `gallery_viewport_borne` | `Gallery` |
| Gallery de navigation en sidebar: Pattern A (remplit la hauteur restante, par defaut, sans spacer) et Pattern B (hauteur exactement egale a ses items, compact sans scroll); FillPortions et Height sont mutuellement exclusifs sur une Gallery; ne jamais mettre un sidebarSpacer FillPortions: =1 a cote. | `gallery_navigation_sidebar_patternes_A_B` | `Gallery (Vertical)` · `GroupContainer (AutoLayout)` |
| Menu de navigation complete: items stockes dans une table inline Table() dans Items, surlignage de l item actif par variable CurrentMenuID, clic = Navigate vers l ecran stocke dans le champ PageNavigation de l enregistrement. | `gallery_comme_menu_navigation_table` | `Gallery (Vertical)` · `GroupContainer` · `Label` · `Image` · `Classic/Button` · `Rectangle` |
| Barre d onglets horizontale (Gallery Horizontal) qui bascule des sections de contenu sur le MEME ecran via la variable varActiveTab; chaque section met Visible contre varActiveTab; le clic se fait par un Classic/Button transparent dans le template. | `tab_bar_onglets_sous_ligne_meme_ecran` | `Gallery (Horizontal)` · `GroupContainer (ManualLayout)` · `Label` · `Rectangle` · `Classic/Button` |
| Galerie horizontale de cartes KPI auto-ajustees: TemplateSize divise la largeur par le nombre d items (-1 evite l ascenseur d arrondi), chaque carte remplit 90% du template. | `gallery_cartes_kpi_auto_fit` | `Gallery (Horizontal)` · `GroupContainer (AutoLayout)` · `Table() inline` |
| Tableau custom = rangee header statique + Gallery verticale; les colonnes s alignent grace a des valeurs FillPortions IDENTIQUES dans le header et le template; separateur de ligne epingle au bas du template via OnSelect: =Select(Parent). | `table_de_donnees_header_plus_gallery` | `Gallery (Vertical)` · `GroupContainer (AutoLayout)` · `Label` · `Rectangle` |
| Pastille de statut en pilule: BorderColor derive par Switch selon le statut, Fill toujours derive du BorderColor par ColorFade 80% (jamais les deux codes couleur en dur separement). | `badge_statut_switch_colorfade` | `GroupContainer (ManualLayout)` · `Label` |
| Groupe Container+Label+Image(SVG)+Classique/Button transparent est LE bouton standard de ce skill: le bouton capture les clics, le label et l icone donnent l apparence; GroupContainer n a pas d OnSelect. | `bouton_custom_overlay_transparent` | `GroupContainer (ManualLayout)` · `Label` · `Image` · `Classic/Button` |
| Tout GroupContainer contenant un bouton overlay transparent DOIT etre ManualLayout (en AutoLayout l overlay consomme toute la hauteur et rend les freres invisibles); l overlay est place EN DERNIER dans Children (dernier = au-dessus). | `overlay_bouton_impose_manuallayout` | `GroupContainer (ManualLayout)` · `Label` · `Rectangle` · `Classic/Button` |
| Header/pied fixes + corps qui scrolle: header et footer en FillPortions: =0 dans un parent AutoLayout Vertical, le corps est un GroupContainer FillPortions: =1 avec LayoutOverflowY: =LayoutOverflow.Scroll; le scroll container ne doit JAMAIS avoir un enfant direct en FillPortions: =1 (sinon le scroll ne s active jamais). | `corps_scrollable_header_fixe` | `GroupContainer (AutoLayout)` |
| Donner des coins arrondis a un controle sans proprietes Radius (Classic/ComboBox, Classic/DatePicker, Classic/Radio, Classic/CheckBox, Label, Rectangle, TextInput@0.0.54...): l envelopper dans un GroupContainer porteur du radius et de la bordure, border interne retire pour eviter la double bordure. | `radius_wrapper_conteneur_arrondi` | `GroupContainer (AutoLayout)` · `Classic/ComboBox` |
| Field group AutoLayout vertical label+input: hauteur explicite = somme des hauteurs des enfants + gaps + padding + bordures rendues hors Height (2 x Max(BorderThickness, FocusedBorderThickness)); le Label doit avoir AutoHeight: =true; sinon PA Studio met 200px par defaut. | `champ_groupe_label_input_hauteur_calculee` | `GroupContainer (AutoLayout)` · `Label` · `Classic/TextInput` |
| Sidebar repliable pilotee par UNE seule booleenne varMenuExpanded initialisee dans OnVisible; largeur, visibilite des labels, alignement et logo derivent tous de cette variable. | `sidebar_repliable_variable_unique` | `GroupContainer (AutoLayout)` · `Gallery` · `Label` · `Image` · `Classic/Button` |
| Grille de cartes en GroupContainer Variant GridLayout: colonnes derivees de App.Width (640/1024), les 4 axes (LayoutGridColumns, LayoutGridRows, LayoutGridColumnMinWidth, LayoutGridRowMinHeight) sets ensemble avec UNE seule expression de colonnes, Height derive des rangees: rows * h + Max(rows - 1, 0) * g + pt + pb. | `grille_de_cartes_gridlayout_responsive` | `GroupContainer (GridLayout)` · `ModernCard` |
| Rangee d actions type dialogue: Cancel a gauche (FillPortions: =0, largeur fixe), groupe de boutons a droite qui remplit l espace restant (FillPortions: =1, LayoutJustifyContent.End) dans un parent SpaceBetween. | `rangee_actions_cancel_gauche_droite_spacebetween` | `GroupContainer (AutoLayout)` · `Classic/Button ou ModernButton` |
| Cercle parfait (avatar, photo de profil): Height = Width (carre), les 4 rayons = largeur / 2, AlignInContainer: Center explicite (le defaut Stretch casse le ratio cercle -> ovale). | `avatar_cercle_rayon_moitie` | `GroupContainer (ManualLayout)` · `Image` |
| Barre de progression sans controle natif: deux GroupContainer ManualLayout imbriques, le remplissage a la largeur proportionnelle a la valeur/total. | `barre_de_progress_custom_deux_conteneurs` | `GroupContainer (ManualLayout)` |
| En CREATE, le premier ecran reel de l app s ecrit dans Screen1.pa.yaml (toujours present dans le workspace de depart) en gardant la cle Screen1; App.StartScreen: =Screen1; ne jamais creer Home.pa.yaml en laissant Screen1 vide, ne jamais Navigate depuis App.OnStart ni l OnVisible de l ecran d arrivee. | `ecran1_reutilise_premier_ecran` | `Screen` · `GroupContainer (AutoLayout)` |
| Logique reutilisable dans App.Formulas: constantes nommees et fonctions utilisateur partagees entre ecrans et controles. | `formules_nommees_app_formulas` | `App` |
| Donnees mock initialisees dans App.OnStart avec ClearCollect (5-8 rangees, descriptions courtes); pour une collection typed mais vide au depart: creer un enregistrement typed puis Clear immediatement; ne JAMAIS ClearCollect des donnees fixes dans l OnVisible d un ecran. | `collections_de_demo_app_onstart` | `App` |
| Controle Form connecte a une source de donnees (le seul trace des sources pour la combinaison Form + Submit/Reset: le tableau de choix des controles et l inventaire des controles; aucune squelette YAML litteral de Form ni formule SubmitForm()/ResetForm() dans les fichiers lus). | `formulaire_form_lie_source` | `Form (DataSource, Item, OnSuccess)` · `FormViewer` |
| Format racine d un ecran: cle top-level Screens:, Properties avec Fill et OnVisible (ClearCollect), conteneur main AutoLayout fills Parent.Width/Height; 'Control: Screen' n est PAS valide (PA2101). | `format_yml_ecran_complet` | `Screen` · `GroupContainer (AutoLayout)` |
| Pour 6 choix statiques ou moins, preferer des boutons de choix visibles ou des radio (le filtre actif reste visible, pas d etat dropdown non committe); les boutons de categorie ecrivent varCategoryFilter directement et la Gallery lit un Filter avec court-circuit 'All'. | `controles_choix_courts_boutons_visibles` | `Gallery` · `ModernRadio ou boutons` · `Variable varCategoryFilter` |
| Pattern canonique mutation collection locale + recu visible: capturer l enregistrement retourne (resultat de Patch) AVANT reset/navigation, afficher un recu lie a cette capture, reinitialiser les saisies seulement apres succes (Feedback: Receipt). | `cycle_mutation_avec_recu` | `Bouton (OnSelect)` · `Gallery` · `Collection colRecords` · `Variables varLastRecord/varLastAction/varReceiptVisible` |

## Families

_Adoption priority order : React first (recommended for any new control), then FluentV9, Classic, MixedReality. Headcounts computed from the `family` field of `02-properties-detail.json`._

| Family | Controls described |
| --- | --- |
| React | 31 |
| FluentV9 | 10 |
| Classic | 60 |
| MixedReality | 16 |
| **Total** | **117** |

## Mandatory keywords at creation

### `Variant:` keyword required at creation

_List derived from the literal `# (required)` / `# (optional)` text of the `creation_keyword` field (and from the auxiliary field `variant_requirement_raw` for the three controls that carry it). Error documented in `04-validation-rules.json` (`error_codes` code `Variant manquant (mot-cle obligatoire)`, probe p4b-container-no-variant) : `Screen1.pa.yaml(6,9): error: The keyword 'Variant' is required but is missing or empty. (sonde p4b-container-no-variant)`_

`DataTableColumn`, `Gallery`, `GroupContainer`, `PartialCircle`, `Star`, `TypedDataCard`

### Optional `Variant:` keyword (or variant reported outside the creation keyword)

`ModernDataGridColumn (variante signalee hors mot-cle de creation)`, `Arrow (variante signalee hors mot-cle de creation)`, `DataCard`, `Form`, `Triangle`

### `Layout:` keyword required at creation

`Form`, `FormViewer`

---

## React family — 31 controls

### DataWedgeScanner

- **Family :** React
- **YAML creation keyword :** `Control: DataWedgeScanner`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 25 · outputs 20 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### Geolocation

- **Family :** React
- **YAML creation keyword :** `Control: Geolocation`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 26 · outputs 23 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### ModernAttachments

- **Family :** React
- **YAML creation keyword :** `Control: ModernAttachments`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 47 · outputs 31 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DropShadow`, `Font`, `AlignInContainer`

### ModernAvatar

- **Family :** React
- **YAML creation keyword :** `Control: ModernAvatar`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 45 · outputs 29 · enumerated properties 8
- **Enumerated properties (values in enums-catalog.md) :** `Appearance`, `Badge`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Shape`, `AlignInContainer`

### ModernAvatarGroup

- **Family :** React
- **YAML creation keyword :** `Control: ModernAvatarGroup`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 50 · outputs 22 · enumerated properties 7
- **Enumerated properties (values in enums-catalog.md) :** `Appearance`, `BorderStyle`, `DisplayMode`, `Layout`, `OverflowIndicator`, `Shape`, `AlignInContainer`

### ModernBadge

- **Family :** React
- **YAML creation keyword :** `Control: ModernBadge`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 48 · outputs 31 · enumerated properties 11
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `IconPosition`, `Intent`, `Shape`, `VerticalAlign`, `AlignInContainer`

### ModernButton

- **Family :** React
- **YAML creation keyword :** `Control: ModernButton`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 47 · outputs 31 · enumerated properties 10
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `IconStyle`, `Layout`, `VerticalAlign`, `AlignInContainer`

### ModernCard

- **Family :** React
- **YAML creation keyword :** `Control: ModernCard`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 39 · outputs 28 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `DropShadow`, `ImagePlacement`, `ImagePosition`, `LayoutDirection`, `AlignInContainer`

### ModernCardGallery

- **Family :** React
- **YAML creation keyword :** `Control: ModernCardGallery`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 64 · outputs 24 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `CardDropShadow`, `CardImagePlacement`, `CardImagePosition`, `CardLayoutDirection`, `DisplayMode`, `AlignInContainer`

### ModernCheckbox

- **Family :** React
- **YAML creation keyword :** `Control: ModernCheckbox`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 32 · outputs 20 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `Font`, `FontWeight`, `AlignInContainer`

### ModernCombobox

- **Family :** React
- **YAML creation keyword :** `Control: ModernCombobox`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 51 · outputs 35 · enumerated properties 7
- **Enumerated properties (values in enums-catalog.md) :** `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `ValidationState`, `AlignInContainer`

### ModernDataGrid

- **Family :** React
- **YAML creation keyword :** `Control: ModernDataGrid`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: ModernDataGridColumn
- **Counts:** inputs 49 · outputs 31 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DropShadow`, `Font`, `Pagination`, `AlignInContainer`

### ModernDataGridColumn

- **Family :** React
- **YAML creation keyword :** `Control: ModernDataGridColumn`
- **`Variant:` keyword:** not mentioned in the creation keyword (variant list reported elsewhere in the output)
- **Variants (literal list from the output) :** `Boolean`, `Button`, `ComplexOptionSet`, `ComplexType`, `Currency`, `DateTime`, `Email`, `Image`, `Number`, `OptionSetArray`, `Phone`, `Textual`, `Url`
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
  - Supported immediate parents: ModernDataGrid
- **Counts:** inputs 51 · outputs 4 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `DisplayType`, `Appearance`, `AlignInContainer`
- **Extraction note (literal) :** Variantes capturees dans variants a partir de la liste litterale 'The Variant keyword is required...' du raw; le mot-cle Variant est requis a la creation pour ce controle (voir raw_describe_output, bloc Control creation keywords).

### ModernDatePicker

- **Family :** React
- **YAML creation keyword :** `Control: ModernDatePicker`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 49 · outputs 32 · enumerated properties 10
- **Enumerated properties (values in enums-catalog.md) :** `Appearance`, `BorderStyle`, `DateTimeZone`, `DisplayMode`, `Font`, `FontWeight`, `Format`, `StartOfWeek`, `ValidationState`, `AlignInContainer`

### ModernDropdown

- **Family :** React
- **YAML creation keyword :** `Control: ModernDropdown`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 45 · outputs 31 · enumerated properties 7
- **Enumerated properties (values in enums-catalog.md) :** `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `ValidationState`, `AlignInContainer`

### ModernIcon

- **Family :** React
- **YAML creation keyword :** `Control: ModernIcon`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 37 · outputs 22 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `IconStyle`, `AlignInContainer`

### ModernInformationButton

- **Family :** React
- **YAML creation keyword :** `Control: ModernInformationButton`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 33 · outputs 20 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `Font`, `FontWeight`, `AlignInContainer`

### ModernLink

- **Family :** React
- **YAML creation keyword :** `Control: ModernLink`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 45 · outputs 35 · enumerated properties 8
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Type`, `VerticalAlign`, `AlignInContainer`

### ModernNumberInput

- **Family :** React
- **YAML creation keyword :** `Control: ModernNumberInput`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 48 · outputs 37 · enumerated properties 9
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Precision`, `ValidationState`, `AlignInContainer`

### ModernPersona

- **Family :** React
- **YAML creation keyword :** `Control: ModernPersona`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 58 · outputs 31 · enumerated properties 12
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Presence`, `Shape`, `TextAlignment`, `TextPosition`, `VerticalAlign`, `AlignInContainer`

### ModernProgressBar

- **Family :** React
- **YAML creation keyword :** `Control: ModernProgressBar`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 28 · outputs 11 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `ProgressColor`, `Shape`, `Thickness`, `AlignInContainer`

### ModernRadio

- **Family :** React
- **YAML creation keyword :** `Control: ModernRadio`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 50 · outputs 36 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Layout`, `AlignInContainer`

### ModernRating

- **Family :** React
- **YAML creation keyword :** `Control: ModernRating`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 25 · outputs 11 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `Step`, `AlignInContainer`

### ModernSlider

- **Family :** React
- **YAML creation keyword :** `Control: ModernSlider`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 26 · outputs 12 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `LayoutDirection`, `Size`, `AlignInContainer`

### ModernSpinner

- **Family :** React
- **YAML creation keyword :** `Control: ModernSpinner`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 37 · outputs 27 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `Appearance`, `DisplayMode`, `Font`, `FontWeight`, `LabelPosition`, `AlignInContainer`

### ModernTabList

- **Family :** React
- **YAML creation keyword :** `Control: ModernTabList`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 39 · outputs 25 · enumerated properties 8
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Alignment`, `Appearance`, `DisplayMode`, `Font`, `FontWeight`, `TabSize`, `AlignInContainer`

### ModernText

- **Family :** React
- **YAML creation keyword :** `Control: ModernText`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 43 · outputs 32 · enumerated properties 7
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`, `AlignInContainer`

### ModernTextInput

- **Family :** React
- **YAML creation keyword :** `Control: ModernTextInput`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 48 · outputs 37 · enumerated properties 10
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `TriggerOutput`, `Type`, `ValidationState`, `AlignInContainer`

### ModernToggle

- **Family :** React
- **YAML creation keyword :** `Control: ModernToggle`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 33 · outputs 21 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `Font`, `FontWeight`, `LabelPosition`, `AlignInContainer`

### NativePdfViewer

- **Family :** React
- **YAML creation keyword :** `Control: NativePdfViewer`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 25 · outputs 13 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### NativePenInput

- **Family :** React
- **YAML creation keyword :** `Control: NativePenInput`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 23 · outputs 12 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

---

## FluentV9 family — 10 controls

### Breadcrumb

- **Family :** FluentV9
- **YAML creation keyword :** `Control: Breadcrumb`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: PowerApps_CoreControls_BreadcrumbTemplate_dataField
- **Counts:** inputs 12 · outputs 9 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### Button

- **Family :** FluentV9
- **Guidance do-not-use (literal) :** Do not use Button unless it's already present in this app. Use the updated ModernButton instead for any new instances in this app.
- **YAML creation keyword :** `Control: Button`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 38 · outputs 10 · enumerated properties 10 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `Appearance`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `IconStyle`, `Layout`, `VerticalAlign`, `AlignInContainer`
- **Deprecated / forbidden (literal) :** Do not use Button unless it's already present in this app. Use the updated ModernButton instead for any new instances in this app.

### Header

- **Family :** FluentV9
- **YAML creation keyword :** `Control: Header`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 38 · outputs 9 · enumerated properties 6 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `Style`, `AlignInContainer`

### InteractiveText

- **Family :** FluentV9
- **YAML creation keyword :** `Control: InteractiveText`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 34 · outputs 8 · enumerated properties 7 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`, `AlignInContainer`

### IoTDeviceManager

- **Family :** FluentV9
- **YAML creation keyword :** `Control: IoTDeviceManager`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 42 · outputs 14 · enumerated properties 4 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `Font`, `Layout`, `AlignInContainer`

### PowerVirtualAgentsChat

- **Family :** FluentV9
- **YAML creation keyword :** `Control: PowerVirtualAgentsChat`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 7 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### SpinButton

- **Family :** FluentV9
- **YAML creation keyword :** `Control: SpinButton`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 17 · outputs 8 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### StreamVideo

- **Family :** FluentV9
- **YAML creation keyword :** `Control: StreamVideo`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 9 · outputs 6 · enumerated properties 1 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `AlignInContainer`

### Switch

- **Family :** FluentV9
- **YAML creation keyword :** `Control: Switch`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 15 · outputs 8 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `LabelPosition`, `AlignInContainer`

### Toolbar

- **Family :** FluentV9
- **YAML creation keyword :** `Control: Toolbar`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: PowerApps_CoreControls_ToolbarTemplate_dataField
- **Counts:** inputs 30 · outputs 17 · enumerated properties 7 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `Alignment`, `DisplayMode`, `Font`, `FontWeight`, `Layout`, `Padding`, `AlignInContainer`

---

## Classic family — 60 controls

### AddMedia

- **Family :** Classic
- **YAML creation keyword :** `Control: AddMedia`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 51 · outputs 40 · enumerated properties 7
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`, `AlignInContainer`

### Arrow

- **Family :** Classic
- **YAML creation keyword :** `Control: Arrow`
- **`Variant:` keyword:** not mentioned in the creation keyword (variant list reported elsewhere in the output)
- **Variants (literal list from the output) :** `BackArrow`
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 33 · outputs 22 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`
- **Extraction note (literal) :** Section Variant presentee dans la sortie : The `Variant` keyword is optional and can be one of the following: - `BackArrow` (un seul variant, variante optionnelle).
- **Extraction note (literal) :** Le bloc YAML de creation affiche aussi la ligne 'Variant: <variant name> # (optional) see below for the list of variants' ; elle est conservee dans raw_describe_output, pas dans creation_keyword.

### Attachments

- **Family :** Classic
- **YAML creation keyword :** `Control: Attachments`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 80 · outputs 64 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DropTargetBorderStyle`, `Font`, `FontWeight`, `AlignInContainer`

### Audio

- **Family :** Classic
- **YAML creation keyword :** `Control: Audio`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 38 · outputs 23 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `ImagePosition`, `AlignInContainer`

### BarChart

- **Family :** Classic
- **YAML creation keyword :** `Control: BarChart`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 45 · outputs 35 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `AlignInContainer`, `BorderStyle`, `DisplayMode`, `Font`, `GridStyle`

### Barcode

- **Family :** Classic
- **YAML creation keyword :** `Control: Barcode`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
  - Supported immediate parents: Screen, unresolved internal template: component
- **Counts:** inputs 26 · outputs 17 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `AlignInContainer`, `BarcodeType`, `BorderStyle`, `DisplayMode`

### BarcodeScanner

- **Family :** Classic
- **YAML creation keyword :** `Control: BarcodeScanner`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 59 · outputs 45 · enumerated properties 8
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `AlignInContainer`, `BarcodeType`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`

### Camera

- **Family :** Classic
- **YAML creation keyword :** `Control: Camera`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 29 · outputs 20 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `AlignInContainer`, `BorderStyle`, `DisplayMode`

### Circle

- **Family :** Classic
- **YAML creation keyword :** `Control: Circle`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### Classic/Button

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/Button`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 44 · outputs 44 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`

### Classic/CheckBox

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/CheckBox`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 46 · outputs 42 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`

### Classic/ComboBox

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/ComboBox`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 59 · outputs 50 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`

### Classic/DatePicker

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/DatePicker`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 42 · outputs 35 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DateTimeZone`, `DisplayMode`, `Font`, `FontWeight`, `StartOfWeek`

### Classic/DropDown

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/DropDown`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 50 · outputs 46 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`

### Classic/Icon

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/Icon`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 34 · outputs 33 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Icon`
- **Input property(ies) marked `required: true` :** `Icon` (Enum, default `Icon.ArrowDown`)

### Classic/Radio

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/Radio`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 46 · outputs 44 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Layout`

### Classic/Slider

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/Slider`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 35 · outputs 32 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Layout`

### Classic/StreamVideo

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/StreamVideo`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 17 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### Classic/TextInput

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/TextInput`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 54 · outputs 49 · enumerated properties 8
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Format`, `Mode`, `VirtualKeyboardMode`

### Classic/Toggle

- **Family :** Classic
- **YAML creation keyword :** `Control: Classic/Toggle`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 43 · outputs 38 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `TextPosition`

### DataCard

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: DataCard
  Variant: <variant name> # (optional) see below for the list of variants
  ```

- **`Variant:` keyword:** optional at creation
- **Variants (literal list from the output) :** `BlankCard`
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: True
  - Supported immediate parents: FluidGrid, Form, FormViewer
- **Counts:** inputs 13 · outputs 13 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`
- **Input property(ies) marked `required: true` :** `X` (Number, default `0`), `Y` (Number, default `0`)
- **Extraction note (literal) :** Variants : un seul nom cite ('BlankCard'), mot-cle Variant declare facultatif ; le mot-cle Variant n'a pas de valeur par defaut indiquee

### DataGrid

- **Family :** Classic
- **Guidance do-not-use (literal) :** Do not use DataGrid unless it's already present in this app. Use the updated ModernDataGrid instead for any new instances in this app.
- **YAML creation keyword :** `Control: DataGrid`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate parents: DataCard, Screen, TypedDataCard
  - Supported immediate children: unresolved internal template: customControl
- **Counts:** inputs 25 · outputs 25 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `Font`, `FontWeight`, `HeadingFont`, `HeadingFontWeight`
- **Deprecated / forbidden (literal) :** Do not use DataGrid unless it's already present in this app. Use the updated ModernDataGrid instead for any new instances in this app.

### DataTable

- **Family :** Classic
- **Guidance do-not-use (literal) :** Do not use DataTable unless it's already present in this app. Use the updated ModernDataGrid instead for any new instances in this app.
- **YAML creation keyword :** `Control: DataTable`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate parents: DataCard, GroupContainer, Screen, TypedDataCard
  - Supported immediate children: DataTableColumn
- **Counts:** inputs 25 · outputs 25 · enumerated properties 5
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `Font`, `FontWeight`, `HeadingFont`, `HeadingFontWeight`
- **Deprecated / forbidden (literal) :** Do not use DataTable unless it's already present in this app. Use the updated ModernDataGrid instead for any new instances in this app.

### DataTableColumn

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: DataTableColumn
  Variant: <variant name> # (required) see below for the list of variants
  ```

- **`Variant:` keyword:** REQUIRED at creation
- **Variants (literal list from the output) :** `Boolean`, `ComplexOptionSet`, `ComplexType`, `Currency`, `DateTime`, `Email`, `Number`, `Phone`, `Textual`, `Url`
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
- **Counts:** inputs 10 · outputs 7 · enumerated properties 0
- **Extraction note (literal) :** Aucune ligne 'Required:' dans Input Properties : required null pour toutes les entrees, meme si le mot-cle Variant est declare obligatoire ('Variant: <variant name> # (required)')

### EntityForm

- **Family :** Classic
- **YAML creation keyword :** `Control: EntityForm`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
- **Counts:** inputs 26 · outputs 23 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `Font`, `FontWeight`, `Pattern`

### Export

- **Family :** Classic
- **YAML creation keyword :** `Control: Export`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 41 · outputs 40 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`

### FluidGrid

- **Family :** Classic
- **YAML creation keyword :** `Control: FluidGrid`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: True
  - Supported immediate children: DataCard
- **Counts:** inputs 13 · outputs 13 · enumerated properties 1
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`

### Form

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: Form
  Variant: <variant name> # (optional) see below for the list of variants
  Layout: <layout name> # (required) one of: `Vertical`, `Horizontal`
  ```

- **`Variant:` keyword:** optional at creation
- **`Layout:` keyword:** REQUIRED at creation (literal line visible in the creation block above)
- **Variants (literal list from the output) :** `Classic`, `Modern`
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=True
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate parents: GroupContainer, Screen
  - Supported immediate children: DataCard, TypedDataCard
  - Unsupported descendants: unresolved internal template: 517e04dd5d6b47d482fa85e799cf3454
- **Counts:** inputs 21 · outputs 21 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DefaultMode`
- **Extraction note (literal) :** Le mot-cle Variant est facultatif, valeurs possibles Classic ou Modern ; aucune variante par defaut indiquee

### FormsPro

- **Family :** Classic
- **YAML creation keyword :** `Control: FormsPro`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
  - Supported immediate parents: Screen, unresolved internal template: component
- **Counts:** inputs 13 · outputs 12 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### FormViewer

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: FormViewer
  Layout: <layout name> # (required) one of: `Vertical`, `Horizontal`
  ```

- **`Layout:` keyword:** REQUIRED at creation (literal line visible in the creation block above)
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=True
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate parents: GroupContainer, Screen
  - Supported immediate children: DataCard, TypedDataCard
- **Counts:** inputs 17 · outputs 14 · enumerated properties 1
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`
- **Extraction note (literal) :** Pas de section 'Variant' dans la sortie : variants [] ; aucun mot-cle Variant accepte ni requis n'est indique

### Gallery

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: Gallery
  Variant: <variant name> # (required) see below for the list of variants
  ```

- **`Variant:` keyword:** REQUIRED at creation
- **Variants (literal list from the output) :** `Horizontal`, `VariableHeight`, `Vertical`
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=True
  - YAML children supported: True; Canvas Component descendants supported: False
  - Unsupported descendants: unresolved internal template: 517e04dd5d6b47d482fa85e799cf3454
- **Counts:** inputs 14 · outputs 23 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Transition`
- **Extraction note (literal) :** Aucune ligne 'Required:' dans Input Properties : required null partout ; le mot-cle Variant est lui declare obligatoire

### GroupContainer

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: GroupContainer
  Variant: <variant name> # (required) see below for the list of variants
  ```

- **`Variant:` keyword:** REQUIRED at creation
- **Variants (literal list from the output) :** `AutoLayout`, `GridLayout`, `ManualLayout`
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: True
- **Counts:** inputs 16 · outputs 25 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DropShadow`, `LayoutAlignItems`, `LayoutDirection`, `LayoutJustifyContent`, `LayoutOverflow`

### Hexagon

- **Family :** Classic
- **YAML creation keyword :** `Control: Hexagon`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### HtmlViewer

- **Family :** Classic
- **YAML creation keyword :** `Control: HtmlViewer`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 25 · outputs 24 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`

### Image

- **Family :** Classic
- **YAML creation keyword :** `Control: Image`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 40 · outputs 42 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `ImagePosition`, `ImageRotation`

### Import

- **Family :** Classic
- **YAML creation keyword :** `Control: Import`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 40 · outputs 41 · enumerated properties 6
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`

### Label

- **Family :** Classic
- **YAML creation keyword :** `Control: Label`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 45 · outputs 44 · enumerated properties 9
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Live`, `Overflow`, `Role`, `VerticalAlign`

### Legend

- **Family :** Classic
- **YAML creation keyword :** `Control: Legend`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 22 · outputs 20 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`

### LineChart

- **Family :** Classic
- **YAML creation keyword :** `Control: LineChart`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 32 · outputs 31 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `GridStyle`

### ListBox

- **Family :** Classic
- **YAML creation keyword :** `Control: ListBox`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 48 · outputs 47 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`

### Lookup

- **Family :** Classic
- **YAML creation keyword :** `Control: Lookup`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 53 · outputs 49 · enumerated properties 4
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`

### Microphone

- **Family :** Classic
- **YAML creation keyword :** `Control: Microphone`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 33 · outputs 30 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `ImagePosition`

### Octagon

- **Family :** Classic
- **YAML creation keyword :** `Control: Octagon`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### PartialCircle

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: PartialCircle
  Variant: <variant name> # (required) see below for the list of variants
  ```

- **`Variant:` keyword:** REQUIRED at creation
- **Variants (literal list from the output) :** `HalfCircle`, `QuarterCircle`, `ThreeQuarterCircle`
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`
- **Extraction note (literal) :** Aucune ligne 'Required:' dans Input Properties : required null partout ; le mot-cle Variant est en revanche declare obligatoire avec 3 valeurs possibles

### PDFViewer

- **Family :** Classic
- **YAML creation keyword :** `Control: PDFViewer`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
  - Supported immediate parents: Screen, unresolved internal template: component
- **Counts:** inputs 29 · outputs 26 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### PenInput

- **Family :** Classic
- **YAML creation keyword :** `Control: PenInput`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 17 · enumerated properties 3
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Mode`

### Pentagon

- **Family :** Classic
- **YAML creation keyword :** `Control: Pentagon`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 2
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`

### PieChart

- **Family :** Classic
- **YAML creation keyword :** `Control: PieChart`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 25 · outputs 25 · enumerated properties 5 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `LabelPosition`, `AlignInContainer`

### PowerBI

- **Family :** Classic
- **YAML creation keyword :** `Control: PowerBI`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
  - Supported immediate parents: GroupContainer, Screen, unresolved internal template: component
- **Counts:** inputs 18 · outputs 12 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### Rating

- **Family :** Classic
- **YAML creation keyword :** `Control: Rating`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 23 · outputs 21 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### Rectangle

- **Family :** Classic
- **YAML creation keyword :** `Control: Rectangle`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### RichTextEditor

- **Family :** Classic
- **YAML creation keyword :** `Control: RichTextEditor`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
  - Supported immediate parents: DataCard, GroupContainer, Screen, TypedDataCard, unresolved internal template: component
- **Counts:** inputs 14 · outputs 13 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### SalesNavigatorAccount

- **Family :** Classic
- **YAML creation keyword :** `Control: SalesNavigatorAccount`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 9 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `AlignInContainer`

### SalesNavigatorLead

- **Family :** Classic
- **YAML creation keyword :** `Control: SalesNavigatorLead`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 21 · outputs 9 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `AlignInContainer`

### SpeechControl

- **Family :** Classic
- **YAML creation keyword :** `Control: SpeechControl`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 33 · outputs 33 · enumerated properties 5 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `Font`, `VerticalAlign`, `AlignInContainer`

### Star

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: Star
  Variant: <variant name> # (required) see below for the list of variants
  ```

- **`Variant:` keyword:** REQUIRED at creation
- **Variants (literal list from the output) :** `12Points`, `5Points`, `6Points`, `8Points`
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### Timer

- **Family :** Classic
- **YAML creation keyword :** `Control: Timer`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 51 · outputs 49 · enumerated properties 7 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `Align`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `VerticalAlign`, `AlignInContainer`

### Triangle

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: Triangle
  Variant: <variant name> # (optional) see below for the list of variants
  ```

- **`Variant:` keyword:** optional at creation
- **Variants (literal list from the output) :** `RightAngled`
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 20 · outputs 19 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### TypedDataCard

- **Family :** Classic
- **YAML creation keyword (multi-line block, literal) :**

  ```yaml
  Control: TypedDataCard
  Variant: <variant name> # (required) see below for the list of variants
  ```

- **`Variant:` keyword:** REQUIRED at creation
- **Variants (literal list from the output) :** `BlankCard`, `ClassicAllowedValuesNumberEdit`, `ClassicAllowedValuesStringEdit`, `ClassicAttachmentsEdit`, `ClassicAttachmentsView`, `ClassicBlankPolymorphicEdit`, `ClassicComboBoxEdit`, `ClassicComboBoxMultiSelectEdit`, `ClassicComboBoxMultiSelectView`, `ClassicComboBoxOptionSetMultiEdit`, `ClassicComboBoxOptionSetMultiView`, `ClassicComboBoxOptionSetSingleEdit`, `ClassicComboBoxOptionSetSingleView`, `ClassicComboBoxView`, `ClassicCurrencyEdit`, `ClassicDateEdit`, `ClassicDateTimeEdit`, `ClassicEmailView`, `ClassicFileEdit`, `ClassicFileView`, `ClassicImageView`, `ClassicLargeImageView`, `ClassicLargePicture`, `ClassicLookupEdit`, `ClassicLookupView`, `ClassicNotes`, `ClassicNumberEdit`, `ClassicPercentageView`, `ClassicPhoneView`, `ClassicPicture`, `ClassicRatingEdit`, `ClassicRatingView`, `ClassicRichTextEditorEdit`, `ClassicRichTextEditorView`, `ClassicSliderEdit`, `ClassicSmartEmailEdit`, `ClassicTextualEdit`, `ClassicTextualMultilineEdit`, `ClassicTextualView`, `ClassicTimeEdit`, `ClassicToggleEdit`, `ClassicToggleView`, `ClassicUrlEdit`, `ClassicUrlView`, `ClassicUSCurrencyView`, `ClassicYesNoRadioEdit`, `ComboBoxEdit`, `ComboBoxMultiSelectEdit`, `ComboBoxMultiSelectView`, `ComboBoxOptionSetMultiEdit`, `ComboBoxOptionSetMultiView`, `ComboBoxOptionSetSingleEdit`, `ComboBoxOptionSetSingleView`, `ComboBoxView`, `CurrencyEdit`, `DateEdit`, `DateTimeEdit`, `FileEdit`, `FileView`, `ImageView`, `LargeImageView`, `LargePicture`, `ModernAllowedValuesNumberEdit`, `ModernAllowedValuesStringEdit`, `ModernAttachmentsEdit`, `ModernAttachmentsView`, `ModernComboBoxEdit`, `ModernComboBoxMultiSelectEdit`, `ModernComboBoxMultiSelectView`, `ModernComboBoxOptionSetMultiEdit`, `ModernComboBoxOptionSetMultiView`, `ModernComboBoxOptionSetSingleEdit`, `ModernComboBoxOptionSetSingleView`, `ModernComboBoxView`, `ModernCurrencyEdit`, `ModernDateEdit`, `ModernDateTimeEdit`, `ModernEmailView`, `ModernNumberEdit`, `ModernPhoneView`, `ModernRatingEdit`, `ModernRatingView`, `ModernSliderEdit`, `ModernTextualEdit`, `ModernTextualMultilineEdit`, `ModernTextualView`, `ModernToggleEdit`, `ModernToggleView`, `ModernUrlEdit`, `ModernUrlView`, `ModernUSCurrencyView`, `ModernYesNoRadioEdit`, `NumberEdit`, `PcfCoreComboBoxEditCard`, `PcfCoreComboBoxMultiSelectEditCard`, `PcfCoreComboBoxMultiSelectViewCard`, `PcfCoreComboBoxOptionSetMultiEditCard`, `PcfCoreComboBoxOptionSetMultiViewCard`, `PcfCoreComboBoxOptionSetSingleEditCard`, `PcfCoreComboBoxOptionSetSingleViewCard`, `PcfCoreComboBoxViewCard`, `PcfCoreCurrencyEditCard`, `PcfCoreDateEditCard`, `PcfCoreDateTimeEditCard`, `PcfCoreNumberEditCard`, `PcfCoreRatingEditCard`, `PcfCoreRatingViewCard`, `PcfCoreSliderEditCard`, `PcfCoreTextualEditCard`, `PcfCoreTextualMultiLineEditCard`, `PcfCoreTextualViewCard`, `PcfCoreToggleEditCard`, `PcfCoreToggleViewCard`, `PcfCoreUrlEditCard`, `Picture`, `RatingEdit`, `RatingView`, `SliderEdit`, `TextualEdit`, `TextualMultilineEdit`, `TextualView`, `ToggleEdit`, `ToggleView`, `UrlEdit`
- **Containment (literal) :**
  - Declared nesting: supports=True, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: True
  - Supported immediate parents: Form, FormViewer
- **Counts:** inputs 8 · outputs 17 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4) · variant inputs (124 variants) 1093
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`
- **Input property(ies) marked `required: true` :** `X` (Number, default `0`)

### Video

- **Family :** Classic
- **YAML creation keyword :** `Control: Video`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 29 · outputs 24 · enumerated properties 4 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `ImagePosition`, `AlignInContainer`

---

## MixedReality family — 16 controls

### AddressInput

- **Family :** MixedReality
- **YAML creation keyword :** `Control: AddressInput`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 48 · outputs 26 · enumerated properties 8 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `AddressDataSource`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `Language`, `TextAlignment`, `AlignInContainer`

### AnnotateInMR

- **Family :** MixedReality
- **YAML creation keyword :** `Control: AnnotateInMR`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 41 · outputs 8 · enumerated properties 8 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DisplayType`, `Font`, `FontWeight`, `TextAlignment`, `VerticalAlign`, `AlignInContainer`

### BarcodeReader

- **Family :** MixedReality
- **YAML creation keyword :** `Control: BarcodeReader`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 47 · outputs 8 · enumerated properties 10 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BarcodeType`, `BorderStyle`, `DisplayMode`, `Font`, `FontWeight`, `ScanningMode`, `ScanningQuality`, `TextAlignment`, `VerticalAlign`, `AlignInContainer`

### BusinessCardReader

- **Family :** MixedReality
- **YAML creation keyword :** `Control: BusinessCardReader`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 22 · outputs 28 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### Chatbot

- **Family :** MixedReality
- **YAML creation keyword :** `Control: Chatbot`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 11 · outputs 12 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### Copilot

- **Family :** MixedReality
- **YAML creation keyword :** `Control: Copilot`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: CopilotTemplate_dataField
- **Counts:** inputs 13 · outputs 15 · enumerated properties 2 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `AlignInContainer`

### CopilotAnswer

- **Family :** MixedReality
- **YAML creation keyword :** `Control: CopilotAnswer`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: CopilotAnswerTemplate_dataField
- **Counts:** inputs 13 · outputs 10 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `LoadingMode`, `AlignInContainer`

### FormProcessor

- **Family :** MixedReality
- **YAML creation keyword :** `Control: FormProcessor`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 25 · outputs 13 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### Map

- **Family :** MixedReality
- **YAML creation keyword :** `Control: Map`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: MapTemplate_dataField
- **Counts:** inputs 65 · outputs 31 · enumerated properties 8 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `AddressDataSource`, `BorderStyle`, `DisplayMode`, `InfoCards`, `MapStyle`, `RouteOptimization`, `RouteTravelMode`, `AlignInContainer`

### MeasureInMR

- **Family :** MixedReality
- **YAML creation keyword :** `Control: MeasureInMR`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: MeasureInMRTemplate_dataField
- **Counts:** inputs 50 · outputs 14 · enumerated properties 10 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DisplayType`, `Font`, `FontWeight`, `MeasurementType`, `TextAlignment`, `Units`, `VerticalAlign`, `AlignInContainer`

### ObjectDetector

- **Family :** MixedReality
- **YAML creation keyword :** `Control: ObjectDetector`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 27 · outputs 11 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### ReceiptProcessor

- **Family :** MixedReality
- **YAML creation keyword :** `Control: ReceiptProcessor`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 23 · outputs 21 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### TextRecognizer

- **Family :** MixedReality
- **YAML creation keyword :** `Control: TextRecognizer`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 23 · outputs 12 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `AlignInContainer`

### ViewIn3D

- **Family :** MixedReality
- **YAML creation keyword :** `Control: ViewIn3D`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: ViewIn3DTemplate_dataField
- **Counts:** inputs 26 · outputs 19 · enumerated properties 3 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `DisplayMode`, `Units`, `AlignInContainer`

### ViewInMR

- **Family :** MixedReality
- **YAML creation keyword :** `Control: ViewInMR`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: True; Canvas Component descendants supported: False
  - Supported immediate children: unresolved internal template: ViewInMRTemplate_dataField
- **Counts:** inputs 58 · outputs 17 · enumerated properties 9 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DisplayType`, `Font`, `FontWeight`, `TextAlignment`, `Units`, `VerticalAlign`, `AlignInContainer`

### ViewShapeInMR

- **Family :** MixedReality
- **YAML creation keyword :** `Control: ViewShapeInMR`
- **Variants (literal list from the output) :** —
- **Containment (literal) :**
  - Declared nesting: supports=False, replicates=False
  - YAML children supported: False; Canvas Component descendants supported: False
- **Counts:** inputs 49 · outputs 8 · enumerated properties 9 · conditional container inputs (vertical_or_horizontal_group 6, grid_group 4)
- **Enumerated properties (values in enums-catalog.md) :** `BorderStyle`, `DisplayMode`, `DisplayType`, `Font`, `FontWeight`, `TextAlignment`, `Units`, `VerticalAlign`, `AlignInContainer`

