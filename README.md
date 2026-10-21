# Gemini Notebook for DSH

Configuration-only DSH bundle that connects the MCP server from
[jacob-bd/gemini-notebook-mcp-cli](https://github.com/jacob-bd/gemini-notebook-mcp-cli)
(`notebooklm-mcp-cli`, MIT) through `@deepseek-ai/dsh-mcp-client`.
Its tools appear to the agent as `mcp__gemini-notebook__<tool>`, for example
`mcp__gemini-notebook__notebook_list`.

## Prerequisites

```bash
uv tool install notebooklm-mcp-cli   # provides nlm + notebooklm-mcp in ~/.local/bin
nlm login                            # sign in to Google once
nlm login --check                    # verify
```

## Install

```bash
git clone https://github.com/EdgarsKalnins/dsh-gemini-notebook-mcp.git
```

Then ask the DSH agent: *"Install the bundle at /absolute/path/to/dsh-gemini-notebook-mcp"*
(it uses `plugin_manager` → `install_bundle` with that directory as target).
Tools appear immediately; no restart needed.

## Configuration

Edit `cordis.patch.yml`:

- `command`: path to `notebooklm-mcp` (default `~/.local/bin/notebooklm-mcp`).
  To run without installing, use `command: uvx` with
  `args: ['--from', 'notebooklm-mcp-cli', 'notebooklm-mcp']`.
- `env`: any upstream variable, for example
  - `NOTEBOOKLM_DISABLED_GROUPS: studio,sharing,research` to hide tool groups
    and save context (the server exposes 53 tools);
  - `NOTEBOOKLM_DOWNLOAD_DIR` for artifact downloads;
  - `NOTEBOOKLM_BASE_URL` / `NOTEBOOKLM_PROJECT_ID` for Enterprise.

If the Google login expires, run `nlm login` again, then reload the plugin.

Upstream uses undocumented Google APIs; use at your own risk.
