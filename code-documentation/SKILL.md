---
name: code-documentation
description: >
  自動為 Python 函數加入詳細的中文註釋與設計說明，
  讓 code 本身成為長期可讀的知識載體，
  降低換對話框、換模型、debug、handover 時的理解斷層。

  This skill should be triggered when:
  - 阿寶生成任何新的 Python 函數
  - 阿寶修改既有 Python 函數
  - 專案進入 debug / refactor / handover 階段
  - 使用者明示要求「把這段註清楚」
---

# code-documentation

## 核心概念

這個技能不是單純把 code 加上漂亮註釋。
它的目標是：

> **把原本只存在聊天記錄裡的設計意圖、限制條件、修補理由，直接寫進程式碼本體。**

當 Claude 寫 code 時，最容易流失的不是 function 本身，而是：

- 為什麼要這樣設計
- 哪些地方不能亂改
- 哪些 workaround 是被 bug 逼出來的
- 哪些參數 / 步驟順序其實有 hidden dependency

所以這個技能要求 Claude 在生成或修改 Python code 時，
**同步寫出真正有價值的中文 docstring 與 inline 註解**，
讓未來的 Claude / 使用者 / 協作者只看 code 也能恢復設計脈絡。

---

## 啟用後的行為規則

Claude 在以下情境下，應自動進入「自文件化」模式：

### 1. 新寫函數
任何新生成的 Python 函數，都應至少補上：
- 函數用途
- 主要輸入 / 輸出
- 核心流程摘要
- 有無副作用 / 狀態變更

### 2. 修改舊函數
如果是修改既有函數，除了更新 docstring，還應補上：
- 這次改動的原因
- 和舊行為相比差在哪裡
- 是否影響其他流程

### 3. Debug 修補
若這段修改是為了修 bug，必須明示：
- bug 觸發條件
- 修補邏輯
- 為何採這個方案
- 哪些錯誤做法不要再犯

### 4. 關鍵流程 / 難懂邏輯
若 code 涉及以下類型，應增加區塊級註解：
- 狀態機 / flow control
- prompt 組裝
- API bridge
- 格式轉換
- 容錯 / fallback
- cache / persistence
- 魔法數字或歷史相容邏輯

---

## 註釋原則

### 要寫什麼
Claude 應優先寫出：
- **設計意圖**：為什麼這段存在
- **限制條件**：什麼不能改，改了會怎樣
- **依賴關係**：和前後哪些流程相扣
- **修補原因**：這是不是被 bug / platform 限制逼出來的

### 不要寫什麼
不要寫這種廢話型註釋：
- `# 初始化變數`
- `# 進入迴圈`
- `# return 結果`

因為這些只是把 code 翻成中文，
沒有保存任何真正會在未來斷 context 的知識。

---

## 建議輸出格式

### 函數 docstring 範例

```python
def build_payload(user_text: str, image_url: str | None) -> dict:
    """
    建立要送往生成模型的 payload。

    設計意圖：
    - 把 text 與 image 組裝邏輯集中在同一處，避免上游多處重複判斷。
    - 允許 image_url 為空，因為舊資料流不一定附圖。

    注意：
    - 這裡不能強制要求 image_url 存在，否則會讓舊版資料在 replay 時直接失敗。
    - 若後續 payload schema 變動，應同步更新下游解析器。
    """
```

### 區塊註解範例

```python
# 先做最小清洗，再保留使用者原句。
# 這裡不能做激進正規化，否則 prompt 風格會被洗掉。
cleaned = sanitize_user_text(raw_text)
```

### Debug 註解範例

```python
# Bugfix:
# 先前 payload 缺少 image_url 時會直接 KeyError。
# 改用 .get() 並在缺失時走 fallback，避免舊資料 replay 失敗。
```

---

## Claude 的執行流程

每當 Claude 產生或修改 Python code 時，應默默跑一次以下檢查：

1. 這是不是新函數？
2. 這段邏輯未來最容易讓人看不懂的點在哪？
3. 哪些設計知識現在只存在聊天裡？
4. 哪些限制條件如果不寫，下次一定會踩雷？
5. 這次是不是在修 bug？如果是，原因有沒有留下？

然後把答案轉成：
- docstring
- block comment
- inline comment

直接寫進 code。

---

## 和其他技能的分工

### 它負責
- 把局部設計知識鎖進程式碼本體
- 提高單一檔案 / 單一函數的可讀性
- 降低未來 debug / handover 的理解成本

### 它不負責
- 專案整體進度摘要
- 今天做了哪些任務
- 跨檔案決策總表
- 下一位協作者的接手說明

那些屬於 `living-handover` 的範圍。

---

## 成功判準

這個技能做得好時，應達成：

- 重要 Python 函數有清楚中文 docstring
- 關鍵流程前有真正有價值的註釋
- bug 修補有留下原因，不再只剩神祕 patch
- 換新對話框後，只看 code 也能恢復大部分脈絡

---

## 一句話提醒 Claude

> 你寫的不只是註釋，你是在替未來會失憶的協作者留下可恢復上下文的錨點。
