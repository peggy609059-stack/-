# 智慧場域實境模擬參訪系統

利用 360 度環景拍攝或數位標註工具，設計一套不需同仁全程帶看的網頁版導覽介面。

## 目錄

- [功能特色](#功能特色)
- [使用技術](#使用技術)
- [系統架構](#系統架構)
- [專案檔案](#專案檔案)
- [開始使用](#開始使用)
- [部署至 GitHub Pages](#部署至-github-pages)



本專案以蘆洲集賢社區長照機構為場域，將不同空間串接成可自行探索的線上參訪體驗。使用者可透過全景影像了解場域配置、服務空間與友善設施。

## 功能特色

- **360° 全景參訪**：以拖曳或觸控方式環視各空間。
- **場景熱點導覽**：點選影像中的導覽熱點，在櫃檯、鞋櫃、走廊、日照生活室、教室、活動區及廁所等場景間移動。
- **空間文字與語音介紹**：查看場景說明，並播放對應語音。
- **平面圖檢視**：從教室區開啟廚房及逃生區、洗手台的平面圖片與說明。
- **地圖與快速選單**：透過迷你地圖、放大地圖或場景選單查看位置及快速前往空間。
- **操作教學**：提供逐步教學，可重新開始或略過。
- **返回與操作提示**：支援返回上一個場景，並提供初次環視提示。

## 使用技術

- HTML、CSS、JavaScript
- [Pannellum 2.5.6](https://pannellum.org/) 全景檢視器，透過 jsDelivr CDN 載入

## 系統架構

本系統採用靜態網頁架構，由瀏覽器直接載入 HTML、場域影像與音訊資源；目前版本未設定後端 API 或資料庫。Pannellum 負責呈現 360° 全景，JavaScript 管理場景切換、互動熱點、地圖標記、空間介紹、語音播放及操作教學。

```mermaid
flowchart TD
    Visitor[參訪者]
    subgraph Client[使用者端：網頁瀏覽器]
        UI[HTML 與 CSS 介面]
        Logic[JavaScript 導覽邏輯]
        Pannellum[Pannellum 全景檢視器]
        Visitor --> UI
        UI --> Logic
        Logic --> Pannellum
        Logic --> Content[場景資料與導覽設定<br/>sceneInfo、sceneCoordinates、tutorialSteps]
    end
    Pannellum --> Panorama[360° 全景影像]
    UI --> Floorplan[平面圖片]
    Logic --> Audio[場景介紹與教學語音]
    UI -. CDN 載入 .-> CDN[jsDelivr：Pannellum CSS / JavaScript]
    Hosting[靜態網站主機<br/>本機伺服器或 GitHub Pages] --> UI
    Hosting --> Panorama
    Hosting --> Floorplan
    Hosting --> Audio
```

**資料流程**：瀏覽器從靜態網站主機取得網頁與媒體檔；參訪者操作介面後，JavaScript 依場景設定呼叫 Pannellum 切換全景，並更新地圖位置及空間介紹。需要語音時，瀏覽器播放對應的音檔。Pannellum 程式庫則由 jsDelivr CDN 提供。
## 專案檔案

目前版本為單一 HTML 頁面，並以相對路徑載入場景影像、平面圖及語音檔。請將下列資源放在 HTML 檔同一層（或同步修改程式中的路徑）：

- **全景影像**：場景設定中 `panorama` 指定的 `.jpg` 檔，例如 `櫃台.jpg`、`上課區.jpg`、`走廊.jpg`、`平安家.jpg`。
- **平面圖片**：`廚房.jpg`、`逃生.jpg`。
- **場景語音**：`sceneAudioFiles` 中指定的 `.mp3` 檔，例如 `櫃台.mp3`、`教室.mp3`。
- **教學語音**：`tutorialAudioFiles` 中指定的 `.mp3` 檔，包括 `1.mp3` 至 `9.mp3`、`tutorial_step10.mp3`、`tutorial_step11.mp3`。

請依 HTML 中的場景與音檔設定備齊對應檔案；檔名需與設定完全一致。若未提供音檔，相關語音播放功能將無法播放；缺少全景影像時，對應場景也無法正常顯示。

## 開始使用

1. 將 HTML 檔及所需影像、音訊檔放在同一個資料夾。
2. 使用瀏覽器開啟 HTML 檔即可參訪。若瀏覽器限制本機檔案載入，請改用本機 HTTP 靜態伺服器，例如 VS Code 的 Live Server。
3. 首次進入時點擊畫面提示以啟用音訊；可跟隨教學操作，或關閉教學後自行探索。

> Pannellum 由 CDN 載入，因此首次載入頁面需要網路連線。

## 部署至 GitHub Pages

1. 將 HTML 檔、場景全景圖、平面圖片與音檔一併提交至 GitHub 儲存庫。
2. 若入口檔名稱不是 `index.html`，建議將其改名為 `index.html`，方便以專案網址直接開啟。
3. 在儲存庫的 **Settings → Pages**，選擇部署來源分支及目錄（例如 `main` 與 `/ (root)`），並儲存設定。
4. 等待 GitHub Pages 完成部署後，使用 Pages 提供的網址開啟導覽頁。

請確認場景資產已納入儲存庫，且檔名大小寫、中文字及相對路徑完全相符。GitHub Pages 網址大小寫有別於本機 Windows 檔案系統。

## 調整場域內容

- **新增或修改場景**：編輯 `viewer` 設定中的 `scenes`，設定場景標題、全景圖檔及 `hotSpots` 導覽位置。
- **修改介紹文字**：編輯 `sceneInfo` 中的標題、描述及 `sceneAudioFiles` 對應音檔。
- **調整地圖位置**：修改 `sceneCoordinates` 中各場景的地圖座標百分比。
- **更新場域地圖及平面圖**：更換對應圖片並確認程式中的檔名和路徑。
- **編輯操作教學**：修改 `tutorialSteps` 及 `tutorialAudioFiles`。

新增場景時，請一併確認其介紹資料、地圖座標、音檔（如有）及相鄰場景的導覽熱點設定。



