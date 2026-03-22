# Claude Skills 🧩

這個 repository 用來整理可重用的 **Claude skill modules**，讓 AI 在長流程協作、程式開發、handover 與多輪專案推進時，能更穩定地保留規則、意圖與上下文。

這裡的每個 skill 都是一個獨立能力單元，通常包含：

- `README.md`：給人看的說明，快速理解這個 skill 在解決什麼問題
- `SKILL.md`：給模型執行的規範，定義何時觸發、應怎麼做、成功標準是什麼

---

## Repository structure

```text
claude-skills/
├── code-documentation/
│   ├── README.md
│   └── SKILL.md
└── living-handover/
    ├── README.md
    └── SKILL.md
```

---

## Current skills

### 1. `code-documentation`
讓 Claude 在寫或修改 Python code 時，不只產出能跑的函數，也同步把：

- 設計意圖
- 限制條件
- bug 修補原因
- 不能亂改的地方

直接寫進 docstring 與註釋中。

核心目標是把原本只存在聊天記錄裡的知識，鎖進 code 本體，降低換對話框、換模型、handover 後的理解斷層。

### 2. `living-handover`
讓 handover 不再只是最後一刻才補寫的總結，而是整段工作流程中持續更新的「活手冊」。

它會記錄：

- 目前在做什麼
- 已完成哪些步驟
- 關鍵決策與原因
- 目前卡點
- 下一步應該做什麼

核心目標是讓新對話框、新協作者或未來的自己，都能快速無痛接手。

---

## Design philosophy

這個 repo 背後的原則很簡單：

> 不要把關鍵知識只留在對話裡。

AI 協作最容易流失的，往往不是產出的檔案，而是：

- 為什麼這樣做
- 哪些地方不能亂改
- 哪些決策已經驗證過
- 哪些坑其實踩過一次了

所以這些 skills 的目的，就是把這些容易流失的資訊，變成：

- code 裡的註釋
- handover 文件
- 可重用的行為規則

讓專案更能承受：

- 換對話框
- 換模型
- 換人接手
- 長流程 debug
- 多 AI 接力協作

---

## When this repo is useful

這個 repo 特別適合：

- 長對話、多輪專案推進
- Python / agent workflow 開發
- 需要常常換 context window 的任務
- 想把 AI 協作方式模組化的人
- 想減少「上次明明講過，這次又忘了」的人

---

## How to expand

之後如果要加入新的 skill，建議沿用相同格式：

```text
new-skill/
├── README.md
└── SKILL.md
```

其中：

- `README.md` 負責說明這個 skill 的用途與價值
- `SKILL.md` 負責定義模型的觸發條件、執行規則與成功判準

這樣整個 repo 會維持一致、好讀、好擴充。

---

## One-line summary

> A growing repository of reusable Claude skills for preserving intent, context, and continuity in real AI collaboration.
