# Changelog

All notable changes to Acelogic Agents. Installers are on the [Releases](https://github.com/acelogicg/Acelogic-Agents-Releases/releases) page.

Bahasa Indonesia: [CHANGELOG.id.md](CHANGELOG.id.md)

## 1.0.10 — 2026-09-25

This release reduces how many tokens each message costs, makes long runs on local models more reliable, and adds a workspace preview for files and web pages.

### New

- **Preview panel.** The `</>` button in the top bar opens a panel beside the chat, with two kinds of tabs:
  - **Files.** Click any file in the workspace tree to open it in a code editor with syntax highlighting. Save with `Ctrl+S`. Line endings (CRLF/LF) are kept as they are, and if the file changed on disk after you opened it, the save is refused so you don't overwrite someone else's changes. Markdown opens rendered, with a switch to the source. Images open as images. After the agent finishes a turn, tabs you haven't edited reload with its changes.
  - **Browser.** Open a local dev server, a website, or an HTML file from the workspace. Short addresses work: `5173` becomes `http://localhost:5173`. The tab has back, forward, reload, DevTools, and open-in-system-browser. Pages run sandboxed with no access to the app.
- **One model list for every connected provider.** Models from your OpenAI-compatible server, your OpenAI API key, and your ChatGPT account load together, grouped by provider. Choosing a model switches to its provider. The provider tabs in Settings → Connection are now only for configuring each provider. If one provider is offline, models from the others still load, and Settings shows that provider's error.
- **The agent can add repositories to the workspace.** Ask it to add a local folder or clone a repository, and it appears in the workspace list. This is available in Full access mode only, and the usual trust prompt still runs before the repository's own config is loaded.
- **The agent knows which app features are off.** If a request needs something that is disabled (web tools, device control, MCP, embeddings, memory, or Full access), the agent says which feature it is and exactly where to turn it on, using the labels you see in the app. It no longer works around it or says it succeeded when it didn't.

### Improved

- **Lower token use per message.** A plain greeting used to cost about 18k tokens and now costs about 6k. The agent now runs on a shorter system prompt and receives only the tools it uses. The skill list contains only the app's own skills. MCP servers are sent only when a message needs them: when it asks about code structure, names the server or one of its tools, continues a turn that already used it, or, with embeddings enabled, closely matches one of its tools.
- **Every turn that changes files ends with a summary.** If the agent stops without explaining its edits, it is asked once to describe each changed file, what changed and how, and which checks actually ran. If it still says nothing, the app adds a list of the changed files with line counts.
- **Slow local models no longer time out.** Prompt processing on local hardware can take longer than five minutes. Before, the connection was cut at that point and the request restarted from scratch several times before the turn failed. The app now keeps the connection open while the model works and waits up to 30 minutes on the Claude runtime. This applies to both runtimes.
- **Automatic reconnect.** If the provider is briefly unreachable (restarting, or returning 502/503/504), the request is retried after 1, 3, and 7 seconds, and the status bar shows *Reconnecting to provider…*. Outages of up to about 30 seconds recover without interrupting the turn.

### Fixed

- Switching from another provider to ChatGPT could hang on loading and show no models. This happened when the model check restarted the ChatGPT runtime while the account status and model list were still being fetched.
- A model list that was still loading could swallow a provider switch, leaving the list stale until a manual refresh.

### Changed

- The subagent tool was removed from the Claude runtime. Each subagent resent the full prompt, which multiplied token use on local models. Searches now run in the main agent.

## Earlier versions

Notes for 1.0.9 and earlier are on each version's [release page](https://github.com/acelogicg/Acelogic-Agents-Releases/releases).
