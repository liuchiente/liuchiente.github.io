---
title: "NotebookLM × Claude 完整整合教學（MCP 實戰版）"
date: 2026-05-24
draft: false
tags: ["NotebookLM", "Claude", "MCP", "AI", "Claude Code", "GitHub Copilot"]
description: "從零開始，在 macOS 上透過 MCP 整合 NotebookLM 與 Claude Desktop、Claude Code、GitHub Copilot，含所有實際踩坑修正步驟。"
---

# NotebookLM × Claude 完整整合教學（MCP 實戰版）

> **適用環境：** macOS（Apple Silicon / Intel）、Python 3.11+、zsh  
> **整合目標：** Claude Desktop（Cowork）、Claude Code、GitHub Copilot  
> **套件：** [notebooklm-mcp-cli](https://github.com/jacob-bd/notebooklm-mcp-cli)（社群維護，非 Google 官方）

---

## 整體架構

```
NotebookLM（知識庫：PDF / Word / txt / 網址）
        ↓
  notebooklm-mcp-cli（MCP Server — 橋接層）
        ↓
  Claude Desktop（統一設定入口）
     ↙          ↓          ↘
Claude Code  Cowork   GitHub Copilot
```

**核心概念：** MCP（Model Context Protocol）讓 AI 工具可以即時呼叫外部服務。只要在 Claude Desktop 設定好一個 MCP server，Claude Code、Copilot 等工具都能共用同一個 NotebookLM 連線，不需要重複設定。

---

## 一、事前確認

```bash
# 確認 Python 版本（需要 3.11 以上）
python3 --version

# 確認是哪個版本的 pip 在使用
pip3 --version
```

> ⚠️ **macOS Homebrew 常見陷阱：** 如果你的系統同時裝了多個 Python 版本，`pip3` 可能指向舊版（例如 3.9 或 3.10），導致安裝失敗。後面的安裝步驟會處理這個問題。

---

## 二、安裝 notebooklm-mcp-cli

### macOS Homebrew Python 環境（正確做法）

如果你是用 Homebrew 安裝的 Python（例如 `python@3.14`），直接用 `pip3` 可能會遇到兩個問題：

1. **版本衝突**：`pip3` 指向舊版 Python，找不到符合需求的套件版本
2. **環境保護**：Homebrew Python 預設不允許直接 `pip install`，需要加旗標

**正確安裝指令：**

```bash
# 明確指定使用 Homebrew 安裝的 Python 版本（以 3.14 為例，依你的版本調整）
python3.14 -m pip install notebooklm-mcp-cli --user --break-system-packages
```

安裝完成後會看到類似這樣的輸出（正常）：
```
Successfully installed notebooklm-mcp-cli-0.6.11 fastmcp-3.3.1 mcp-1.27.1 ...
```

### 加入 PATH（重要！）

Homebrew Python 的 `--user` 安裝位置與一般不同，需要手動加入 PATH：

```bash
# 先確認你的 Python user base 路徑
python3.14 -m site --user-base
# 通常輸出：/Users/你的帳號/Library/Python/3.14
```

把 `bin` 目錄加入 PATH，並讓設定永久生效：

```bash
echo 'export PATH="$HOME/Library/Python/3.14/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

> 如果你用的是 Python 3.11 / 3.12 / 3.13，把上面的 `3.14` 換成對應版本號。

確認安裝成功：

```bash
nlm --version
```

---

## 三、不適用的安裝方式（避免踩坑）

| 方式 | 問題 |
|------|------|
| `npm install -g notebooklm-mcp-cli` | ❌ 此套件只在 PyPI，npm 上不存在 |
| `pipx install notebooklm-mcp-cli` | ⚠️ 若 `~/.local` 權限不對會失敗（Permission denied） |
| `pip3 install notebooklm-mcp-cli` | ⚠️ pip3 可能指向舊版 Python，導致找不到符合版本 |

**pipx 權限問題修復方式（若你堅持用 pipx）：**

```bash
sudo chown -R $(whoami) ~/.local
mkdir -p ~/.local/pipx
pipx install notebooklm-mcp-cli
```

---

## 四、Google 帳號授權

```bash
nlm login
```

執行後會自動開啟瀏覽器，登入你的 Google 帳號並授權 NotebookLM 存取。

確認授權狀態：

```bash
nlm doctor
# 若顯示 ✅ Auth: OK 表示成功
```

---

## 五、設定 Claude Desktop（MCP 核心設定）

### 注意：`claude-desktop` 已不是有效的 client 名稱

執行以下指令確認目前支援的 client 清單：

```bash
nlm setup add
# 顯示：Available clients: claude-code, gemini, cursor, github-copilot,
#        windsurf, cline, antigravity, codex, opencode, json, all
```

`claude-desktop` 不在清單裡，需要改用 `json` 選項手動設定。

### 步驟 1：產生 JSON 設定內容

```bash
nlm setup add json
```

互動式選單：
- **Config type** → 選 `2`（Regular，使用已安裝的版本）
- **Command format** → 選 `1`（command name，預設值）

輸出結果：
```json
{
  "notebooklm-mcp": {
    "command": "notebooklm-mcp"
  }
}
```

### 步驟 2：寫入 Claude Desktop 設定檔

設定檔位置（macOS）：
```
~/Library/Application Support/Claude/claude_desktop_config.json
```

開啟設定檔：
```bash
open ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

若檔案不存在，先建立：
```bash
mkdir -p ~/Library/Application\ Support/Claude
touch ~/Library/Application\ Support/Claude/claude_desktop_config.json
open ~/Library/Application\ Support/Claude/claude_desktop_config.json
```

貼入以下完整內容（若已有其他 MCP server，只新增 `notebooklm-mcp` 區塊）：

```json
{
  "mcpServers": {
    "notebooklm-mcp": {
      "command": "notebooklm-mcp"
    }
  }
}
```

### 步驟 3：完全關閉並重啟 Claude Desktop

重啟後，在 Claude 的工具列就能看到 NotebookLM 相關工具。可以直接說：

```
列出我在 NotebookLM 的所有 Notebook
```

Claude 會自動呼叫 MCP 回傳你的知識庫清單。

---

## 六、設定其他工具

### Claude Code

```bash
nlm setup add claude-code
```

### GitHub Copilot（VS Code）

```bash
nlm setup add github-copilot
```

設定完後重啟 VS Code，在 Copilot Chat 中就能存取 NotebookLM。

### 一次設定所有支援的工具

```bash
nlm setup add all
```

---

## 七、在 NotebookLM 建立知識庫

在使用 Claude 查詢前，先到 [notebooklm.google.com](https://notebooklm.google.com) 建立知識庫：

1. 點選「新增 Notebook」
2. 上傳資料來源：
   - **PDF**：直接拖放（最大 500MB）
   - **Word（.docx）**：直接上傳
   - **文字檔（.txt / .md）**：直接上傳
   - **網址**：貼上 URL，自動抓取內容
   - **Google Drive**：支援 Docs / Slides
3. 等待索引完成（約 30 秒至 2 分鐘）

> ⚠️ **Excel 注意事項：** NotebookLM 不直接支援 `.xlsx`。如果要用 Excel 資料，建議另存為 CSV 再上傳，或直接在 Claude in Excel 插件中處理數值運算，讓 NotebookLM 專門管理文字型知識庫。

---

## 八、實際使用範例

### Claude Desktop / Cowork

```
你：列出我所有的 NotebookLM Notebook
你：從「專案規格書」Notebook 中，整理出所有 API endpoint 清單
你：比較「Phase 3」和「Phase 4」兩個 Notebook 的功能差異
```

### Claude Code（程式化應用）

```
從我的 NotebookLM「產品規格書」提取所有欄位定義，
整理成 TypeScript interface 格式
```

### GitHub Copilot Chat（VS Code 內）

```
@notebooklm 查詢「系統規格」Notebook 中關於錯誤碼的定義
```

### Claude in Excel

在 Excel 插件對話框中：

```
從我的 NotebookLM「市場報告」Notebook 找出各產品的成長率，
對照 A 欄的產品名稱，填入 B 欄
```

---

## 九、常用 CLI 指令速查

```bash
# 查看所有 Notebook
nlm notebook list

# 建立新 Notebook
nlm notebook create --title "我的研究筆記"

# 新增來源
nlm source add --notebook-id <ID> --file report.pdf
nlm source add --notebook-id <ID> --url https://example.com

# 查詢內容
nlm query --notebook-id <ID> "請摘要這份文件的重點"

# 環境診斷
nlm doctor

# 查看所有工具設定狀態
nlm setup list

# 更新套件
python3.14 -m pip install --upgrade notebooklm-mcp-cli --user --break-system-packages
```

---

## 十、故障排除

| 問題 | 原因 | 解決方式 |
|------|------|---------|
| `zsh: command not found: nlm` | PATH 未設定 | `echo 'export PATH="$HOME/Library/Python/3.14/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc` |
| `pip3 install` 找不到版本 | pip3 指向舊版 Python | 改用 `python3.14 -m pip install ...` |
| `pipx: Permission denied` | `~/.local` 權限問題 | `sudo chown -R $(whoami) ~/.local` |
| `nlm setup add claude-desktop` 失敗 | client 名稱已更新 | 改用 `nlm setup add json` 手動設定 |
| Claude Desktop 看不到工具 | JSON 格式錯誤或未重啟 | 確認 JSON 正確，完全關閉後重開 |
| `nlm doctor` 顯示 Auth 失敗 | Token 過期 | 重新執行 `nlm login` |
| MCP 突然失效 | Google 更新了內部介面 | `python3.14 -m pip install --upgrade notebooklm-mcp-cli --user --break-system-packages` |

---

## 重要聲明

> **notebooklm-mcp-cli 是非官方社群套件**，透過瀏覽器自動化與 Google 未公開的內部 API 運作，與 Google 無任何關聯。
>
> Google 若更新 NotebookLM 介面，可能導致套件暫時失效。建議用於個人研究、原型測試等非關鍵性用途。生產環境請等待 Google 官方 API。

---

## 參考資源

- [notebooklm-mcp-cli GitHub](https://github.com/jacob-bd/notebooklm-mcp-cli)
- [notebooklm-mcp-cli PyPI](https://pypi.org/project/notebooklm-mcp-cli/)
- [Google NotebookLM](https://notebooklm.google.com)
- [Claude MCP 文件](https://docs.claude.com)

---

*發布日期：2026 年 5 月 24 日*
