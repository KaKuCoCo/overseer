# Fork changelog

上游：[mmnavarr/overseer](https://github.com/mmnavarr/overseer)

## 2026-11-10

### Changed
- 檢查建立或刪除 worktree 的 helper 是否還活著時，改用 `cx.procs:find(pid)`，不再開 `powershell.exe Get-Process` 或 `/bin/kill -0`。
  - 原因：同一個 API 跨平台共用，也省下 PowerShell 冷啟動。`cx.procs:find` 用約 2 秒前的程序快照，剛啟動的 helper 查不到，所以要相隔 3 秒連續兩次查無才算已結束。
  - 上游：未回報。

### Removed
- 移除狀態列的 Projects 按鈕。
  - 原因：精簡狀態列；面板仍可用 `Ctrl+Alt+Shift+L` 或命令開關。

## 2026-11-08

### Added
- 新版本號 `2.4.1-windows.1`，README 指向 fork 的安裝方式。
  - 原因：與上游正式版本區分。

### Changed
- Windows 上未指定工作目錄的程序改從 `%SystemRoot%` 啟動。
  - 原因：程序占用 plugin 目錄會使強制更新失敗。
- Windows 的 Projects 快捷鍵改為 `Ctrl+Alt+Shift+L`。
  - 原因：`Cmd` 在 Windows 對應到 Win 鍵，且與 Tern 子母畫面快捷鍵衝突。
- Windows 改用 Tern 檔案檢視開啟操作紀錄。
  - 原因：Git for Windows 的 `less` 不一定在 PATH。
- Windows 刪除 worktree 前先關閉其 session。
  - 原因：程序占用工作目錄會留下空資料夾。
- 家族 worktree 刪除完成通知列出保留的子 worktree。
  - 原因：先前的保留通知會被後續通知立即蓋掉。
- 隱藏 Projects 時把焦點還給原窗格。
  - 原因：Tern 會選到其他窗格。
- Projects 依儲存寬度直接在目標位置開啟。
  - 原因：先開啟再移動與調整大小，造成鄰近終端多次重繪。
- 快捷鍵命令直接切換 Projects，不再排入計時器。
  - 原因：單次按鍵後要等下一個事件才會切換。

### Fixed
- 比較 omp 連結目標前先解析 Windows junction。
  - 原因：每次載入都重複執行 `omp plugin link`。
- Windows 表單支援 `Ctrl+A`、`Ctrl+C`。
  - 原因：原本只辨識 Cmd。
- 缺少 `.git` 的 checkout 視為已移除。
  - 原因：外部刪除後空資料夾仍在，worktree 無法關閉 session。
- 測試以 UTF-8 解碼 sub-worktree 腳本輸出。
  - 原因：繁體中文 Windows 預設 cp950 會解碼失敗。
- agent 尚無最後訊息時，PR relay 不附空引文。
  - 原因：完成事件可能早於 `last_message` 更新。

## 2026-11-06

### Changed
- Windows 支援跟上主線功能（強制刪除、程序時鐘、omp 連結、Visualize PR、`git-wt`）。
  - 原因：主線新增功能後，Windows 版需同步支援。
  - 上游：移植自上游 `windows-support` 分支。

## 2026-10-13

### Added
- 原生 Windows 專案與 worktree 支援。
  - 原因：原本僅支援 macOS。
  - 上游：移植自上游 `windows-support` 分支，未合併入 `main`。

## 已移除
