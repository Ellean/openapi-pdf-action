# OpenAPI to PDF Action

[![Convert OpenAPI to PDF](https://github.com/Ellean/openapi-pdf-action/actions/workflows/convert.yml/badge.svg)](https://github.com/Ellean/openapi-pdf-action/actions/workflows/convert.yml)

使用 widdershins + pandoc 通过 GitHub Actions 将 OpenAPI 规范转换为 PDF 文档。

Convert OpenAPI specifications to PDF documentation using widdershins and pandoc via GitHub Actions.

## 功能特性 / Features

- 🔄 自动转换 OpenAPI 规范为 PDF 文档 / Automatically convert OpenAPI specs to PDF
- 📁 支持多个 OpenAPI 文件 / Support multiple OpenAPI files
- ⚙️ 可配置的转换选项 / Configurable conversion options
- 📦 生成可下载的 artifact / Generate downloadable artifacts
- 🔧 可作为可重用的 GitHub Action / Reusable as a GitHub Action

## 使用方法 / Usage

### 作为独立仓库使用 / As a Standalone Repository

1. **添加 OpenAPI 文件**: 将您的 OpenAPI 规范文件（`.yaml`, `.yml`, 或 `.json`）放入 `/api` 目录

2. **配置转换选项**:
   - `widdershins.conf`: Widdershins 转换配置
   - `pandoc.conf`: Pandoc PDF 生成配置

3. **触发转换**: 推送代码或创建标签时，GitHub Actions 将自动运行转换流程

4. **下载 PDF**: 在 Actions 页面的 Artifacts 中下载生成的 PDF 文档

### 作为可重用 Action 使用 / As a Reusable Action

在您的工作流中使用此 Action：

```yaml
name: Generate API Documentation

on:
  push:
    branches:
      - main

jobs:
  generate-docs:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Convert OpenAPI to PDF
        uses: Ellean/openapi-pdf-action@main
        with:
          api-dir: 'api'
          widdershins-config: 'widdershins.conf'
          pandoc-config: 'pandoc.conf'
          output-file: 'api-documentation.pdf'
      
      - name: Upload PDF
        uses: actions/upload-artifact@v4
        with:
          name: api-docs
          path: output/*.pdf
```

## 配置说明 / Configuration

### Widdershins 配置 / Widdershins Configuration

`widdershins.conf` 文件控制 OpenAPI 到 Markdown 的转换选项：

```json
{
  "language_tabs": [
    { "shell": "Shell" },
    { "javascript": "JavaScript" },
    { "python": "Python" }
  ],
  "codeSamples": true,
  "theme": "darkula",
  "search": true
}
```

### Pandoc 配置 / Pandoc Configuration

`pandoc.conf` 文件控制 Markdown 到 PDF 的转换选项：

```json
{
  "pdf-engine": "xelatex",
  "variable": [
    "geometry:margin=1in",
    "documentclass:article",
    "fontsize:11pt"
  ],
  "table-of-contents": true,
  "toc-depth": 3,
  "number-sections": true,
  "highlight-style": "tango"
}
```

## Action 输入参数 / Action Inputs

| 参数 / Input | 描述 / Description | 必需 / Required | 默认值 / Default |
|--------------|-------------------|-----------------|------------------|
| `api-dir` | OpenAPI 文件目录 / Directory with OpenAPI files | No | `api` |
| `widdershins-config` | Widdershins 配置文件 / Widdershins config file | No | `widdershins.conf` |
| `pandoc-config` | Pandoc 配置文件 / Pandoc config file | No | `pandoc.conf` |
| `output-file` | 输出 PDF 文件名 / Output PDF filename | No | `api-documentation.pdf` |

## Action 输出 / Action Outputs

| 输出 / Output | 描述 / Description |
|--------------|-------------------|
| `pdf-path` | 生成的 PDF 文件路径 / Path to generated PDF |

## 目录结构 / Directory Structure

```
.
├── api/                    # OpenAPI 规范文件 / OpenAPI specification files
│   └── example.yaml
├── widdershins.conf        # Widdershins 配置 / Widdershins configuration
├── pandoc.conf            # Pandoc 配置 / Pandoc configuration
├── .github/
│   └── workflows/
│       └── convert.yml    # 转换工作流 / Conversion workflow
└── action.yml             # Action 定义 / Action definition
```

## 许可证 / License

MIT

## 参考 / References

- [Widdershins](https://github.com/Mermade/widdershins)
- [Pandoc](https://pandoc.org/)
- [Pandoc Action Example](https://github.com/pandoc/pandoc-action-example)