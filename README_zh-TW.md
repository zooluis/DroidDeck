<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="artwork/droiddeck-banner-dark.svg">
    <img alt="DroidDeck" src="artwork/droiddeck-banner-light.svg" width="100%">
  </picture>
</p>

DroidDeck 將 SteamOS 體驗帶到 Android：在你的 Adreno 掌機上執行 Valve 的 Steam 用戶端（Big Picture 模式），並透過 Valve 的 ARM64 Proton 執行 Windows 遊戲。

<p align="center"><a href="https://discord.gg/JRGAvawjsm"><img src="https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white" alt="Join the DroidDeck Discord"></a></p>

<p align="center"><img src="docs/releases/media/0.2.0/launch-into-steam.gif" width="80%" alt="Tapping DroidDeck on the Android home screen and landing in Steam Big Picture"></p>

> 注意：DroidDeck 沒有獨立的網站。不要點擊任何聲稱是 DroidDeck 團隊的網站上的下載連結。

## 要求與安裝

在受支援的 Adreno 裝置（730 或更新，或 8xx）上使用 Android 9 或更新版本。Adreno 6xx 是實驗性的：DirectX 11 使用 DXVK 2 且可能執行，而 DirectX 12 遊戲仍可能當機。不支援 Mali、Xclipse、PowerVR 和 Adreno 710。無需 root。為執行時預留約 3 GB，為桌面和模擬器再預留 1.1 GB。從 [Releases](https://github.com/Droid-Deck/DroidDeck/releases) 安裝 APK，安裝 Linux 執行時，然後按 **Play** 並登入。Steam 會在首次啟動時下載。安裝 **Desktop & apps** 以使用桌面和模擬器。**Store** 使用 Flatpak 從 Flathub（ARM64 建置）安裝 Linux 應用程式和遊戲；還有安裝 Appimage 和設定指令碼的附加選項。**Stores**（設定 › Stores › “Show Stores in the rail”）登入 GOG、Epic Games 和 Amazon Games，並把遊戲下載到 Games 儲存。Epic 保留其直接 API 安裝路徑。開啟 Epic Stores 會準備 Epic 的官方 Linux 啟動器預覽。安裝後，從 DroidDeck 啟動的 Epic 遊戲使用該用戶端，用戶端中安裝的遊戲會匯入到 DroidDeck 的函式庫和捷徑中。x86-64 用戶端透過 FEX 在 ARM 上執行，DroidDeck 保留其 Proton 鉤子；此預覽和 ARM 設定在 Epic 列出的 Linux 支援範圍之外。商店晶片列上的齒輪圖示可設定其開啟索引標籤和下載速度等級。

在 Android 14 及更新版本上，在 Steam 啟動前，你必須開啟開發者選項中的 **Disable child process restrictions**（停用子程序限制）。如果開發者設定中沒有此選項（Android 12 和 13 裝置），Steam 首次啟動時會顯示一個 “Fix it for me” 按鈕，可幫助自動化設定過程。

## 智慧型眼鏡

使用 **設定 → Smart glasses** 在 DroidDeck 選單、啟動器、桌面會話和遊戲中設定全應用程式範圍的虛擬螢幕設定。受支援的 VITURE、RayNeo 和 XREAL 連線提供 3DoF；Luma Ultra 增加可選的實驗性 6DoF 和立體輔助手部/Joy-Con 追蹤。型號支援和 SDK 要求不同：支援裝置、控制、設定和限制請見 [智慧型眼鏡支援矩陣](docs/smart-glasses-support.md)。

## 社群

加入 [DroidDeck Discord](https://discord.gg/JRGAvawjsm) 獲取協助、預覽版和裝置報告。Bug 報告放在其 **#bug-reports** 論壇；附上 **Share logs**（設定，或會話抽屜）匯出的 zip，以便日誌隨附。

## 建置

執行 `tools/build_local.sh`，需要 Docker、Java 17、Android SDK/NDK、透過 rustup 的 Rust、GitHub CLI、Python 3 和已安裝的 `zstd`。它準備與 CI 相同的原生函式庫、鎖定元件和音訊來源，然後檢查 APK 的資源、JNI 匯出和相依性。APK 位於 `app/build/outputs/apk/release/app-release.apk`。設定 `DROIDDECK_PA13_SOURCE_DIR` 指向現有的 PulseAudio 13.0 原始碼目錄可跳過下載。設定 `DROIDDECK_SIGNING_ENV` 指向簽章環境檔，可在發布建置之上安裝；相對 keystore 路徑相對於該檔解析。要在已連線的裝置上安裝，執行 `tools/deploy_local.sh`。

準備之後，普通的 `./gradlew :app:assembleRelease` 建置可重複使用已驗證的輸入。如果原生原始碼、元件鎖定或暫存檔發生變化，打包會停止並提示重新執行輔助程式。產生的音訊留在 `app/build/` 下；被追蹤的基礎包永不修改。`./gradlew -PskipRust=true :app:testDebugUnitTest` 無需原生準備。若要建置不含商店引擎的 APK，使用 `DROIDDECK_SKIP_RUST=1 tools/build_local.sh`；它也會省略快取的引擎函式庫。

對於準備好的 Windows 檢出，執行 `./tools/run_windows.ps1`（可選 `-Serial <adb-serial>`），或在 Android Studio 中選擇 **DroidDeck XR** 並在已連線手機上執行 **app** 模組。機器相關選項在忽略的 `local.properties` 中：`droiddeck.python` 選擇 Python 可執行檔，`droiddeck.skipRust=true` 省略可選商店下載引擎，`droiddeck.debugApplicationIdSuffix=.xr` 在簽章發布版旁邊安裝 **DroidDeck XR Dev**。開發應用程式有自己的執行時和 Steam 登入。本機 Java 選擇在 `.gradle/config.properties` 中。

`python tools/prepare_windows_runtime.py` 從鎖定的 CI APK、三個本地重建的 preload 函式庫和 `build/windows-setup/` 中校驗和鎖定的 Gamescope 封存還原此檢出的已稽核執行時輸入。先啟動 Docker Desktop 的 Linux 引擎：準備過程現在會從目前原始碼重建 DroidDeck 的 ARM64 VR 驅動和 Vulkan 層，並執行其原生迴歸測試。編譯器映像檔在首次使用時自動建置。Vulkan 標頭來自 NDK（`--llvm-strip` 旁邊），或來自明確的 `--vk-include <NDK-sysroot-include>` 目錄。

要重複使用該建置，傳入 `--rebuilt-vr build/windows-setup/vr`。準備過程會將其 `vr-build.json` 與目前 Git 修訂版、來源檔案、Vulkan 標頭、輸出校驗和和 ARM64 架構進行校驗；缺失或過期的建置會失敗，而不是回退到捐贈者的 VR 函式庫。指令碼檢查實際捐贈者 CI 合併樹，仍拒絕無關的未稽核執行時變更，並在 `app/build/windows-runtime-provenance.json` 中記錄元件來源。本地重建時保留這些被忽略的準備檔；`clean` 會移除準備好的資源和其驗證清單。

用 `bash tools/test_local.sh` 執行輔助程式和 preload 迴歸套件。在 macOS 上，它透過 Docker 在 Linux 中執行，與輔助程式的執行時和 CI 一致，而不是針對 macOS 標頭編譯 Linux preloads。

## 限制

相容性和效能因裝置而異；硬體驗證有限。桌面合成使用軟體渲染。Firefox 沙箱在 proot 下被削弱。日誌保持應用程式私有（`files/logs/`，最近 30 個會話）；**Share logs** 打包最新會話，連同商店和工具日誌，放入一個清理過的 zip。

## 鳴謝與授權條款

GPL-3.0。執行時、墊片、輸入和控制器工作建立在 WinNative 和 Bannerlator（maxjivi05）之上。Epic Games 和 GOG 支援分別建立在 [Phobos](https://github.com/phobos665) 對 [Legendary](https://github.com/legendary-gl/legendary) 和 [gogdl](https://github.com/Heroic-Games-Launcher/heroic-gogdl) 的改編之上。LSFG 幀生成來自 Camille LaVey 和 [Eden](https://eden-emu.dev) 模擬器專案的工作，遵循 [lsfg-vk](https://github.com/PancakeTAS/lsfg-vk)，由 [@maxjivi05](https://github.com/maxjivi05) 移植到 WinNative 和 DroidDeck；它需要你自己擁有的 [Lossless Scaling](https://store.steampowered.com/app/993090/) 副本，且不附帶任何其著色器。x86 AppImage 以及任何其自身執行時無法解包的應用，都由 VHSgunzo 的 [uruntime](https://github.com/VHSgunzo/uruntime)（MIT）解包，原樣附帶其授權條款。Steam 用戶端的音訊透過 [@The412Banner](https://github.com/The412Banner) 的 [DirectAudio](https://github.com/The412Banner/directaudio) PulseAudio 匯（LGPL-2.1-or-later）。mmap 環形緩衝控制器輸入由 ribbit384 開發（[WinNative #345](https://github.com/WinNative-Emu/WinNative/pull/345)）。遊戲用 [Mesa](https://gitlab.freedesktop.org/mesa/mesa) Turnip 渲染。見 [LICENSE](LICENSE)。Steam 和 Proton 屬於 Valve Corporation；本專案與 Valve 無關聯。

XR 眼鏡支援建立在 WinNative 的 VITURE 眼鏡工作之上。VITURE 協定細節對照 [XRLinuxDriver](https://github.com/wheaney/XRLinuxDriver)（含其隨附的 VITURE 協定標頭）、[ar-drivers-rs](https://github.com/badicsalex/ar-drivers-rs) 和 [go-macos/iokit VITURE 包](https://pkg.go.dev/github.com/go-macos/iokit/viture)；可選的 VITURE SDK 屬於 VITURE，未包含。RayNeo 支援改編自 [@informalTechCode](https://github.com/informalTechCode) 的 [RayNeo-Air-Series-3DoF-Scaffold](https://github.com/informalTechCode/RayNeo-Air-Series-3DoF-Scaffold)（Apache-2.0）並參考 [@verncat](https://github.com/verncat) 的 [RayNeo-Air-3S-Pro-OpenVR](https://github.com/verncat/RayNeo-Air-3S-Pro-OpenVR)（MIT）和 [Monado](https://gitlab.freedesktop.org/monado/monado)（BSL-1.0）。XREAL 1S 支援參考 XRLinuxDriver 和 @wheaney 的 [xrealOneDeviceKit](https://github.com/wheaney/xrealOneDeviceKit)、[@rohitsangwan01](https://github.com/rohitsangwan01) 的 [xreal_one_driver](https://github.com/rohitsangwan01/xreal_one_driver)（MIT）和 [@0xcaff](https://github.com/0xcaff) 的 [xr-tools](https://github.com/0xcaff/xr-tools)。VITURE、RayNeo、XREAL 和 REDMAGIC 是其所有者的商標；本專案與它們無關聯。

Joy-Con VR 控制器移植了 Google 的 [Google VR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk)（Apache-2.0）手臂模型，改編了 Valve [OpenVR](https://github.com/ValveSoftware/openvr) `handskeletonsimulation` 範例（BSD-3-Clause）的手部骨骼，並使用 Monado `vive_poses.c` 的 Index 控制器偏移（BSL-1.0）。Joy-Con 行為參考了 Linux 的 [hid-nintendo](https://github.com/torvalds/linux/blob/master/drivers/hid/hid-nintendo.c) 驅動、[@dekuNukem](https://github.com/dekuNukem) 的 [Nintendo_Switch_Reverse_Engineering](https://github.com/dekuNukem/Nintendo_Switch_Reverse_Engineering)、[SDL](https://github.com/libsdl-org/SDL) 和 Android Open Source Project。Nintendo Switch 和 Joy-Con 是任天堂的商標；本專案與任天堂無關聯。

相機手部追蹤執行 Google 的 [MediaPipe](https://github.com/google-ai-edge/mediapipe) Tasks Vision 函式庫及其 Pose Landmarker (Lite) 模型、BlazePose GHUM 3D 和 Hand Landmarker 模型（均 Apache-2.0），處理來自 AndroidX [CameraX](https://developer.android.com/jetpack/androidx/releases/camera)（Apache-2.0）的幀；MediaPipe 的使用情況報告已關閉。手勢參考了 [@F8LEFT](https://github.com/F8LEFT) 的 [JoyConVr](https://github.com/F8LEFT/JoyConVr) 相機應用程式（MIT）、[ALVR](https://github.com/alvr-org/ALVR) 的 `hand_gestures.rs`（MIT）等。見 [docs/third-party/handtracking](docs/third-party/handtracking/README.md)。

Store 使用 [Flatpak](https://github.com/flatpak/flatpak)（LGPL-2.1）安裝和執行應用程式。其沙箱透過 DroidDeck 自己的 bubblewrap 墊片進行，遵循 Flatpak 將 bwrap 環境交給它的方式。
