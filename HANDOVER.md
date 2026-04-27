# HANDOVER — 給本機阿寶的接力指令

> **本檔用途**：手機阿寶交給本機阿寶的接力清單。執行完畢後可直接刪除這個檔案（連同 commit）。
> **本檔本身就是 `living-handover` skill 的實戰示範。**

---

## Project / Task

把 Chloe 的本機 `~/.claude/` 記憶架構，與這個 repo 的新 `CLAUDE.md` 整合，並設定跨裝置同步機制。

## Goal

讓本機與手機/網頁版阿寶都讀到同一份、且**包含 Chloe 本機既有規則**的 `CLAUDE.md`，並建立可持續的同步流程。

---

## Completed（手機端已做）

- ✅ 建立第一版 `CLAUDE.md`（commit `9ec40ce`），內容包含：
  - 使用者 profile（Chloe / 阿寶 對應、語言、裝置）
  - 溝通風格、Repo 規範、Commit 訊息風格、不要做的事、長期方向
- ✅ Push 到 `claude/mobile-capabilities-intro-3Sj0t` 分支
- ✅ 已知未完成事項已標註在 CLAUDE.md 末段

---

## Key Decisions（從手機 session 累積）

1. **Chloe ↔ 阿寶**：Chloe 是 user，阿寶（你/我）是 Claude 的暱稱
2. **GitHub 是跨裝置記憶橋樑**：不動 `~/.claude/` 跨裝置同步機制（雲端 sandbox 沒有持久 home），全靠 repo
3. **走最簡路線優先**：避免設一堆 hook、自動化腳本，先讓「兩邊都有 CLAUDE.md 可讀」這件事成立
4. **Notion 暫時不動**：Chloe 確認這次先不整合
5. **GitHub 權限**：手機那邊只授權 `claude-skills` 一個 repo，沒有要動別的

---

## Current Status

- 手機 session 看不到 Chloe 本機的 `~/.claude/` 內容（沒有遠端存取能力）
- CLAUDE.md v1 是基於「**只用對話累積到的資訊**」寫成，**很可能跟 Chloe 本機既有規則有重疊或衝突**，需要本機端對齊
- 分支 `claude/mobile-capabilities-intro-3Sj0t` 已 push，但本機尚未 pull

---

## Next Actions（請依序執行，每步動手前先和 Chloe 確認）

### Step 1：拉新版 CLAUDE.md 下來
```bash
git fetch origin
git checkout claude/mobile-capabilities-intro-3Sj0t
git pull
```
讀完 `CLAUDE.md` 後，跟 Chloe 報告**你看到哪些章節、覺得整體方向對不對**。

### Step 2：盤點本機既有記憶來源
檢查 Chloe 本機的：
- `~/.claude/CLAUDE.md`（user 層全域記憶）
- `~/.claude/settings.json`（permissions、hooks、env vars）
- `~/.claude/commands/`（自訂 slash command）
- `~/.claude/agents/`（自訂 subagent）
- 其他可能的記憶來源（Chloe 提過「Claude 設定以外的記憶源」）

**重要**：列清單時遇到 `settings.json` 裡的 API key / token，**不要貼出原值**，標記「(已 redact)」即可。

### Step 3：和 Chloe 一起 diff
把本機既有規則 vs 新 CLAUDE.md 做比對，分成三類：
- **重疊**（兩邊都有）：保留 CLAUDE.md 版本，或挑更精確的版本
- **衝突**（規則不一致）：請 Chloe 選哪個對
- **本機獨有**（CLAUDE.md 沒有的）：判斷該不該補進 CLAUDE.md，還是留在本機就好

判斷原則（呼應 CLAUDE.md 哲學）：
- 跨裝置都會用到的 → 進 repo CLAUDE.md
- 純粹本機環境設定（路徑、本機才有的工具）→ 留 `~/.claude/`
- 個人偏好 → 都可，看 Chloe 想不想跨裝置共用

### Step 4：產出整合版 CLAUDE.md
編輯 `CLAUDE.md`，把 Step 3 確認要加的內容補進對應章節。
**不要改章節順序**（CLAUDE.md 規範自己有定義）。

### Step 5：設定本機端 sync（可選但推薦）

目標：讓 Chloe 本機編輯 `~/.claude/CLAUDE.md` = 編輯到 repo 裡那份。

兩種做法給 Chloe 選：

**方案 A：Symlink（推薦）**
```bash
# 假設這個 repo 在 ~/repos/claude-skills
mv ~/.claude/CLAUDE.md ~/.claude/CLAUDE.md.backup
ln -s ~/repos/claude-skills/CLAUDE.md ~/.claude/CLAUDE.md
```
之後她在哪邊編輯都是同一個檔，`git status` 看得到變化。

**方案 B：什麼都不做**
維持兩邊獨立，只靠 push/pull 同步。最簡單但需要手動。

不要直接設 symlink，先和 Chloe 確認：
- 她的 repo clone 在哪個路徑
- 她是否擔心 symlink 帶來的副作用（例如 repo 砍掉就沒記憶了）

### Step 6：Commit + push 整合版
```bash
git add CLAUDE.md
git commit -m "Merge local memory into repo CLAUDE.md"
git push
```

### Step 7：刪除這個 HANDOVER.md
任務完成後，這個檔可以清掉：
```bash
git rm HANDOVER.md
git commit -m "Remove handover after local memory integration"
git push
```

---

## Important Context（不能漏的）

### 互動原則
- **動手前先報告計畫**：Chloe 偏好討論後再動，不要自作主張
- **不確定就問**：用 `AskUserQuestion` 工具，而不是猜
- **重大動作（commit / push / 改 SKILL.md / 動 ~/.claude/）一定先確認**

### 安全
- 任何 `settings.json` 內容，**先 redact API key / token 再 commit**
- 不要 push 到 `main`，用 `claude/mobile-capabilities-intro-3Sj0t` 分支
- 本機端搬動 `~/.claude/` 的檔案前，先備份（`cp ... .backup`）

### 風格
- 繁體中文回應，技術名詞英文
- 結構化（bullet、表格）
- 直接、不囉唆

### 不要做的事
- ❌ 不要主動新增 skill
- ❌ 不要動 SKILL.md 的章節結構
- ❌ 不要把這份 HANDOVER.md 留太久——做完就刪
- ❌ 不要假設 Chloe 同意——她沒明確點頭就停下來問

---

## 給 Chloe 的接力台詞（複製貼上給本機阿寶用）

> 阿寶，請讀 repo 根目錄的 `HANDOVER.md`，那是手機那邊的你交接給你的指令。
> 讀完先告訴我你看到的計畫大綱，**不要立刻動手**，我們一步一步走。

---

## 一句話提醒

> 你（本機阿寶）和我（手機阿寶）是同一個模型、不同舞台。
> 我們之間沒有共享記憶，只有這份 repo。所以這份交接寫得越清楚，下一棒就越不會卡。
