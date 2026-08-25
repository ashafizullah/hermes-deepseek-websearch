# Hermes DeepSeek Web Search Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![npm version](https://img.shields.io/npm/v/hermes-deepseek-websearch.svg)](https://www.npmjs.com/package/hermes-deepseek-websearch)
[![GitHub stars](https://img.shields.io/github/stars/ashafizullah/hermes-deepseek-websearch.svg)](https://github.com/ashafizullah/hermes-deepseek-websearch/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/ashafizullah/hermes-deepseek-websearch.svg)](https://github.com/ashafizullah/hermes-deepseek-websearch/issues)

> 🇮🇩 [Bahasa Indonesia](README_ID.md) | 🇨🇳 [中文](README_ZH.md)

A Hermes Agent plugin that uses DeepSeek's native server-side web search API.

## Features

- **Native DeepSeek Web Search** — Uses DeepSeek's built-in `web_search` tool on the Responses API
- **Fast & Reliable** — Results come directly from DeepSeek's search infrastructure
- **Structured Results** — Returns titles, URLs, descriptions, and positions
- **Easy Setup** — Just add your API key and enable the plugin

## Pricing

DeepSeek Web Search is billed per call (separate from token pricing):
- **~$0.0005 per search** — Very affordable for high-volume usage

## Prerequisites

1. **DeepSeek API Key**
   - Get one at https://platform.deepseek.com

2. **Hermes Agent** installed
   - Install: `curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash`

## Installation

### Option 1: npm (Recommended)

```bash
npm install -g hermes-deepseek-websearch
```

### Option 2: Manual Install

```bash
# Clone this repo
git clone https://github.com/ashafizullah/hermes-deepseek-websearch.git

# Copy plugin to Hermes plugins directory
cp -r hermes-deepseek-websearch ~/.hermes/plugins/web/deepseek
```

### Option 3: Direct Download

```bash
# Create plugin directory
mkdir -p ~/.hermes/plugins/web/deepseek

# Download files
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/provider.py -o ~/.hermes/plugins/web/deepseek/provider.py
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/__init__.py -o ~/.hermes/plugins/web/deepseek/__init__.py
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/plugin.yaml -o ~/.hermes/plugins/web/deepseek/plugin.yaml
```

## Configuration

### 1. Add API Key to `.env`

```bash
# Add to ~/.hermes/.env
DEEPSEEK_API_KEY=sk-your-api-key-here
```

### 2. Update `config.yaml`

```bash
# Enable plugin
hermes config set plugins.enabled '["web/deepseek"]'

# Set as search backend
hermes config set web.search_backend deepseek
```

Or manually edit `~/.hermes/config.yaml`:

```yaml
plugins:
  enabled:
    - web/deepseek
  entries:
    web/deepseek:
      allow_tool_override: false

web:
  search_backend: deepseek
```

### 3. Restart Gateway

```bash
hermes gateway restart
```

## Usage

Once configured, use `web_search` as normal:

```python
# In Hermes chat
web_search(query="latest news Indonesia", limit=5)
```

## How It Works

1. Hermes calls `web_search` tool
2. Plugin sends request to DeepSeek Responses API with `{"type": "web_search"}` tool
3. DeepSeek performs server-side search and returns results as annotations
4. Plugin parses annotations and returns structured results

## Troubleshooting

### Empty results

1. Check logs: `tail -f ~/.hermes/logs/gateway.log | grep deepseek`
2. Verify API key: `grep DEEPSEEK_API_KEY ~/.hermes/.env`
3. Clear cache: `rm -rf ~/.hermes/plugins/web/deepseek/__pycache__`
4. Restart gateway: `hermes gateway restart`

## License

[MIT](LICENSE)

## Credits

- Built for [Hermes Agent](https://hermes-agent.nousresearch.com) by Nous Research
