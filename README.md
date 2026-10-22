<p align="center">
  <img src="icon.svg" width="96" height="96" alt="Gemini Notebook for DeepSeek Harness">
</p>

<h1 align="center">Gemini Notebook (NotebookLM) MCP plugin for DeepSeek Harness</h1>

<p align="center">
  <b>Use Google NotebookLM / Gemini Notebook from the DeepSeek Harness (DSH) agent</b><br>
  53 tools for notebooks, sources, grounded Q&amp;A, web research, audio podcasts, video, slides, quizzes and more.
</p>

<p align="center">
  <a href="https://github.com/deepseek-ai/deepseek-harness"><img alt="DeepSeek Harness plugin" src="https://img.shields.io/badge/DeepSeek%20Harness-dsh--plugin-4f7cf7"></a>
  <a href="https://modelcontextprotocol.io"><img alt="MCP" src="https://img.shields.io/badge/MCP-stdio-9b59e6"></a>
  <a href="https://pypi.org/project/notebooklm-mcp-cli/"><img alt="notebooklm-mcp-cli on PyPI" src="https://img.shields.io/pypi/v/notebooklm-mcp-cli?label=notebooklm-mcp-cli"></a>
  <a href="LICENSE"><img alt="MIT License" src="https://img.shields.io/badge/license-MIT-green"></a>
</p>

<p align="center">
  English | <a href="README.zh-CN.md">简体中文</a>
</p>

---

