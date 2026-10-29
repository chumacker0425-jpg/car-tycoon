# 造車大亨：我的車廠

以 HTML、JavaScript 和 Three.js 製作的單人網頁遊戲，包含車廠經營、車款研發、賽車與開放世界駕駛。

## 線上遊玩

https://chumacker0425-jpg.github.io/car-tycoon/

由 GitHub Pages 從 `main` 分支自動部署；首頁 `index.html` 會自動跳轉到遊戲。

## 執行

使用靜態網頁伺服器提供專案資料夾，開啟 `造車大亨_我的車廠.html`（需支援 WebGL 的瀏覽器）。
直接雙擊檔案開啟（file://）會因瀏覽器限制而無法載入車模。

## 檔案結構

- `造車大亨_我的車廠.html`：遊戲本體。
- `lib/`：Three.js r128、GLTFLoader 與 meshopt 解碼器，放在本地不依賴 CDN。
- `models/`：預設車模（`car0`–`car4` 為自家各級距，`riv0`–`riv4` 為競品），用 meshopt 壓縮並合併網格。賽車與自己駕駛時使用。
- `models/lod/`：同一批車模的低面數版本，工廠、停車場與世界車流使用。

車模由原始 Blender 匯出檔以 [glTF Transform](https://gltf-transform.dev/) 處理：

```
gltf-transform optimize in.glb out.glb --compress meshopt --palette false --instance false --flatten true --join true --join-named true --join-meshes true --simplify false
```

低面數版改用 `--simplify true --simplify-ratio 0.12 --simplify-error 0.01`。遊戲靠材質名稱 `BODYPAINT` 換車漆，所以不能開 `--palette`；Three.js r128 不支援 GPU instancing 擴充，所以關掉 `--instance`。

## 存檔

進度保存在瀏覽器的 localStorage，自訂模型保存在 IndexedDB。存檔不會跨裝置同步；網站網址或瀏覽器變更時，也不會自動移轉。

## GitHub 同步

每次更新完成後，先詢問使用者是否要同步到 GitHub。上傳程式碼不等於已發布可玩的網站。
