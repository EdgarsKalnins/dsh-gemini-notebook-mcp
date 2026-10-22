<p align="center">
  <img src="icon.svg" width="96" height="96" alt="Gemini Notebook for DeepSeek Harness">
</p>

<h1 align="center">DeepSeek Harness 的 Gemini Notebook（NotebookLM）MCP 插件</h1>

<p align="center">
  <b>在 DeepSeek Harness（DSH）智能体中使用 Google NotebookLM / Gemini Notebook</b><br>
  53 个工具：笔记本、来源、基于来源的问答、网络研究、音频播客、视频、幻灯片、测验等。
</p>

<p align="center">
  <a href="README.md">English</a> | 简体中文
</p>

---

**dsh-gemini-notebook-mcp** 是一个 [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness) 插件包（dsh 插件 / bundle），
通过 DSH 内置的 MCP 客户端 `@deepseek-ai/dsh-mcp-client` 以 stdio 方式启动
[Gemini Notebook / NotebookLM MCP 服务器](https://github.com/jacob-bd/gemini-notebook-mcp-cli)（`notebooklm-mcp-cli`）。
工具在 DSH 中显示为 `mcp__gemini-notebook__<tool>`。本插件只含配置，不含代码。

## 什么是 DeepSeek Harness？

[DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)（**DSH**）是 DeepSeek 的 AI 智能体框架，
理念是「Everything is a Plugin（一切皆插件）」。它提供桌面版、Web 界面（`dsh web`）、无头 CLI、ACP 和 SDK。
工具、界面、模型、技能和 MCP 连接都是 Cordis 插件，按 **profile** 由 **bundle**（声明 `cordis.patch.yml` 的包）组合而成。

## 功能

- 📚 笔记本：列出、创建、重命名、摘要、删除
- 🔗 来源：URL、YouTube、文本、Google Drive、本地文件（PDF、DOCX、音频、图片等）
- 💬 基于来源的问答（带引用），支持跨笔记本查询
- 🔎 网络 / Drive 快速或深度研究，并导入来源
- 🎙️ Studio：音频概览（播客）、视频、幻灯片、信息图、报告、测验、闪卡、思维导图、数据表，并可下载
- 🗂️ 笔记、标签、集合、批量操作、流水线；🤝 分享；👤 多 Google 账号与企业版

## 安装

```bash
uv tool install notebooklm-mcp-cli   # 安装 nlm 和 notebooklm-mcp
nlm login                            # 在浏览器中登录 Google
nlm login --check
```

然后在 DSH 中安装插件包，任选一种：

- 对 DSH 智能体说：*「安装 DSH bundle：github:EdgarsKalnins/dsh-gemini-notebook-mcp」*
- 在侧边栏 **Plugins（插件）** 页面安装 `github:EdgarsKalnins/dsh-gemini-notebook-mcp`
- `git clone` 本仓库后，让智能体安装该目录的绝对路径

启用热重载时无需重启。验证：让智能体调用 `server_info`，应显示 `auth_status: configured` 和 53 个工具。

## 使用示例

- 「列出我的 Gemini Notebook 笔记本」
- 「创建名为『AI 战略研究』的笔记本，并添加这个链接作为来源」
- 「对『云市场趋势』做深度研究并导入引用的来源」
- 「用这个笔记本生成深度音频播客，完成后下载」
- 「根据课堂笔记生成 10 道难题测验和学习指南」

## 配置

编辑 [`cordis.patch.yml`](cordis.patch.yml) 的 `env`：

- `NOTEBOOKLM_DISABLED_GROUPS: studio,sharing,research`：隐藏工具组以节省上下文
- `NOTEBOOKLM_HL: zh-CN`：界面与生成内容的语言
- `NOTEBOOKLM_DOWNLOAD_DIR`：下载目录
- `NOTEBOOKLM_BASE_URL` / `NOTEBOOKLM_PROJECT_ID`：企业版

登录过期时运行 `nlm login`，然后调用 `refresh_auth` 或重新加载插件。

## 致谢与声明

MCP 服务器来自 [jacob-bd/gemini-notebook-mcp-cli](https://github.com/jacob-bd/gemini-notebook-mcp-cli)（MIT）。
更多 DSH 插件：[awesome-dsh-plugin](https://github.com/awesome-dsh-plugin/awesome-dsh-plugin) · [`dsh-plugin` 话题](https://github.com/topics/dsh-plugin)。
NotebookLM 与 Gemini 是 Google LLC 的商标；本项目与 Google、DeepSeek 无关。上游使用未公开的 API，风险自负。

## 许可证

[MIT](LICENSE)
