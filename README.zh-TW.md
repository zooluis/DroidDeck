<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="artwork/droiddeck-banner-dark.svg">
    <img alt="DroidDeck" src="artwork/droiddeck-banner-light.svg" width="100%">
  </picture>
</p>

[簡體中文](README.zh-CN.md) · [English](README.md) · **繁體中文**

DroidDeck 把 SteamOS 的體驗帶到 Android：Steam 遊戲模式、完整的 Linux 桌面、與 GOG、Epic Games、Xbox 及其他商店平台的整合、Steam VR，等等！

<p align="center"><b>100% 開源。無廣告。無遙測。不隱藏任何原始碼。</b></p>

<p align="center">
  <a href="https://github.com/Droid-Deck/DroidDeck/releases/download/0.6.9/DroidDeck-0.6.9.apk"><img src="https://img.shields.io/badge/Download-DroidDeck%200.6.9%20APK-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="下載 DroidDeck 0.6.9"></a>
  <a href="https://discord.gg/JRGAvawjsm"><img src="https://img.shields.io/badge/Discord-Join%20us-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="加入 DroidDeck Discord"></a>
</p>

<p align="center">
  <a href="https://www.star-history.com/droid-deck/droiddeck"><img src="https://api.star-history.com/badge?repo=Droid-Deck/DroidDeck&type=rank&theme=dark" alt="Star History 排名"></a>
</p>

<p align="center">
  <a href="https://trendshift.io/repositories/267852"><img src="https://trendshift.io/api/badge/trendshift/repositories/267852/daily?language=C" alt="Droid-Deck%2FDroidDeck | Trendshift" width="250" height="55"></a>
  <a href="https://trendshift.io/repositories/267852"><img src="https://trendshift.io/api/badge/trendshift/repositories/267852/daily?language=Kotlin" alt="Droid-Deck%2FDroidDeck | Trendshift" width="250" height="55"></a>
</p>

<p align="center"><img src="docs/media/steam-to-desktop.gif" width="100%" alt="在 Retroid Pocket Duo 上：從 DroidDeck 啟動器遊玩 Steam，在 Steam 大螢幕模式中開啟 Pac-Man 並遊玩，結束遊戲，然後從電源選單切換到桌面"></p>

