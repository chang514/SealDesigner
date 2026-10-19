# 篆刻印面設計器｜Codex 接交記錄

最後整理日期：2026-10-19（Asia/Taipei）

這份文件供重新安裝 Codex 或更換工作階段後，接續開發使用。開始工作前請先讀完本檔案，再檢查本機檔案與 GitHub 最新狀態。

## 專案位置

目前實際工作資料夾位於 `D:\chang\changCodex`，不是目前工作區名稱中的 `D:\chang\chang_Codex`。

### Windows 桌面版

- 專案：`D:\chang\changCodex\SealDesigner`
- 專案檔：`D:\chang\changCodex\SealDesigner\SealDesigner.csproj`
- 主要介面：`D:\chang\changCodex\SealDesigner\MainWindow.xaml`
- 主要程式：`D:\chang\changCodex\SealDesigner\MainWindow.xaml.cs`
- 啟動檔：`D:\chang\changCodex\啟動篆刻印面設計器.bat`
- Release 輸出：`D:\chang\changCodex\SealDesigner\bin\Release\net8.0-windows\`

### 網頁版

- 專案：`D:\chang\changCodex\SealDesignerWeb`
- 入口：`D:\chang\changCodex\SealDesignerWeb\index.html`
- 程式：`D:\chang\changCodex\SealDesignerWeb\app.js`
- 樣式：`D:\chang\changCodex\SealDesignerWeb\style.css`
- 字型路徑轉換函式庫：`D:\chang\changCodex\SealDesignerWeb\opentype.min.js`

### GitHub 文件用本地副本

- Git repository：`D:\chang\changCodex\GitHub-SealDesigner`
- 遠端：`https://github.com/chang514/SealDesigner.git`

## 已完成的桌面版功能

- WPF／.NET 8 Windows 桌面程式
- 方形／圓形印面
- 10、15、20、25 mm 與自訂尺寸
- 1～15 個中文字
- Windows 已安裝字型清單，顯示中文名稱
- 預設優先使用標楷體／DFKai 可用版本
- 陰刻／陽刻
- 一般／粗體
- 最外圍直角邊框，預設最細且滿版
- 傳統右至左、欄內上至下排列
- 三字：右欄姓氏，左欄名字上下排列，姓與名字欄維持比例
- 4～15 字欄位配置：
  - 4：右 2、左 2
  - 5：右 3、左 2
  - 6：右 3、左 3
  - 7：右 3、中 2、左 2
  - 8：右 3、中 3、左 2
  - 9：右 3、中 3、左 3
  - 10：右 4、中 3、左 3
  - 11：右 4、中 4、左 3
  - 12：右 4、中 4、左 4
  - 13：右 5、中 4、左 4
  - 14：右 5、中 5、左 4
  - 15：右 5、中 5、左 5
- 共用基準字級，避免多字中某些字特別大
- 單一字拖曳移動
- 移動對象可選：單一字、左欄、中欄、右欄、全部文字
- 比例對象可選：單一字、左欄、中欄、右欄、全部
- 整體大小、左右寬度、上下高度比例調整
- 比例數字顯示與輸入
- 中央預覽縮放滑桿
- 左／中央預覽／右側面板 GridSplitter
- PNG、JPG、SVG 輸出
- SVG 輸出為路徑
- 已移除筆畫修整功能
- 曾修正中文輸入第 9 字亂碼、啟動錯誤、PNG 裁切、粗體多餘線條等問題

## 桌面版建置與測試

PowerShell：

```powershell
Set-Location 'D:\chang\changCodex\SealDesigner'
$env:Path = 'C:\Program Files\dotnet;' + $env:Path
dotnet build .\SealDesigner.csproj --configuration Release
```

啟動：

```powershell
cmd.exe /c 'D:\chang\changCodex\啟動篆刻印面設計器.bat'
```

目前已驗證 Release 建置為 0 警告、0 錯誤，並能正常啟動。

自動輸出測試（功能已加入程式）：

```powershell
& 'D:\chang\changCodex\SealDesigner\bin\Release\net8.0-windows\SealDesigner.exe' --self-test 'D:\chang\changCodex\SealDesigner\self-test.png' '國立玉井高級工商職業學校' Bold
```

