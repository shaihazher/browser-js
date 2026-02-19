# browser-js

Lightweight CDP (Chrome DevTools Protocol) browser control for AI agents. Built as an [OpenClaw](https://openclaw.ai) skill.

**3-10x fewer tokens per interaction** compared to traditional browser automation tools.

## Why?

Traditional browser tools for AI agents (Playwright snapshots, accessibility trees) return 2-10KB per interaction. The agent then spends thousands of thinking tokens parsing the output to find the right element to click.

**browser-js** returns minimal, indexed output that's immediately actionable:

```
[0] (link) Hacker News → https://news.ycombinator.com/news
[1] (link) new → https://news.ycombinator.com/newest
[2] (input:text) Search
[3] (button) Submit
```

Then `bjs click 3` — done. No interpretation layer.

## Install

### As an OpenClaw skill

```bash
npx clawhub@latest install browser-js
```

### Standalone

```bash
git clone https://github.com/shaihazher/browser-js.git
cd browser-js/scripts
npm install
```

## Setup

Requires Chrome/Chromium running with CDP enabled:

```bash
# With OpenClaw (automatic):
# browser start profile=openclaw

# Or manually:
google-chrome --remote-debugging-port=18800 --user-data-dir=~/.browser-data
```

### Optional alias

```bash
mkdir -p ~/.local/bin
echo '#!/bin/bash
exec node /path/to/scripts/browser.js "$@"' > ~/.local/bin/bjs
chmod +x ~/.local/bin/bjs
```

## Commands

| Command | Description |
|---------|-------------|
| `bjs tabs` | List open tabs |
| `bjs open <url>` | Navigate to URL |
| `bjs elements` | List all interactive elements (indexed) |
| `bjs click <n>` | Click element by index |
| `bjs type <n> <text>` | Type into element |
| `bjs upload <path>` | Upload file (bypasses OS dialog) |
| `bjs text` | Extract page text (compact) |
| `bjs eval <js>` | Run JavaScript |
| `bjs screenshot [path]` | Save screenshot |
| `bjs scroll <direction>` | Scroll up/down/top/bottom |
| `bjs url` | Current URL |
| `bjs back / forward / refresh` | Navigation |
| `bjs tab <n>` | Switch tab |
| `bjs newtab [url]` | Open new tab |
| `bjs close [n]` | Close tab |
| `bjs html <selector>` | Get element HTML |
| `bjs wait <ms>` | Wait |

## Key Features

### Auto-indexing
`click` and `type` automatically index elements if not already done. No need to call `elements` first.

### File uploads
`upload` uses CDP's `DOM.setFileInputFiles` to inject files directly into hidden `<input type="file">` elements — bypasses the OS file picker dialog entirely. Works with Instagram, Twitter, any site.

### Signed-in sessions
Uses your existing browser profile with all cookies and sessions intact. If you're signed into GitHub, Instagram, LinkedIn — browser-js has access.

### Token efficiency

| Approach | Tokens per action | 10-step flow |
|----------|------------------|--------------|
| **browser-js** | ~50-200 | ~1,500 |
| Browser tool (snapshot) | ~2,000-5,000 | ~30,000 |
| Browser tool + thinking | ~3,000-8,000 | ~60,000+ |

## Environment

| Variable | Default | Description |
|----------|---------|-------------|
| `CDP_URL` | `http://127.0.0.1:18800` | Chrome CDP endpoint |

## License

MIT
