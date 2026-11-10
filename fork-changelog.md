# Fork 變更紀錄

比較基準為 `mmnavarr/overseer` 的 `main` 與 `KaKuCoCo/overseer` 的 `windows`。以下按時間列出共同祖先之後 fork 分支的所有提交，不包含共同祖先以前的 upstream 提交。日期為 Git 作者日期。

前兩項移植自 upstream 的 `windows-support` 分支，並非 fork 原創。來源依 fork 的 `CHANGELOG.md` 與 upstream 分支提交訊息辨識。

## 2026-10-13

- 移植原生 Windows 專案與 worktree 支援。
  - 包含 PowerShell 資料夾選擇器、worktree 操作、路徑處理和測試。
  - 讓原本限定 macOS 的功能可在 Windows 使用。

## 2026-11-06

- 將 Windows 支援帶到較新的主線功能。
  - 涵蓋強制刪除、程序時鐘、路徑、omp 連結、Visualize PR 與 `git-wt`。
  - 移除不再需要的分段程序啟動，以及針對舊介面且未在 macOS 執行過的 E2E 測試。

## 2026-11-08

- 調整 Windows 程序的預設工作目錄。
  - 未指定工作目錄的程序從 `%SystemRoot%` 啟動。
  - 避免常駐程序占用 plugin 目錄而使強制更新失敗。
- 解析 Windows junction 後比較 omp 連結目標。
  - 避免每次載入都重複執行 `omp plugin link`。
- 區分 Windows 移植版與 upstream 正式版本。
  - 將版本設為 `2.4.1-windows.1`，並在 README 指向此 fork 的安裝來源。
  - 避免將 Windows 移植版冒充 upstream 正式版本。
- 調整 Windows 的 Projects 快捷鍵。
  - 改為 `Ctrl+Alt+Shift+L`。
  - 避開 Windows 鍵對應差異及 Tern 既有的子母畫面快捷鍵。
- 使用 Tern 檔案檢視開啟 Windows 操作紀錄。
  - Git for Windows 的 `less` 不一定在 PATH。
- 修正 Windows 表單的全選與複製操作。
  - 使用 `Ctrl+A` 和 `Ctrl+C` 全選及複製。
  - 原本只辨識 Cmd，導致操作無法使用。
- 調整 Windows worktree 刪除流程。
  - 刪除前先關閉其 session，並調整確認與錯誤提示。
  - 避免程序占用工作目錄導致留下空資料夾。
- 將缺少 `.git` 的 checkout 視為已移除。
  - 外部刪除後，即使空資料夾仍存在，worktree 也會顯示為已消失，並能關閉 session。
- 以 UTF-8 解碼測試中的 sub-worktree 腳本輸出。
  - 避免繁體中文 Windows 的 cp950 預設編碼造成錯誤。
- 避免 PR relay 附上空引文。
  - agent 尚無最後一則訊息時，不再附上引文。
  - 避免完成事件早於 `last_message` 更新而傳出空字串。
- 在家族 worktree 刪除完成通知中列出保留的子 worktree。
  - 避免先前的保留通知被後續通知立即取代。
- 隱藏 Projects 時恢復原窗格焦點。
  - 原窗格仍屬同一分頁時，將焦點還給它。
  - 避免 Tern 選到其他窗格。
- 依儲存寬度直接在目標位置開啟 Projects。
  - 避免先開啟再移動和調整大小造成鄰近終端多次重繪。
- 讓快捷鍵命令直接切換 Projects。
  - 不再排入零毫秒計時器。
  - 避免單次按鍵後必須等待下一個事件才切換。

## 2026-11-10

- 移除狀態列的 Projects 按鈕。
  - 保留快捷鍵與命令入口。
  - 提交訊息和 diff 未說明移除原因。
