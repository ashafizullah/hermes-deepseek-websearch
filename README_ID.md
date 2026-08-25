# Plugin Web Search DeepSeek untuk Hermes

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![npm version](https://img.shields.io/npm/v/hermes-deepseek-websearch.svg)](https://www.npmjs.com/package/hermes-deepseek-websearch)
[![GitHub stars](https://img.shields.io/github/stars/ashafizullah/hermes-deepseek-websearch.svg)](https://github.com/ashafizullah/hermes-deepseek-websearch/stargazers)
[![GitHub issues](https://img.shields.io/github/issues/ashafizullah/hermes-deepseek-websearch.svg)](https://github.com/ashafizullah/hermes-deepseek-websearch/issues)

> 🇬🇧 [English](README.md) | 🇨🇳 [中文](README_ZH.md)

Plugin Hermes Agent yang menggunakan API web search native dari DeepSeek.

## Fitur

- **Web Search Native DeepSeek** — Menggunakan tool `web_search` built-in DeepSeek pada Responses API
- **Cepat & Stabil** — Hasil langsung dari infrastruktur search DeepSeek
- **Hasil Terstruktur** — Mengembalikan judul, URL, deskripsi, dan posisi
- **Mudah Setup** — Tinggal tambah API key dan aktifkan plugin

## Harga

Web Search DeepSeek dibayar per panggilan (terpisah dari harga token):
- **~$0.0005 per pencarian** — Sangat terjangkau untuk penggunaan volume tinggi

## Syarat

1. **API Key DeepSeek**
   - Daftar di https://platform.deepseek.com

2. **Hermes Agent** terinstall
   - Install: `curl -fsSL https://raw.githubusercontent.com/NousResearch/hermes-agent/main/scripts/install.sh | bash`

## Instalasi

### Opsi 1: npm (Direkomendasikan)

```bash
npm install -g hermes-deepseek-websearch
```

### Opsi 2: Manual

```bash
# Clone repo ini
git clone https://github.com/ashafizullah/hermes-deepseek-websearch.git

# Copy plugin ke direktori Hermes
cp -r hermes-deepseek-websearch ~/.hermes/plugins/web/deepseek
```

### Opsi 3: Langsung Download

```bash
# Buat direktori plugin
mkdir -p ~/.hermes/plugins/web/deepseek

# Download file
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/provider.py -o ~/.hermes/plugins/web/deepseek/provider.py
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/__init__.py -o ~/.hermes/plugins/web/deepseek/__init__.py
curl -sL https://raw.githubusercontent.com/ashafizullah/hermes-deepseek-websearch/main/plugin.yaml -o ~/.hermes/plugins/web/deepseek/plugin.yaml
```

## Konfigurasi

### 1. Tambah API Key ke `.env`

```bash
# Tambahkan ke ~/.hermes/.env
DEEPSEEK_API_KEY=sk-api-key-kamu
```

### 2. Update `config.yaml`

```bash
# Aktifkan plugin
hermes config set plugins.enabled '["web/deepseek"]'

# Set sebagai search backend
hermes config set web.search_backend deepseek
```

Atau edit manual `~/.hermes/config.yaml`:

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

## Penggunaan

Setelah dikonfigurasi, gunakan `web_search` seperti biasa:

```python
# Di chat Hermes
web_search(query="berita terbaru Indonesia", limit=5)
```

## Cara Kerja

1. Hermes memanggil tool `web_search`
2. Plugin mengirim request ke DeepSeek Responses API dengan tool `{"type": "web_search"}`
3. DeepSeek melakukan pencarian server-side dan mengembalikan hasil sebagai annotations
4. Plugin mem-parsing annotations dan mengembalikan hasil terstruktur

## Troubleshooting

### Hasil kosong

1. Cek log: `tail -f ~/.hermes/logs/gateway.log | grep deepseek`
2. Verifikasi API key: `grep DEEPSEEK_API_KEY ~/.hermes/.env`
3. Hapus cache: `rm -rf ~/.hermes/plugins/web/deepseek/__pycache__`
4. Restart gateway: `hermes gateway restart`

## Lisensi

[MIT](LICENSE)

## Kredit

- Dibuat untuk [Hermes Agent](https://hermes-agent.nousresearch.com) oleh Nous Research