輸出後應使用 Codex 的圖片檢視工具查看 PNG，而不是只看編譯結果。歷史測試圖在 `D:\chang\changCodex\SealDesigner\`，例如 `十五字測試.png`、`欄位拖曳測試.png`、`介面確認_移動對象.png`。

## 免安裝版發布

發布命令：

```powershell
Set-Location 'D:\chang\changCodex\SealDesigner'
$env:Path = 'C:\Program Files\dotnet;' + $env:Path
dotnet publish .\SealDesigner.csproj --configuration Release --runtime win-x64 --self-contained true -p:PublishSingleFile=true -p:IncludeNativeLibrariesForSelfExtract=true -p:EnableCompressionInSingleFile=true --output 'D:\chang\changCodex\SealDesigner_Portable_New'
```

目前已發布的正式 ZIP：

- 本機原檔：`D:\chang\changCodex\篆刻印面設計器_免安裝版_win-x64_20261004.zip`
- Release 使用名稱：`SealDesigner_Portable_win-x64_20261004.zip`
- 內容是 Windows x64 self-contained single-file，約 66 MB
- 適用 Windows 10／11 64 位元

## 網頁版目前狀態

網頁版是純 HTML／CSS／JavaScript，無後端，可直接開啟 `index.html`。

已完成：

- 1～15 字
- 方形／圓形、尺寸與邊框
- 陰刻／陽刻、一般／粗體
- 傳統欄位排列
- 單字、左／中／右欄、全部文字移動
- 單字／欄位比例調整
- 預覽縮放
- PNG／JPG／SVG 介面
- 以 opentype.js 將上傳 TTF／OTF 字型轉為 SVG path

已修正：

- 預覽縮放時 canvas 被清空的問題：`resize()` 現在會重新 `draw()`
- 左右寬度／上下高度只改裁切框、不改文字的問題：現在使用 Canvas transform 真正縮放文字，SVG path 也同步套用 transform
- 初始字體過小、留白過多的問題：提高自動 fitting 字級

已知限制與下一步：

- 瀏覽器不能安全列出所有本機字型；網頁版提供常用字型選項與 TTF／OTF 上傳
- SVG 路徑輸出必須先上傳可由 opentype.js 解析的 TTF／OTF 字型，否則會提示並不輸出文字路徑
- `raster()` 目前使用預覽 canvas 輸出，JPG DPI 選項尚未真正改變輸出像素密度；若要正式使用，應建立獨立高解析度 canvas 後重繪
- 建議下一輪用 Chrome／Edge 實測滑桿、拖曳、陰刻／陽刻、PNG／JPG／SVG 匯出與多字排列，再更新網頁版 ZIP

目前網頁版 Release ZIP：

- 本機 v3：`D:\chang\changCodex\篆刻印面設計器_網頁版_20261004_v3.zip`
- GitHub Release 名稱：`SealDesigner_Web_20261004_v3.zip`

## GitHub 發布狀態

公開 repository：

`https://github.com/chang514/SealDesigner`

目前 Release：

`https://github.com/chang514/SealDesigner/releases/tag/v2026.10.04`

下載網址：

- `https://github.com/chang514/SealDesigner/releases/download/v2026.10.04/SealDesigner_Portable_win-x64_20261004.zip`
- `https://github.com/chang514/SealDesigner/releases/download/v2026.10.04/SealDesigner_Web_20261004_v3.zip`

GitHub repository 目前主要放 README、操作截圖與交接文件；完整桌面原始碼尚未放入 GitHub。若日後要公開原始碼，應先確認授權與是否要排除 `bin`、`obj`、測試輸出圖。

## GitHub CLI

免安裝 CLI 位於：

`D:\chang\changCodex\gh-cli-portable\bin\gh.exe`

檢查登入：

```powershell
$env:Path = 'D:\chang\changCodex\gh-cli-portable\bin;' + $env:Path
gh auth status
```

若重新安裝 Codex 或登入狀態遺失：

```powershell
$env:Path = 'D:\chang\changCodex\gh-cli-portable\bin;' + $env:Path
gh auth login --hostname github.com --git-protocol https --web
```

登入時由使用者自行輸入 GitHub 密碼與驗證碼，不要把密碼或 token 寫入此檔案。

## 重新接續工作的建議順序

1. 先讀本 `handoff.md`。
2. 檢查 `D:\chang\changCodex\SealDesigner` 與 `SealDesignerWeb` 是否仍存在。
3. 先建置桌面版並啟動，確認既有功能沒有被破壞。
4. 以 3 字、9 字、15 字與「國立玉井高級工商職業學校」測試，並用圖片查看結果。
5. 網頁版若有修改，使用 Chrome／Edge 實際開啟與截圖驗證。
6. 只有在功能與視覺確認後才重新產生 ZIP。
7. 更新 GitHub README、Release asset 與版本號；不要覆蓋舊 Release，除非使用者明確要求。

## 注意

- 不要使用 `git reset --hard` 或刪除整個專案資料夾。
- 不要把 Windows 商業字型檔直接打包進發布版本。
- 免安裝版會使用目標電腦已安裝的字型。
- 若新增免費字型，必須一併保留其授權文件與來源聲明。
