# 📚 arXiv Paper Ingest — Hermes Skill (Public Version)

一键自动精读 arXiv/学术 PDF → 生成 Obsidian 笔记 → 导入 Zotero 并智能分类。

> **原作者**: [Your GitHub Name]  
> **适用平台**: [Hermes Agent](https://hermes-agent.nousresearch.com)  
> **Skill 类型**: 自动化工作流 (Academic)

---

## 🌟 功能

| 步骤 | 操作 |
|------|------|
| 1 | 扫描桌面（或指定路径）最新的 PDF 文件 |
| 2 | 用 PyMuPDF 提取全文文字 + 所有嵌入图片 |
| 3 | AI 精读全文，生成结构化 Markdown 笔记 |
| 4 | 图片统一存入共享文件夹，以 `YYYYMMDD_文章简称_FigN.png` 命名 |
| 5 | 笔记写入 Obsidian 知识库（含 Frontmatter 标签 + 作者单位表格） |
| 6 | 通过 Zotero MCP 创建文献条目、导入 PDF、归类、打标签 |
| 7 | 清理桌面原件 |

---

## 🚀 快速开始

### 前置条件

| 依赖 | 说明 |
|------|------|
| [Hermes Agent](https://hermes-agent.nousresearch.com) | 运行本 Skill 的 AI 环境 |
| [Zotero](https://www.zotero.org/) | 文献管理软件，**需保持运行** |
| [Zotero MCP Plugin](https://github.com/cookjohn/zotero-mcp) | 版本 ≥ 1.5.0，**启用写操作** |
| Python 3.10+ | 安装 `pymupdf` (`pip install pymupdf`) |
| Obsidian | 笔记软件（非必需，可改为本地 Markdown 目录） |

### Zotero MCP 配置

1. 在 Zotero 中安装 [Zotero MCP Plugin](https://github.com/cookjohn/zotero-mcp)
2. 进入 Zotero → 工具 → 插件 → 点击插件设置
3. 确保以下首选项已开启：
   - `extensions.zotero.zotero-mcp-plugin.write.enabled` = `true`
   - `extensions.zotero.zotero-mcp-plugin.mcp.server.enabled` = `true`
   - 端口保持默认 `23120`
4. 重启 Zotero，确认端口监听：
   ```bash
   netstat -an | grep 23120
   ```

### 首次设置（仅需一次）

当您第一次说"帮我精读桌面上的paper"时，Hermes 会引导您完成一次性设置：

1. **输入 Obsidian Vault 路径**（例如 `C:\Users\YourName\Obsidian\你的知识库`）
2. **输入 Zotero MCP URL**（默认 `http://127.0.0.1:23120/mcp`）
3. **确认文献笔记存放目录**（例如 `科研内容/文献阅读/`）
4. **确认图片存放目录**（例如 `科研内容/文献阅读/论文配图/`）

设置完成后会**持久化记忆**，后续直接使用。

---

## 📖 使用方式

### 方式一：一键处理桌面最新 PDF

> **"帮我精读桌面上的paper"**  
> **"帮我处理一下新下的文献"**

自动扫描桌面 `.pdf` 文件，选最新的那个执行全流程。

### 方式二：指定路径

> **"帮我精读这篇文献：D:\Downloads\paper.pdf"**

直接传入路径，跳过桌面扫描步骤。

---

## 📝 笔记模板说明

### 文件名

```
YYYYMMDD 论文标题.md
```

示例：`20260615 Octave bandwidth 3D-Printed Couplers for Low-Loss Thin-Film Lithium Tantalate Circuits.md`

### Frontmatter

```yaml
---
tags: [LTOI, 3D打印, 电光调制, 海德堡大学]
created: 2026-06-15
---
```

### 正文结构

```markdown
# YYYYMMDD 论文标题

**arXiv:XXXX.XXXXX** | 日期

## 作者 & 单位

| 作者 | 单位 |
|------|------|
| Erik Jung† (共一) | Heidelberg University |
| **Wolfram Pernice\*** (通信) | Heidelberg University / Univ. Münster |

> **通信作者**: Name — email
> **†共同第一作者**

## 摘要

...

## 1. 研究背景

...

## 2. 实验结果

![Fig.1 描述](论文配图/YYYYMMDD_名称_Fig1.png)

...

## N. 与本课题的关系
```

---

## 🗂️ Zotero 智能分类规则

Skill 会根据论文内容，自动匹配您 Zotero 库中的**分类（Collection）**：

1. 读取您的 Zotero 所有顶层分类
2. 根据论文关键词匹配合适的分类
3. 若无匹配，自动在根目录下创建新分类
4. 标签 ≤ 4 个，优先复用库中已有的标签

> **设计哲学**：不追求精确子分类匹配，论文只要对某个研究方向有启发即可归入。

---

## ⚙️ 自定义配置

Skill 首次运行后会将配置记忆在 Hermes Memory 中。如需修改，可以手动调整以下内容：

| 配置项 | 说明 |
|--------|------|
| `obsidian_vault_path` | Obsidian 知识库根目录 |
| `obsidian_notes_dir` | 笔记存放子路径（如 `科研内容/文献阅读`）|
| `obsidian_images_dir` | 图片存放子路径（如 `科研内容/文献阅读/论文配图`）|
| `zotero_mcp_url` | Zotero MCP 服务地址 |
| `zotero_library_id` | Zotero 库 ID（默认 1）|

---

## 📦 安装到 Hermes

```bash
# 将 SKILL.md 保存到 Hermes skills 目录
# Linux/Mac:
cp arxiv-paper-ingest/arxiv-paper-ingest.md ~/.hermes/skills/

# Windows (PowerShell):
Copy-Item arxiv-paper-ingest\arxiv-paper-ingest.md $env:USERPROFILE\.hermes\skills\
```

然后在 Hermes 中加载：
```bash
hermes skills load arxiv-paper-ingest
```

或直接在聊天中说：
> **"加载 arxiv-paper-ingest 技能"**

---

## 🔧 技术细节

### 架构

```
User Input → Hermes Agent → PyMuPDF (extract) → AI Analysis →
                            ├── Obsidian Note (MD)
                            ├── Images (PNG/JPG)
                            └── Zotero (via MCP/HTTP)
```

### 关键依赖

```bash
pip install pymupdf
# Zotero MCP 通过 HTTP JSON-RPC 通信（无需额外 Python 包）
```

### Zotero MCP API 调用

```python
import urllib.request, json

def mcp_call(method, params=None):
    url = 'http://127.0.0.1:23120/mcp'
    data = json.dumps({'jsonrpc':'2.0','id':1,'method':method,'params':params or {}}).encode('utf-8')
    req = urllib.request.Request(url, data=data, headers={'Content-Type':'application/json'})
    resp = urllib.request.urlopen(req, timeout=30)
    return json.loads(resp.read())

# 示例：创建条目
mcp_call('tools/call', {
    'name': 'write_item',
    'arguments': {
        'action': 'create',
        'itemType': 'journalArticle',
        'fields': {'title': '...', 'date': '...'},
        'creators': [{'creatorType': 'author', 'firstName': '...', 'lastName': '...'}]
    }
})
```

---

## ⚠️ 已知问题

| 问题 | 解决方案 |
|------|---------|
| PDF 导入路径报错 `NS_ERROR_FILE_UNRECOGNIZED_PATH` | 使用 Python raw string `r'C:\Users\...'` 而非 `C:/Users/...` |
| Zotero 写操作失败 | 确认插件 `write.enabled=true` |
| 图片过大（单张可达 7MB+） | 可手动压缩，或增加自动压缩步骤 |
| Zotero import 是复制操作 | 导入成功后需手动删除桌面原件 |

---

## 📄 许可

MIT License — 自由使用、修改、分发。

---

## 🙏 致谢

- [Zotero MCP](https://github.com/cookjohn/zotero-mcp) — 为 AI 代理提供 Zotero 接口
- [PyMuPDF](https://pymupdf.readthedocs.io/) — PDF 解析库
- [Hermes Agent](https://hermes-agent.nousresearch.com) — AI 代理框架
