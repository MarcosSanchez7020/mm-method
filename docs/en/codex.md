# Codex support

mm-method 1.2.0 packages the same English and Spanish skills for Codex and Claude Code. It contains instructions only. It adds no installer scripts, hooks, servers, connectors or authentication requirements. The catalog declares `policy.authentication` because the format requires it; the plugin has no connectors, so there is nothing to authenticate.

## Install and invoke

In a terminal with Codex CLI and plugin support:

```bash
codex plugin marketplace add MarcosSanchez7020/mm-method
codex plugin add metodologia-agentes@mm-method
codex plugin list --marketplace mm-method --json
```

To install a published version instead of the default branch, add the catalog with its tag: `codex plugin marketplace add MarcosSanchez7020/mm-method --ref v1.2.0`.

Start a new session. Use the complete names shown by the plugin:

- English: `$metodologia-agentes:agent-methodology`.
- Spanish: `$metodologia-agentes:metodologia-agentes`.

For example: "Use $metodologia-agentes:agent-methodology to plan adoption in this repository. Before writing files, give me the decisions as a numbered list."

The short unqualified name did not inject the full skill in the tested non-interactive Codex session. The complete names did. The model may also select a skill from its description, but automatic selection is not guaranteed.

## One source, two clients

The Codex catalog is `.agents/plugins/marketplace.json`; it points to `plugins/metodologia-agentes/`. That directory contains the portable `plugin.json` and the existing `.claude-plugin/plugin.json`. Claude keeps its `.claude-plugin/marketplace.json` at the repository root. Both clients use the two existing `skills/` folders and their ten references each. No symlinks, generated copies or synchronization scripts are needed.

Codex installs the package under `plugins/cache/mm-method/metodologia-agentes/<version>/`, inside its configuration directory (`~/.codex` by default). This is an installation cache, not another source to edit. Make changes in the repository and reinstall the package.

When adopting the methodology in a project, keep shared rules in `CLAUDE.md`. Include the `AGENTS.md` bridge from the [adoption guide](adoption.md#6-agentsmd-whenever-codex-is-used), even if Codex is the only agent. Installing the plugin does not create those project files or configure GitHub protections.

## Update and remove

For a Git-backed catalog, refresh it and install the selected version again:

```bash
codex plugin marketplace upgrade mm-method
codex plugin add metodologia-agentes@mm-method
```

Start a new session. To remove this plugin, then the catalog if no longer needed:

```bash
codex plugin remove metodologia-agentes@mm-method
codex plugin marketplace remove mm-method
```

These commands affect Codex only. Claude Code keeps its own installation and update commands in the [README](../../README.md#claude-code).

## Test scope

Tests on 2026-10-25 used Windows, Codex CLI 0.162.1 and Claude Code 2.1.296. The two exact public Codex installation commands succeeded on `main` 1.0.1 before being documented. Version 1.1.0 was installed separately from the working branch. The commands install the catalog's default branch.

The 1.1.0 local installation passed in both clients. Codex had no mm-method catalog, plugin registration or cache before the first test; its unrelated user configuration was not a new profile. Claude installation used an empty, separate configuration directory. The two skills loaded in ES and EN in fresh sessions: Codex used complete skill names with user configuration ignored; Claude used the clean installation's plugin directory with restricted tools and external MCP configuration excluded. All 22 installed Markdown files matched the shared source in each client. Claude strict validation passed for the catalog and plugin, and the skill validator passed for both languages.

These are installation and explicit-loading tests, not a guarantee of every workflow or automatic activation. Desktop/IDE UI installation, macOS, Linux, other client versions and Codex context cost were not tested.

Tests of 1.2.0 on 2026-10-25 used Windows, Codex CLI 0.162.1 and Claude Code 2.1.296, with the working branch's local catalog. Installation passed in both clients with new, empty configuration directories (`CLAUDE_CONFIG_DIR` and `CODEX_HOME`): the 25 installed files, `LICENSE` included, matched the source byte for byte. In Claude, the two skills loaded in ES and EN in fresh sessions with `--plugin-dir`, `--restricted` and `--strict-mcp-config`.

After the merge, Codex CLI 0.162.1 installed 1.2.0 from `main` (`5c76fc4`) with the two public commands, with no previous mm-method catalog, registration or cache: the 25 installed files, `LICENSE` included, match `main` with line endings normalized, and both skills loaded in full in ES and EN in fresh read-only sessions, with `--ignore-user-config` and the complete names.

The installation pinned to `v1.2.0` was tested on 2026-10-25 from GitHub, with Claude Code 2.1.296 (`marketplace add MarcosSanchez7020/mm-method@v1.2.0`) and Codex CLI 0.162.1 (`marketplace add MarcosSanchez7020/mm-method --ref v1.2.0`), each with a new, empty configuration directory. Both recorded the `v1.2.0` ref, cloned commit `5c76fc4` and installed 1.2.0. The 25 installed files match the tag's files, except for line endings, which Git for Windows converted to CRLF. `marketplace update` on a pinned catalog was not tested.

## Sources

Official documentation consulted on 2026-10-25:

- [Package your plugin](https://developers.openai.com/plugins/build/plugins): portable manifests, catalogs and installation storage.
- [Build skills](https://learn.chatgpt.com/docs/build-skills): skill structure, local discovery and distribution.
- [Project instructions](https://learn.chatgpt.com/docs/agent-configuration/agents-md): Codex discovers `AGENTS.md`.

The `codex plugin add` command was obtained from local CLI help and then executed successfully; it was not inferred from the marketplace command or presented as a quotation from the website.
