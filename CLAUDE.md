# CLAUDE.md

這是一支跑在我自己電腦上的 Discord bot：指定頻道有人傳食物照片，就用 `claude -p` 判斷、估熱量，記進 Excel、存照片，並回覆上傳者。

## 開始任何工作前

先讀 `docs/architecture.md`。設計、檔案結構、`claude -p` 的約定和驗證清單都在裡面，需求的來源是 `docs/requirements.md`。

## 四段防護

架構文件第 12 節定義了四段防護（路徑範圍限制、附註只當資料、檔名清洗、Excel 公式防護）。寫程式時：

- 每一段防護都要用清楚的註解框起來，格式固定：

  ```python
  # ===== 防護 N 開始：<一句話說明> =====
  ...
  # ===== 防護 N 結束 =====
  ```

- 一開始四段**全部開啟**，不要預設關掉任何一段。
- 之後教學時會逐段開關來對照效果，所以不要把防護拆散到別處，也不要把兩段混在同一段註解裡。

## 不可以 commit 的東西

- 不可以把 `.env` commit 進 git。裡面有 `DISCORD_TOKEN`。只有沒有真實值的 `.env.example` 可以 commit。
- `DATA_DIR` 裡的照片和 Excel（`records.xlsx`、`photos/`、`state.db`）也不要 commit，那是別人吃了什麼的隱私資料。
- 任何地方都不要把 token 印在 log、錯誤訊息或回覆裡。
