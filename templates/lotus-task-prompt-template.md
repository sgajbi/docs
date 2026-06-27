# Lotus Task Prompt Template

Use this template when creating a new reusable or one-off Lotus implementation prompt.

```text
You are working in `<repo>` at `<repo-path>`.

Goal:
`<clear outcome>`

Scope:
1. `<in-scope item>`
2. `<in-scope item>`
3. `<in-scope item>`

Out of scope:
1. `<explicit non-goal>`
2. `<explicit non-goal>`

Before editing:
1. Load Lotus context in AGENTS order.
2. Read `<repo-path>\REPOSITORY-ENGINEERING-CONTEXT.md`.
3. Read `<specific RFC/standard/playbook/skill>`.
4. Run branch and PR orientation.
5. Summarize plan, validation lane, risks, and evidence target.

Implementation rules:
1. Work slice by slice.
2. Preserve source ownership boundaries.
3. Keep commits small and meaningful when commits are requested.
4. Add meaningful tests.
5. Update README/docs/wiki/context/supported-features when truth changes.
6. Create downstream or upstream issues when out-of-scope integration gaps are found.

Validation:
1. `<focused local check>`
2. `<contract or OpenAPI check>`
3. `<integration or browser check>`
4. `<GitHub check expectation>`

Definition of done:
1. Implementation complete and tested.
2. Docs/wiki/context/supported-features updated or explicit no-change decision recorded.
3. PR checks green if a PR is opened.
4. Wiki published if wiki source changed.
5. Branch hygiene and clean status verified.
```
