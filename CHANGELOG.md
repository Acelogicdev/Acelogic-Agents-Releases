# Changelog

All notable changes to Acelogic Agents. Installers are on the [Releases](https://github.com/acelogicg/Acelogic-Agents-Releases/releases) page.

Bahasa Indonesia: [CHANGELOG.id.md](CHANGELOG.id.md)

## 1.0.13 — 2026-10-05

This release lets you attach any file to a message, keeps the app's cache in check, and makes code and links in replies easy to use.

### New

- **Attach files.** The paperclip button in the composer attaches any file, not just images. You can also drag files onto the composer or paste them. Up to 8 attachments per message.
  - Text files up to 256 KB (code, Markdown, JSON, CSV, logs…) are sent with the message, so any model can read them.
  - Larger or binary files (PDF, ZIP, Word…) are passed by their path, and the agent reads them with its tools.
  - Vision models still receive images as images.
- **Automatic cache cleanup.** Settings → Behavior → *Auto clean cache* clears the app's cache every 30 days by default (7, 14, 30, 60 or 90 days). *Clean now* shows the current cache size and clears it right away. Your chats, memory, settings and the downloaded voice model are kept.
- **Copy code.** Code blocks in replies have a *Copy* button, and inline code or commands such as `npm run build` copy with a single click.
- **Clickable links.** Links in replies, including Markdown links, open in your default browser.

### Improved

- Attachments sit inside the message box as tidy cards with the file name, type and size, and follow the selected theme.

### Fixed

- Inline code in replies is shown exactly as written. Names like `my_var_name` used to lose their underscores.
- Dropping a file outside the message box no longer replaces the app window, and links can no longer open inside a new app window.

## 1.0.12 — 2026-10-04

This release makes it clear why the agent stopped, and fixes replies that went missing on the ChatGPT/OpenCode runtime.

### New

- **Clear provider errors.** When the AI provider rejects a request, the chat names the cause and what to do next, followed by the provider's own message:
  - rate limit, with how long to wait when the provider says so
  - quota or credit exhausted
  - API key rejected, or access to the model denied
  - model or endpoint not found
  - conversation longer than the model's context
  - provider timeout, overloaded, or internal error
  - model server unreachable (for example LM Studio not running)
- **Retry status.** Both runtimes retry rate limits and overloaded providers on their own. While they wait, the status bar and composer show the reason, with a countdown when the provider gives one, for example *Provider rate limit hit — retrying in 18s*.
- **Delete the saved OpenAI-compatible connection.** Settings → Connection → *Delete connection* removes the saved URL and API token, including leftovers from older versions that used to come back after a restart.

### Fixed

- **Replies no longer disappear on ChatGPT and other OpenCode-runtime models.** From the second message in a session, the reply was sometimes generated but never shown, and the turn ended with no message. The app now waits for the runtime's event stream before sending, and fills in any text it still missed from the final result.
- If a model finishes without any reply, the chat now says so instead of showing nothing.
- The OpenCode runtime gets 90 seconds to start instead of 15, so a first launch on a slow machine or under an antivirus scan no longer fails with *startup timed out*. The error now points to the antivirus and the OpenCode log when it does happen.
- Asked about voice, the agent said the app had none. It now knows the app's own features (dictation, live voice, screen and camera sharing, preview panel, terminal, and more) and points to them.

## 1.0.11 — 2026-10-01

### New

- Right-click menu in the composer (Paste, Cut, Copy, Select All) and Copy on selected text in chat bubbles.
- Paste screenshots and images straight from the clipboard into the composer.

### Improved

- Lower and more stable token use: resumed sessions no longer resend the full history, and the system prompt stays identical across turns so provider caching keeps working.

### Fixed

- Clearer errors when the agent runtime fails to launch, and the workspace folder is checked before a session starts.

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