**dsh-gemini-notebook-mcp** is a [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) plugin bundle (a "dsh plugin") that connects the
[Gemini Notebook / NotebookLM MCP server](https://github.com/jacob-bd/gemini-notebook-mcp-cli)
(`notebooklm-mcp-cli`) to the DSH agent. Once it's installed, you can ask DSH things like
*"create a notebook about quantum computing, add these three URLs and generate a podcast"*
and the agent does it through Gemini Notebook (formerly Google NotebookLM).

It contains configuration only, with no code of its own. It uses DSH's built-in MCP client, `@deepseek-ai/dsh-mcp-client`, to start
the `notebooklm-mcp` server over stdio. The server's tools then appear in DSH as
`mcp__gemini-notebook__<tool>`.

## Contents

- [What is DeepSeek Harness?](#what-is-deepseek-harness)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Usage examples](#usage-examples)
- [Available tools](#available-tools)
- [Configuration](#configuration)
- [Troubleshooting](#troubleshooting)
- [FAQ](#faq)
- [How it works](#how-it-works)
- [Credits and disclaimer](#credits-and-disclaimer)

## What is DeepSeek Harness?

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) (**DSH**) is DeepSeek's
AI agent harness. Its motto is *"Everything is a Plugin."* It runs as a Desktop app, a
Web GUI (`dsh web`), a headless CLI, an ACP server or an SDK. Every capability,
including tools, UI panels, models, skills and MCP connections, is a
[Cordis](https://github.com/topics/cordis) plugin. Plugins are composed into **profiles**
from **bundles**: npm-style packages that declare a `cordis.patch.yml`.

This repository is one of those bundles. It adds Gemini Notebook as an MCP tool provider to your
DSH profile.

## Features

- 📚 **Notebooks**: list, create, rename, describe and delete Gemini Notebook notebooks.
- 🔗 **Sources**: add URLs, YouTube videos, pasted text, Google Drive docs and local files (PDF, DOCX, audio, images and more); read raw source text; sync stale Drive sources.
- 💬 **Grounded Q&A**: ask questions answered only from your sources, with citations. Queries can also run across several notebooks. Chats stay in the NotebookLM web history.
- 🔎 **Research**: fast or deep web research and Google Drive search, with import of the sources it finds.
- 🎙️ **Studio**: generate Audio Overviews (podcasts), video overviews, slide decks, infographics, reports, quizzes, flashcards, mind maps, data tables and interactive reports, then download them.
- 🗂️ **Organization**: notes, source labels, collections, tags, aliases, batch operations and multi-step pipelines.
- 🤝 **Sharing**: public links and collaborator invites.
- 👤 **Accounts**: multiple Google profiles, usage quota checks and NotebookLM Enterprise.

## Requirements

| Requirement | Notes |
|---|---|
| [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) | Desktop app or `dsh web`, with a profile that includes Plugin Manager (the default ones do) |
| [`uv`](https://docs.astral.sh/uv/) | Used to install the Python MCP server |
| [`notebooklm-mcp-cli`](https://pypi.org/project/notebooklm-mcp-cli/) | Provides the `nlm` CLI and the `notebooklm-mcp` server |
| A Google account | With access to [Gemini Notebook / NotebookLM](https://notebooklm.google.com) |

Tested with DeepSeek Harness Desktop 0.2.1 on macOS and `notebooklm-mcp-cli` 0.15.4.

## Installation

### 1. Install and sign in to the MCP server

```bash
uv tool install notebooklm-mcp-cli   # installs nlm + notebooklm-mcp into ~/.local/bin
nlm login                            # opens a browser; sign in to Google once
nlm login --check                    # should print "Authentication valid!"
```

### 2. Install the DSH plugin bundle

**Option A: ask the DSH agent**, in a session where the agent has the `plugin_manager` tool (for example Creator mode):

> Install the DSH bundle from github:EdgarsKalnins/dsh-gemini-notebook-mcp

**Option B: Plugins page.** Open **Plugins** in the DSH sidebar, choose install and enter
`github:EdgarsKalnins/dsh-gemini-notebook-mcp`.

**Option C: from a local clone**, which lets you edit the config before installing:

```bash
git clone https://github.com/EdgarsKalnins/dsh-gemini-notebook-mcp.git
```

Then ask the DSH agent: *"Install the bundle at /absolute/path/to/dsh-gemini-notebook-mcp"*.

With hot reload (the default), the tools appear right away and no restart is needed.

### 3. Verify

Ask the agent: *"Call gemini-notebook server_info and list my notebooks."*
`server_info` should report `auth_status: configured` and 53 visible tools.

## Usage examples

Talk to the DSH agent in plain language:

- "List my Gemini Notebook notebooks."
- "Create a notebook called *AI Strategy Research* and add https://example.com/article as a source."
- "Do deep research on *cloud marketplace trends* and import the cited sources."
- "What are the key findings in my *Product Research* notebook? Cite the sources."
- "Generate a deep-dive audio podcast from this notebook and download it when it's ready."
- "Make a 10-question hard quiz and a study guide from my lecture notes notebook."
- "Create a slide deck, then revise slide 3 to be more concise."
- "Which Google Drive sources in this notebook are out of date? Sync them."

## Available tools

The tools are exposed as `mcp__gemini-notebook__<name>`.

| Group | Tools |
|---|---|
| Notebooks | `notebook_list`, `notebook_get`, `notebook_describe`, `notebook_create`, `notebook_rename`, `notebook_delete` |
| Sources | `source_add`, `source_list_drive`, `source_describe`, `source_get_content`, `source_rename`, `source_sync_drive`, `source_delete` |
| Chat & query | `notebook_query`, `notebook_query_start`, `notebook_query_status`, `chat_configure`, `chat_list`, `chat_get`, `chat_export`, `cross_notebook_query` |
| Studio | `studio_create`, `studio_status`, `studio_revise`, `studio_delete`, `report`, `download_artifact`, `download_all_artifacts`, `export_artifact` |
| Research | `research_start`, `research_status`, `research_import` |
| Notes | `note`, `chat_save_to_note` |
| Organization | `label`, `tag`, `collection_list`, `collection_create`, `collection_edit`, `collection_set_emoji`, `collection_delete` |
| Automation | `batch`, `pipeline` |
| Sharing | `notebook_share_status`, `notebook_share_public`, `notebook_share_invite`, `notebook_share_batch` |
| Account | `profile`, `alias`, `refresh_auth`, `save_auth_tokens`, `usage_get`, `server_info` |

Destructive tools such as `notebook_delete`, `source_delete`, `studio_delete` and sharing do nothing unless `confirm=True` is passed. Studio generation also asks for confirmation.

## Configuration

All settings are in [`cordis.patch.yml`](cordis.patch.yml):

```yaml
- insert:
    - id: gemini-notebook-mcp
      name: '@deepseek-ai/dsh-mcp-client'
      config:
        serverName: gemini-notebook
        transport: stdio
        command: !!js '`${process.env.HOME}/.local/bin/notebooklm-mcp`'
        args: ['--transport', 'stdio']
        env:
          NOTEBOOKLM_HL: en
          NOTEBOOKLM_QUERY_TIMEOUT: '150'
        toolCallTimeoutMs: 180000
        failOnStartupError: true
```

Common changes:

| Goal | Change |
|---|---|
| **Reduce context usage** (53 tool schemas go with every request) | Add `NOTEBOOKLM_DISABLED_GROUPS: studio,sharing,research` under `env` |
| Hide or show single tools | `NOTEBOOKLM_DISABLED_TOOLS` / `NOTEBOOKLM_ENABLED_TOOLS` |
| Run without installing (via `uvx`) | `command: uvx`, `args: ['--from', 'notebooklm-mcp-cli', 'notebooklm-mcp']` |
| Binary in a different location | Set `command` to the output of `which notebooklm-mcp` |
| Download folder for artifacts | `NOTEBOOKLM_DOWNLOAD_DIR: /path/to/dir` |
| Language of the interface and generated artifacts | `NOTEBOOKLM_HL: de` (any BCP-47 tag, for example `es-419`) |
| NotebookLM Enterprise | `NOTEBOOKLM_BASE_URL`, `NOTEBOOKLM_PROJECT_ID`, `NOTEBOOKLM_LOCATION` |

Tool groups: `notebooks_read`, `notebooks_manage`, `sources_read`, `sources_manage`, `chat`,
`query_multi`, `organization`, `automation`, `notes`, `auth`, `server`, `sharing`, `research`, `studio`.

DSH scrubs secret-looking variables (`*TOKEN*`, `*KEY*`, …) from the server's environment. The
Google login lives in `~/.notebooklm-mcp-cli`, and is encrypted there if you use `nlm auth storage set protected`.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Plugin fails to activate | Check that `~/.local/bin/notebooklm-mcp --help` works, or fix `command` |
| `auth_status: stale` or auth errors | Run `nlm login`, then call `refresh_auth` or reload the plugin |
| Queries time out on large notebooks | Raise `NOTEBOOKLM_QUERY_TIMEOUT` and `toolCallTimeoutMs`, or use `notebook_query_start` + `notebook_query_status` |
| Rate-limit / quota errors | Ask the agent to call `usage_get`; NotebookLM meters a rolling ~5 h window plus a weekly cap |
| Old server version | `uv tool upgrade notebooklm-mcp-cli`, then reload the plugin |

## FAQ

**How do I use NotebookLM in DeepSeek Harness?**
Install `notebooklm-mcp-cli`, run `nlm login`, then install this bundle in DSH. The agent gets the full set of Gemini Notebook tools.

**Is this an official Google or DeepSeek project?**
No. It's a community DSH plugin. The MCP server comes from [jacob-bd/gemini-notebook-mcp-cli](https://github.com/jacob-bd/gemini-notebook-mcp-cli) and uses undocumented NotebookLM APIs.

**Does it work with Claude Code, Cursor or Codex?**
This bundle is for DeepSeek Harness only. For other agents, use upstream's `nlm setup` wizard.

**Can I generate a NotebookLM podcast (Audio Overview) from DSH?**
Yes. Ask the agent to create an audio artifact. It polls `studio_status` and downloads the result with `download_artifact`.

**Does it support multiple Google accounts?**
Yes. Run `nlm login --profile work`, then have the agent call `profile(action="switch", name="work")`.

## How it works

```
DeepSeek Harness agent
   └─ @deepseek-ai/dsh-mcp-client  (row "gemini-notebook-mcp", from this bundle's cordis.patch.yml)
        └─ stdio ─► notebooklm-mcp  (Python, notebooklm-mcp-cli)
                      └─ HTTPS ─► Gemini Notebook / NotebookLM (your Google session)
```

Repository layout:

| File | Purpose |
|---|---|
| `package.json` | Bundle manifest (`dsh.bundle.patch`) |
| `cordis.patch.yml` | Inserts the MCP client row |
| `locale/*.json` | Title and description shown in DSH Plugin Manager |
| `icon.svg` | Plugin icon |

## Credits and disclaimer

- MCP server and CLI: [jacob-bd/gemini-notebook-mcp-cli](https://github.com/jacob-bd/gemini-notebook-mcp-cli) (MIT).
- Agent harness: [deepseek-ai/deepseek-harness](https://github.com/deepseek-ai/deepseek-harness).
- More DSH plugins: [awesome-dsh-plugin](https://github.com/awesome-dsh-plugin/awesome-dsh-plugin) · [GitHub topic `dsh-plugin`](https://github.com/topics/dsh-plugin).

NotebookLM and Gemini are trademarks of Google LLC. This project is not affiliated with Google or DeepSeek. The
upstream server uses undocumented APIs that may change, and use may not match Google's Terms of Service. Use it at your own risk.

## License

[MIT](LICENSE)
