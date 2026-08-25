# Hermes DeepSeek 网页搜索插件

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![npm version](https://img.shields.io/npm/v/hermes-deepseek-websearch.svg)](https://www.npmjs.com/package/hermes-deepseek-websearch)
[![GitHub stars](https://img.shields.io/github/stars/ashafizullah/hermes-deepseek-websearch.svg)](https://github.com/ashafizullah/hermes-deepseek-websearch/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/ashafizullah/hermes-deepseek-websearch.svg)](https://github.com/ashafizullah/hermes-deepseek-websearch/issues)

> 🇬🇧 [English](README.md) | 🇮🇩 [Bahasa Indonesia](README_ID.md)

一个使用 DeepSeek 原生服务器端网页搜索 API 的 Hermes Agent 插件。

## 功能特点

- **DeepSeek 原生网页搜索** — 使用 DeepSeek 内置的 `web_search` 工具（Responses API）
- **快速可靠** — 结果直接来自 DeepSeek 的搜索基础设施
- **结构化结果** — 返回标题、URL、描述和位置
- **设置简单** — 只需添加 API 密钥并启用插件

## 定价

DeepSeek 网页搜索按调用次数计费（与 token 定价分开）：
- **约 $0.0005 每次搜索** — 非常适合高频使用

## 前提条件

1. **DeepSeek API 密钥**
   - 在 https://platform.deepseek.com 获取

2. **已安装 Hermes Agent**
   - 安装：`curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash`

## 安装

### 方式一：npm（推荐）

```bash
npm install -g hermes-deepseek-websearch
```

### 方式二：手动安装

```bash
# 克隆此仓库
git clone https://github.com/ashafizullah/hermes-deepseek-websearch.git

# 复制插件到 Hermes 插件目录
cp -r hermes-deepseek-websearch ~/.hermes/plugins/web/deepseek
```

### 方式三：直接下载

```bash
# 创建插件目录
mkdir -p ~/.hermes/plugins/web/deepseek

# 下载文件
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/provider.py -o ~/.hermes/plugins/web/deepseek/provider.py
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/__init__.py -o ~/.hermes/plugins/web/deepseek/__init__.py
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/plugin.yaml -o ~/.hermes/plugins/web/deepseek/plugin.yaml
```

## 配置

### 1. 在 `.env` 中添加 API 密钥

```bash
# 添加到 ~/.hermes/.env
DEEPSEEK_API_KEY=sk-你的api密钥
```

### 2. 更新 `config.yaml`

```bash
# 启用插件
hermes config set plugins.enabled '["web/deepseek"]'

# 设置为搜索后端
hermes config set web.search_backend deepseek
```

或手动编辑 `~/.hermes/config.yaml`：

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

### 3. 重启网关

```bash
hermes gateway restart
```

## 使用方法

配置完成后，正常使用 `web_search`：

```python
# 在 Hermes 聊天中
web_search(query="最新新闻", limit=5)
```

## 工作原理

1. Hermes 调用 `web_search` 工具
2. 插件向 DeepSeek Responses API 发送带有 `{"type": "web_search"}` 工具的请求
3. DeepSeek 执行服务器端搜索并以 annotations 形式返回结果
4. 插件解析 annotations 并返回结构化结果

## 故障排除

### 结果为空

1. 查看日志：`tail -f ~/.hermes/logs/gateway.log | grep deepseek`
2. 验证 API 密钥：`grep DEEPSEEK_API_KEY ~/.hermes/.env`
3. 清除缓存：`rm -rf ~/.hermes/plugins/web/deepseek/__pycache__`
4. 重启网关：`hermes gateway restart`

## 许可证

[MIT](LICENSE)

## 致谢

- 为 [Hermes Agent](https://hermes-agent.nousresearch.com) 构建，由 Nous Research 开发
