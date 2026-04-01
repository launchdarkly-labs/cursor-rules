# cursor-rules

Example rules to help Cursor users build better with [LaunchDarkly](https://launchdarkly.com/).

This repo ships **Cursor project rules** as `.mdc` files. Each file is a small policy Cursor can apply when you work in Agent or Chat, so suggestions stay aligned with good flag and SDK patterns.

| Rule file | Intent |
| --------- | ------ |
| [`using-flags.mdc`](using-flags.mdc) | Centralized evaluation, context design, fallbacks |
| [`implementing-launchdarkly.mdc`](implementing-launchdarkly.mdc) | SDK wrappers, config, singleton init, shutdown |
| [`troubleshooting-launchdarkly.mdc`](troubleshooting-launchdarkly.mdc) | Debugging and operational guidance |

---

## TL;DR — quick adoption (single monorepo)

For one repo that contains all your apps (**open the monorepo root in Cursor**):

1. Create **`/.cursor/rules/`** at the **repository root** (same level as your root `package.json` / `go.work` / etc.).
2. Copy **`using-flags.mdc`**, **`implementing-launchdarkly.mdc`**, and **`troubleshooting-launchdarkly.mdc`** from this repo into that folder (keep names and **`.mdc`** extension).
3. **Commit** `.cursor/rules/*.mdc` so the team gets the same rules.
4. Reload the window or reopen the folder if rules do not show up immediately.

That’s enough for day-to-day use. **Verification** (Settings UI, `alwaysApply` / `globs`, sub-packages) is in the [full guide](#full-guide) below.

---

## Full guide

### 1. Prerequisites

- [Cursor](https://cursor.com/) installed (rules format targets **Cursor’s project rules**, not generic Copilot config).
- Your app opened in Cursor as a **folder** or **multi-root workspace** (rules apply relative to the workspace).

### 2. Copy the rule files into your project

From this repository, copy the three `.mdc` files into **your** repo:

```text
your-project/
  .cursor/
    rules/
      using-flags.mdc
      implementing-launchdarkly.mdc
      troubleshooting-launchdarkly.mdc
```

Create `.cursor/rules/` if it does not exist. **Keep the `.mdc` extension** so Cursor treats them as project rules.

Ways to get the files:

- **Download:** GitHub → each file → *Raw* → save with the same name under `.cursor/rules/`.
- **Clone and copy:** `git clone https://github.com/launchdarkly-labs/cursor-rules.git` then copy the three files into your project.
- **Submodule (optional):** add this repo as a submodule and symlink or copy the `.mdc` files into `.cursor/rules/` (Cursor does not read rules from arbitrary paths outside `.cursor/rules/` unless you copy or link them there).

### 3. Commit them (recommended for teams)

Check `.cursor/rules/*.mdc` into version control so everyone gets the same guidance:

```bash
git add .cursor/rules/*.mdc
git commit -m "chore: add LaunchDarkly Cursor project rules"
```

### 4. Verifying rules are active

1. Open **Cursor Settings** → search for **Rules** (wording may vary by Cursor version, e.g. **Cursor Settings → General → Project rules**).
2. You should see your **project** rules listed; each rule’s **description** comes from the YAML frontmatter `description` field in each `.mdc` file.
3. Start **Agent** or **Chat** in a file where LaunchDarkly is relevant; ask something that would touch flags or SDK setup—the model should respect the rule content when it’s included in context.

If a rule has `alwaysApply: false` (the default in these examples), Cursor may attach it when the task matches the rule’s **description** or **globs**. You can set `alwaysApply: true` in the frontmatter if you want that file’s rules considered for every request in that project (heavier context).

### 5. (Optional) Scope rules with `globs`

To limit a rule to certain paths, edit the frontmatter in that `.mdc` file, for example:

```yaml
---
description: LaunchDarkly flag usage conventions for application code
globs: src/**/*.ts,src/**/*.tsx
alwaysApply: false
---
```

Use patterns that match your repo layout. Leave `globs` empty to rely on description-based inclusion only.

### 6. Monorepos and multiple apps

- **Single monorepo, root opened in Cursor:** use **one** `.cursor/rules/` at the **repo root** (same as the TL;DR above). Rules apply across the tree for that workspace.
- **You only open a subpackage as the workspace folder:** put `.cursor/rules/` **inside that package** instead (Cursor only loads project rules from the opened root).

---

## Keeping rules up to date

- **Pull updates:** Re-copy or merge changes from this repo when the `.mdc` files change.
- **Fork:** Fork this repo if your org wants to customize rules and still track upstream.

---

## References

- Cursor **Rules** documentation: [https://docs.cursor.com/context/rules](https://docs.cursor.com/context/rules)
- LaunchDarkly documentation: [https://docs.launchdarkly.com/](https://docs.launchdarkly.com/)

---

## License

See [LICENSE](LICENSE).