> DroidDeck 沒有官網。請只從本儲存庫的 [Releases](https://github.com/Droid-Deck/DroidDeck/releases) 下載。

<p align="center">
  <img src="docs/releases/media/0.6.7/library.gif" width="49%" alt="遊戲庫切換來源標籤、變更卡片尺寸並開啟篩選選單">
  <img src="docs/releases/media/0.3.0/in-game.gif" width="49%" alt="一款透過 Proton 執行的 Windows 遊戲，正在使用螢幕控制">
  <img src="docs/releases/media/0.4.0/multi-controller.gif" width="49%" alt="兩支手把在同一臺掌機上分割畫面遊玩">
  <img src="docs/releases/media/0.6.7/ubuntu-desktop-discover.jpg" width="49%" alt="Kubuntu 桌面，已開啟 Discover">
</p>

## 安裝

1. 需要 Android 9 或更新版本，顯示晶片為 Adreno 730 或更新，或任意 8xx。Adreno 6xx 為實驗性支援。不支援 Mali、Xclipse、PowerVR 與 Adreno 710。不需要 root。
2. 執行環境約需 3 GB 可用空間，桌面與模擬器另需 1.1 GB。
3. 安裝 [APK](https://github.com/Droid-Deck/DroidDeck/releases/download/0.6.9/DroidDeck-0.6.9.apk)，安裝 Linux 執行環境，按 **Play** 並登入 Steam。
4. 在 Android 14 及更新版本上，請在開發者選項中開啟 **停用子程序限制**（Disable child process restrictions）。在 Android 12 與 13 上，Steam 首次啟動會顯示 **Fix it for me** 按鈕，引導你完成設定。

智慧眼鏡的設定見[支援矩陣](docs/smart-glasses-support.md)。發行說明見 [docs/releases](docs/releases)。

協助、預覽版建置與裝置報告請前往 [Discord](https://discord.gg/JRGAvawjsm)。回報 Bug 請在 **#bug-reports** 發文，並附上透過 **Share logs** 匯出的 zip。

## 建置

需要 Docker、Java 17、Android SDK 與 NDK、透過 rustup 安裝的 Rust、GitHub CLI、Python 3 以及 `zstd`。

```bash
tools/build_local.sh
```

APK 輸出在 `app/build/outputs/apk/release/`。使用 `tools/deploy_local.sh` 安裝。首次執行之後，`./gradlew :app:assembleRelease` 會重用已準備好的輸入。單元測試透過 `./gradlew -PskipRust=true :app:testDebugUnitTest` 執行，輔助程式與 preload 測試套件透過 `bash tools/test_local.sh` 執行。

在 Windows 上，參見 `tools/run_windows.ps1` 與 `tools/prepare_windows_runtime.py`。

## 限制

相容性與效能因裝置而異。桌面合成採用軟體渲染，Firefox 沙箱在 proot 下有所削弱。

## 致謝與授權

DroidDeck 採用 [GPL-3.0](LICENSE) 授權。Steam 與 Proton 歸 Valve Corporation 所有。VITURE、RayNeo、XREAL、Rokid、REDMAGIC、Nintendo Switch 與 Joy-Con 為各自所有者的商標。本專案與上述公司均無關聯。

<details>
<summary><b>執行環境、Wine 與 Proton</b></summary>

- 執行環境、shim、輸入與控制器相關工作，建立在 maxjivi05 的 WinNative 與 Bannerlator 之上。
- mmap 環形緩衝區控制器輸入由 ribbit384 實作（[WinNative #345](https://github.com/WinNative-Emu/WinNative/pull/345)）。
- [FEX](https://github.com/FEX-Emu/FEX)（MIT）負責執行 x86 程式碼。DroidDeck 的建置把平台名稱寫入 `AT_PLATFORM`，以便 VHSgunzo 的 [sharun](https://github.com/VHSgunzo/sharun) AppImage（MIT）能透過其 [userland-execve](https://crates.io/crates/userland-execve) 載入器（MIT）啟動。FEX 的程式碼磁碟快取已在其原始碼中核對。
- x86 AppImage，以及執行環境無法在 Android 上解包的 arm64 AppImage，均使用 VHSgunzo 的 [uruntime](https://github.com/VHSgunzo/uruntime)（MIT）解包，並原樣附帶其授權。
- 連結 OpenSSL 3 的 x86 程式使用 [Steam Linux Runtime 4.0](https://gitlab.steamos.cloud/steamrt/steamrt) 中的副本。x86 Linux 遊戲會取得由 [pressure-vessel](https://gitlab.steamos.cloud/steamrt/steam-runtime-tools) 寫入的 `/etc/asound.conf`。
- `WriteProcessMemory` 掛鉤會退回 `/proc/<pid>/mem`，與 Wine 11 之前 [Wine](https://gitlab.winehq.org/wine/wine) 的 `server/ptrace.c` 做法一致。
- Wine 的寫入監視使用 [Proton](https://github.com/ValveSoftware/wine) 的 `dlls/ntdll/unix/virtual.c`（LGPL-2.1-or-later）中的 userfaultfd 支援。libblsession 提供 arm64 系統呼叫編號，並從 `/proc/self/pagemap` 回應 `PAGEMAP_SCAN`。
- libblsession 在依 [Linux](https://github.com/torvalds/linux) `arch/arm64/kernel/signal.c` 配置的堆疊框架上原位執行 Wine 的 arm64 `SIGUSR2` 上下文處理常式（`dlls/ntdll/unix/signal_arm64.c`，LGPL-2.1-or-later），並向 `dlls/ntdll/unix/process.c` 中的 `fill_vm_counters` 提供核心自己的 `/proc/<pid>/status`。
- droiddeck-ntsync 遵循 [Linux](https://github.com/torvalds/linux) 在 `Documentation/userspace-api/ntsync.rst` 中定義的 ntsync 語意（GPL-2.0）。
- proot 快速路徑依 [PRoot](https://github.com/proot-me/proot) 的 `path/binding.c`（GPL-2.0-or-later）方式排列綁定。
- DiRT 3 與 DiRT Showdown 依 RoccoC 的 DiRTTelemetryErrorFix（MIT）保留其遙測 Socket，該修正基於 Carsten Gottschling 的方案。
- 遊戲使用 [Mesa](https://gitlab.freedesktop.org/mesa/mesa) Turnip 的逐算圖通道記憶體選擇。Steam 客戶端維持 `TU_DEBUG=sysmem`。
- Black Ops III 與 Dark Souls Remastered 在沒有 [umu-protonfixes](https://github.com/Open-Wine-Components/umu-protonfixes)（BSD-2-Clause）的情況下啟動，其 `311210.py` 與 `570940.py` 在 ARM64 Proton 下會失敗。
- [ReSkate](https://github.com/Dingo-Shenanigans/ReSkate)（GPL-3.0，由 Dingo Shenanigans 開發）使用其 README 中的啟動選項與 D3D12 特性層級 12_1。
- arm64 客戶端的登入，是透過以 [deadlock-api-ingest](https://github.com/deadlock-api/deadlock-api-ingest) 的 `ConnectCache` 格式讀取 Steam 已儲存的權杖來核對的。

</details>

<details>
<summary><b>商店</b></summary>

- Epic Games 與 GOG 建立在 [Phobos](https://github.com/phobos665) 對 [Legendary](https://github.com/legendary-gl/legendary) 和 [gogdl](https://github.com/Heroic-Games-Launcher/heroic-gogdl) 的改編之上。GOG 安裝腳本的登錄檔項目，按 [Heroic Games Launcher](https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher)（GPL-3.0）的方式讀取。
- Epic 官方 Linux 啟動器預覽透過 FEX 執行。該配置不在 Epic 列出的 Linux 支援範圍內。
- Battle.net 由 [@kayakyakr](https://github.com/kayakyakr) 開發。它在 Proton 中執行暴雪自己的客戶端（從暴雪下載，未隨附），並顯示來自 [Battle.net 商店](https://shop.battle.net)的主題圖。沒有 Steam 時，其遊戲會透過 [Proton](https://github.com/ValveSoftware/wine) `dlls/winebus.sys` 中的 `PROTON_SPOOF_STEAMINPUT_VIDPID`，把手把識別為 Xbox 360 控制器。
- Battle.net 安裝按 [@barncastle](https://github.com/barncastle) 的 [Battle.Net-Installer](https://github.com/barncastle/Battle.Net-Installer) 方式驅動暴雪的 Agent，請求細節來自 [wowdev.wiki](https://wowdev.wiki/Agent)。僅閱覽、未取用程式碼的有：[@mubaraknumann](https://github.com/mubaraknumann) 的 [unifideck](https://github.com/mubaraknumann/unifideck)、[@djmittens](https://github.com/djmittens) 的 [neo-mittens](https://github.com/djmittens/neo-mittens) `bnetswitch`、[@BCD1210](https://github.com/BCD1210) 的 [soju](https://github.com/BCD1210/soju) 以及 [Lutris](https://github.com/lutris/lutris) 的 `product.db` 讀取器。
- 《魔獸世界》外掛匯入會從暫存副本取代外掛資料夾，做法與 [WowUp](https://github.com/WowUp/WowUp) 相同；版本目錄讀取方式與 [WoWDeck](https://github.com/sigboe/wowdeck) 一致。兩者都未取用程式碼。
- 《魔獸世界》外掛下載視窗透過其 MMOUI API 搜尋 [WoWInterface](https://www.wowinterface.com)。[CurseForge](https://www.curseforge.com/wow) 在使用者瀏覽器中開啟，不使用 API 與金鑰。
- Xbox 遊戲載入 [@domsvoboda](https://github.com/domsvoboda) 的 [xodus-nexus](https://github.com/domsvoboda/xodus-nexus) 模組（LGPL-2.1-or-later），包括 `wintypes.dll`。Minecraft 的 GameInput 配置已對照 [@Wyze3306](https://github.com/Wyze3306) 的 [BedrockOnLinux](https://github.com/Wyze3306/BedrockOnLinux)（MIT）核對。
- Xbox Cloud Gaming 播放器使用 [@unknownskl](https://github.com/unknownskl) 的 [Greenlight](https://github.com/unknownskl/greenlight)（MIT）的 WebRTC 客戶端，其 offer 調整與銳化功能遵循 [@redphx](https://github.com/redphx) 的 [Better xCloud](https://github.com/redphx/better-xcloud)（MIT）。參見 `tools/xcloud-player/NOTICE.md`。
- itch.io 的 butler 套件隨附 butler 的 `7z.so`，即 itch.io 基於 Igor Pavlov 的 [7-Zip](https://www.7-zip.org) 建置的版本（LGPL-2.1，RAR 程式碼帶 unRAR 限制），未作修改。其他壓縮檔使用 Apache Commons Compress 與執行環境的 bsdtar（libarchive）。
- 商店透過 DroidDeck 自有的 bubblewrap shim 使用 [Flatpak](https://github.com/flatpak/flatpak)（LGPL-2.1）。Chromium 與 Electron 應用程式直接啟動，因為 [@refi64](https://github.com/refi64) 的 [zypak](https://github.com/refi64/zypak)（BSD-3-Clause）始終使用 Flatpak 的 spawn portal。
- 資料夾同步執行 [Syncthing](https://github.com/syncthing/syncthing)（MPL-2.0），基於 [@researchxxl](https://github.com/researchxxl) 的 [syncthing-android](https://github.com/researchxxl/syncthing-android) 分支（MPL-2.0）建置。

</details>

<details>
<summary><b>音訊、影片與 HUD</b></summary>

- Steam 音訊經由 [@The412Banner](https://github.com/The412Banner) 的 [DirectAudio](https://github.com/The412Banner/directaudio) PulseAudio 匯（LGPL-2.1-or-later），由 `tools/directaudio/sink` 建置。
- DroidDeck HUD 為 [@The412Banner](https://github.com/The412Banner) 的 [FusionHUD](https://github.com/The412Banner/FusionHUD)（GPL-3.0，含署名條款）。其讀數與 Valve 的 [MangoHud](https://github.com/flightlessmango/MangoHud) 建置（MIT）一致，幀時間來自 gamescope 補丁 0125。
- HUD 的 CPU、GPU 負載、溫度與電池讀數來自 [@xXJSONDeruloXx](https://github.com/xXJSONDeruloXx) 的工作。
- LSFG 幀生成來自 Camille LaVey 與 [Eden](https://eden-emu.dev) 專案，遵循 [lsfg-vk](https://github.com/PancakeTAS/lsfg-vk)，由 [@maxjivi05](https://github.com/maxjivi05) 移植。它需要你自己擁有的 [Lossless Scaling](https://store.steampowered.com/app/993090/) 副本，且不隨附其任何著色器。
- 遠端遊玩透過 Android 的 MediaCodec 或 Steam 執行環境中的 [FFmpeg](https://ffmpeg.org)（LGPL-2.1-or-later）解碼。高通低延遲設定遵循 [Moonlight](https://github.com/moonlight-stream/moonlight-android)（GPL-3.0）。
- [PipeWire](https://gitlab.freedesktop.org/pipewire/pipewire)（MIT）Socket 使用其 `module-access.c` 中的設定，[WirePlumber](https://gitlab.freedesktop.org/pipewire/wireplumber)（MIT）讓這些客戶端維持完整權限。

</details>

<details>
<summary><b>桌面</b></summary>

- KDE 的 pacman 儲存庫快照自 [Arch Linux ARM](https://archlinuxarm.org)，並透過 [pacman](https://gitlab.archlinux.org/pacman/pacman)（GPL-2.0-or-later）安裝。
- [KWin](https://invent.kde.org/plasma/kwin)（GPL-2.0-or-later）經過修補，以傳遞其 `wayland_backend.cpp` 中仍標為 TODO 的捲動資料。
- 僅閱覽、未取用程式碼的有：[Qt](https://code.qt.io/cgit/qt/qtbase.git) 的 `qwaylandinputdevice.cpp` 與 `qguiapplication.cpp`、[Konsole](https://invent.kde.org/utilities/konsole) 的 `TerminalDisplay.cpp`，以及 [Mesa](https://gitlab.freedesktop.org/mesa/mesa) 的 `u_threaded_context.c` 與 `zink_resource.c`。
- Steam 鍵盤的視窗規則遵循 Valve 的 `steamdeck-kde-presets`，並參考 [Bazzite](https://github.com/ublue-os/bazzite) 的 `0001-steam-virtual-keyboard.patch`。
- Steam 的瀏覽按鈕透過 PyGObject（LGPL-2.1-or-later）開啟 GTK 3 檔案選擇器，並由 [gamescope](https://github.com/ValveSoftware/gamescope)（BSD-2-Clause）經 `GetAppIdFromReaper` 顯示在 Steam 之上。
- AYN Odin 2 的幻影第二螢幕，依據 [@SapphireRhodonite](https://github.com/SapphireRhodonite) 的 [WatermelonDS 0.0.5](https://github.com/SapphireRhodonite/WatermelonDS/releases/tag/0.0.5) 說明與 [iiSU #363](https://github.com/iisu-network/iiSU/issues/363) 得到確認，未取用其程式碼。

</details>

<details>
<summary><b>智慧眼鏡</b></summary>

- 建立在 WinNative 的 VITURE 工作之上。協定已對照 [@wheaney](https://github.com/wheaney) 的 [XRLinuxDriver](https://github.com/wheaney/XRLinuxDriver)、[@badicsalex](https://github.com/badicsalex) 的 [ar-drivers-rs](https://github.com/badicsalex/ar-drivers-rs) 與 [go-macos/iokit](https://pkg.go.dev/github.com/go-macos/iokit/viture) 核對。可選的 VITURE SDK 未包含在內。
- VITURE 協定 V2 幀結構改編自 Holger Bartnick 基於 [@elasticjava](https://github.com/elasticjava) 的 [viture-v2](https://github.com/elasticjava/viture-v2)（MIT）的工作。參見 `docs/third-party/viture-v2`。
- RayNeo 支援改編自 [@informalTechCode](https://github.com/informalTechCode) 的 [RayNeo-Air-Series-3DoF-Scaffold](https://github.com/informalTechCode/RayNeo-Air-Series-3DoF-Scaffold)（Apache-2.0），並參考 [@verncat](https://github.com/verncat) 的 [RayNeo-Air-3S-Pro-OpenVR](https://github.com/verncat/RayNeo-Air-3S-Pro-OpenVR)（MIT）與 [Monado](https://gitlab.freedesktop.org/monado/monado)（BSL-1.0）。參見 [docs/third-party/rayneo](docs/third-party/rayneo/README.md)。
- XREAL 1S 參考 XRLinuxDriver 與 @wheaney 的 [xrealOneDeviceKit](https://github.com/wheaney/xrealOneDeviceKit)、[@rohitsangwan01](https://github.com/rohitsangwan01) 的 [xreal_one_driver](https://github.com/rohitsangwan01/xreal_one_driver)（MIT）以及 [@0xcaff](https://github.com/0xcaff) 的 [xr-tools](https://github.com/0xcaff/xr-tools)。
- Rokid 遵循 ar-drivers-rs、@badicsalex 的 [Void Computing 文章](https://voidcomputing.hu/blog/worse-better-prettier/) 與 Monado 的 Rokid 驅動程式。
- REDMAGIC 顯示相關結論參考了 [@mekhontsev](https://github.com/mekhontsev) 的 [MagicDesk 調查](https://github.com/mekhontsev/magicdesk/blob/main/docs/xr-resolution-fix.md) 與 [@Coding-BR](https://github.com/Coding-BR) 的[反編譯 Nubia 核心](https://github.com/Coding-BR/android_kernel_nubia_sm8850_qwjujube)。

</details>

<details>
<summary><b>VR、Joy-Con 與手部追蹤</b></summary>

- Joy-Con VR 移植了 Google [GVR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk)（Apache-2.0）的手臂模型，並改編了 Valve [OpenVR](https://github.com/ValveSoftware/openvr) `handskeletonsimulation` 範例（BSD-3-Clause）的手部骨骼，該範例遵循 Moshi Turner 在 Monado 中的工作。Index 偏移來自 Monado 的 `vive_poses.c`（BSL-1.0）。
- Joy-Con 研究：Daniel J. Ogorchock 的 Linux [hid-nintendo](https://github.com/torvalds/linux/blob/master/drivers/hid/hid-nintendo.c)、[@dekuNukem](https://github.com/dekuNukem) 的 [Nintendo_Switch_Reverse_Engineering](https://github.com/dekuNukem/Nintendo_Switch_Reverse_Engineering)、[SDL](https://github.com/libsdl-org/SDL) 與 AOSP。已對照 [ALVR](https://github.com/alvr-org/ALVR)、[OpenComposite](https://gitlab.com/znixian/OpenOVR)、[PSMoveSteamVRBridge](https://github.com/HipsterSloth/PSMoveSteamVRBridge)、[@F8LEFT](https://github.com/F8LEFT) 的 [JoyConVr](https://github.com/F8LEFT/JoyConVr)、[@krnelpan1c](https://github.com/krnelpan1c) 的 [Daydream-Controller-SteamVR](https://github.com/krnelpan1c/Daydream-Controller-SteamVR)、[@yoyobuae](https://github.com/yoyobuae) 的 [joyconvr](https://github.com/yoyobuae/joyconvr)、[Moonlight](https://github.com/moonlight-stream/moonlight-android) 與 [Dolphin](https://github.com/dolphin-emu/dolphin) 核對。參見 [docs/third-party/joycon](docs/third-party/joycon/README.md)。
- 攝影機手部追蹤在 [CameraX](https://developer.android.com/jetpack/androidx/releases/camera) 幀（Apache-2.0）上執行 Google [MediaPipe](https://github.com/google-ai-edge/mediapipe) 的 Pose Landmarker (Lite)、BlazePose GHUM 3D 與 Hand Landmarker（Apache-2.0），並已關閉使用情形回報。已對照 JoyConVr 的相機應用程式（MIT）核對。
- 追蹤數學：GVR 手臂模型的加速度計模式、Eric Foxlin 的零速更新、Frosio、Pedersini 與 Borghese 的加速度計自動校準、卡爾曼濾波（Kalman，1960）、Horn 的四元數方法（1987）以及 1€ 濾波（Casiez、Roussel 與 Vogel，2012）。
- 手勢遵循 [ALVR](https://github.com/alvr-org/ALVR) 的 `hand_gestures.rs`（MIT）與 Meta Quest 的選單捏合，並在 [HaGRID](https://github.com/hukenovs/hagrid) 照片（CC BY-SA 4.0，未隨附）上調校。
- 手套模型遵循 OpenVR 的渲染模型參考，使用表面網格（Gibson，1998；Mikola Lysenko）生成網格，並採用 Inigo Quilez 的平滑最小值、距離場環境光遮蔽與多邊形距離（MIT）。參見 [docs/third-party/handtracking](docs/third-party/handtracking/README.md)。

</details>

## Star 歷史

<a href="https://www.star-history.com/#Droid-Deck/DroidDeck&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=Droid-Deck/DroidDeck&type=Date&theme=dark">
    <img alt="Star 歷史圖表" src="https://api.star-history.com/svg?repos=Droid-Deck/DroidDeck&type=Date" width="100%">
  </picture>
</a>
