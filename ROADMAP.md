# Claude Skills Roadmap 🌱

這份 roadmap 用來規劃這個 repo 接下來可以擴充的方向，
讓 `claude-skills` 不只是兩個零散 skill，而是逐步長成一套可重用的 AI collaboration skill library。

---

## Current state

目前已完成：

- `code-documentation`
- `living-handover`

它們分別解決：

- **局部知識保存**：把設計意圖寫進 code
- **全局狀態保存**：把專案脈絡寫進 handover

這兩個其實已經形成一組很好的基礎骨架：

> 一個負責把知識鎖在程式裡，另一個負責把進度鎖在流程裡。

---

## Next recommended skills

### 1. `debug-forensics`
**定位問題真正原因的 skill**

用途：
- 區分 symptom 和 root cause
- 要求 Claude 在修 bug 前先列出假設
- 避免一上來就亂 patch
- 記錄哪些方案已試過、哪些失敗過

適合情境：
- 多輪 debug
- API / deployment / integration 問題
- 明明修了但又復發的 bug

---

### 2. `prompt-contract`
**固定 prompt 規格與不可漂移區域的 skill**

用途：
- 保護核心 prompt 結構不被每輪對話洗掉
- 明確標記哪些段落是可調、哪些不能亂改
- 讓 prompt 迭代有版本感

適合情境：
- agent prompt
- system / role prompt
- 長流程創作 prompt
- 多模型共用的提示詞規格

---

### 3. `artifact-packaging`
**把成果整理成交付物的 skill**

用途：
- 自動整理 output 成 repo / zip / demo submission 可用格式
- 檢查檔名、結構、缺漏檔案
- 生成 handoff summary / delivery checklist

適合情境：
- hackathon 提交
- PRD / demo / repo 打包
- 多檔案交付前整理

---

### 4. `decision-log`
**保存關鍵決策與選型理由的 skill**

用途：
- 記錄為什麼選 A 不選 B
- 保留 tradeoff 與限制條件
- 讓未來回看時不會只剩結論沒有理由

適合情境：
- 技術選型
- 架構調整
- prompt / workflow 分歧選擇

---

### 5. `skill-template`
**新 skill 的標準骨架模板**

用途：
- 提供統一格式
- 降低新增 skill 的摩擦
- 確保每個 skill 都有一致結構

建議內容：
- `README.md` 模板
- `SKILL.md` 模板
- naming guideline
- success criteria checklist

---

## Expansion priorities

如果要照實用性排序，我會建議：

### Phase 1 — 基礎穩定層
1. `debug-forensics`
2. `decision-log`
3. `skill-template`

### Phase 2 — 協作控制層
4. `prompt-contract`
5. `artifact-packaging`

### Phase 3 — 進階延伸層
之後可以再考慮：
- `repo-onboarding`
- `multi-agent-routing`
- `spec-to-build`
- `postmortem-review`
- `evaluation-checklist`

---

## What “good growth” looks like

這個 repo 如果長得健康，未來應該會呈現這種狀態：

- 每個 skill 都有明確職責
- skills 之間邊界清楚，不互相打架
- README 給人看，SKILL 給模型跑
- 可以按需求拼裝使用，而不是每次從頭重新教 AI

也就是說，它最後應該像一套：

> **可重用的 AI collaboration operating system modules**

而不是一堆零散筆記。

---

## One-line direction

> Build this repo into a modular skill library for preserving continuity, reasoning quality, and delivery discipline across real AI projects.
