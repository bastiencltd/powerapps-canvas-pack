# Credits & provenance

## Third-party skills (vendored, own licenses preserved)

- **canvas-apps-ui-gen** — © Tolu Victor Sanwoolu, MIT license (full text kept at `skills/canvas-apps-ui-gen/LICENSE`).
  Upstream: [ToluVictor/canvas-apps-tools](https://github.com/ToluVictor/canvas-apps-tools), v1.0.2.
  This vendored copy comes from the Hermes power-platform skills collection, which carries a
  superset of the upstream reference (`controls-reference.md` adds 12 documented PA2108 traps);
  no other modification except this attribution line in the SKILL.md frontmatter.

## Reference sources used to build powerapps-canvas-yaml

The five reference files in `skills/powerapps-canvas-yaml/references/` are original extractions,
but their documentation citations point at:

- **Microsoft power-platform-skills** (MIT) — [microsoft/power-platform-skills](https://github.com/microsoft/power-platform-skills)
  (ControlGuide, YamlSyntax, PowerFxGuide, QAChecks, LayoutGuide and related plugin references), cited inline as `github:` prefixes.
- **Hermes power-platform skills collection** (upstream: ToluVictor/canvas-apps-tools), cited inline as `hermes:` prefixes.

## Machine data provenance

- All control/property/enum data was extracted live through the **Microsoft Canvas Authoring MCP server**
  (`Microsoft.PowerApps.CanvasAuthoring.McpServer`, run via `dnx` on .NET 10) on **2026-11-13**, from a single
  audited canvas app session. No values were inferred: anything the service did not return is recorded as null with a note.
- Every one of the 15 templates was validated by `compile_canvas` until "Validation PASSED"; proof protocols are described in
  each template header and in `skills/powerapps-canvas-yaml/SKILL.md`.

## Disclaimer

This pack is a community effort, not an official Microsoft project. Power Apps control versions and the
authoring service change; the extraction is dated and should be re-audited when versions move (see the
LIMITES section of the skill).
