<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="artwork/droiddeck-banner-dark.svg">
    <img alt="DroidDeck" src="artwork/droiddeck-banner-light.svg" width="100%">
  </picture>
</p>

**简体中文** · [English](README.md) · [繁體中文](README.zh-TW.md)

DroidDeck 把 SteamOS 的体验带到 Android：Steam 游戏模式、完整的 Linux 桌面、与 GOG、Epic Games、Xbox 及其他商店平台的整合、Steam VR，等等！

<p align="center"><b>100% 开源。无广告。无遥测。不隐藏任何源码。</b></p>

<p align="center">
  <a href="https://github.com/Droid-Deck/DroidDeck/releases/download/0.6.9/DroidDeck-0.6.9.apk"><img src="https://img.shields.io/badge/Download-DroidDeck%200.6.9%20APK-3DDC84?style=for-the-badge&logo=android&logoColor=white" alt="下载 DroidDeck 0.6.9"></a>
  <a href="https://discord.gg/JRGAvawjsm"><img src="https://img.shields.io/badge/Discord-Join%20us-5865F2?style=for-the-badge&logo=discord&logoColor=white" alt="加入 DroidDeck Discord"></a>
</p>

<p align="center">
  <a href="https://www.star-history.com/droid-deck/droiddeck"><img src="https://api.star-history.com/badge?repo=Droid-Deck/DroidDeck&type=rank&theme=dark" alt="Star History 排名"></a>
</p>

<p align="center">
  <a href="https://trendshift.io/repositories/267852"><img src="https://trendshift.io/api/badge/trendshift/repositories/267852/daily?language=C" alt="Droid-Deck%2FDroidDeck | Trendshift" width="250" height="55"></a>
  <a href="https://trendshift.io/repositories/267852"><img src="https://trendshift.io/api/badge/trendshift/repositories/267852/daily?language=Kotlin" alt="Droid-Deck%2FDroidDeck | Trendshift" width="250" height="55"></a>
</p>

<p align="center"><img src="docs/media/steam-to-desktop.gif" width="100%" alt="在 Retroid Pocket Duo 上：从 DroidDeck 启动器游玩 Steam，在 Steam 大屏幕模式中打开 Pac-Man 并游玩，退出游戏，然后从电源菜单切换到桌面"></p>

> DroidDeck 没有官网。请只从本仓库的 [Releases](https://github.com/Droid-Deck/DroidDeck/releases) 下载。

<p align="center">
  <img src="docs/releases/media/0.6.7/library.gif" width="49%" alt="游戏库切换来源标签、更改卡片尺寸并打开筛选菜单">
  <img src="docs/releases/media/0.3.0/in-game.gif" width="49%" alt="一款通过 Proton 运行的 Windows 游戏，正在使用屏幕控制">
  <img src="docs/releases/media/0.4.0/multi-controller.gif" width="49%" alt="两台手柄在同一台掌机上分屏游玩">
  <img src="docs/releases/media/0.6.7/ubuntu-desktop-discover.jpg" width="49%" alt="Kubuntu 桌面，已打开 Discover">
</p>

## 安装

1. 需要 Android 9 或更新版本，显卡为 Adreno 730 或更新，或任意 8xx。Adreno 6xx 为实验性支持。不支持 Mali、Xclipse、PowerVR 与 Adreno 710。无需 root。
2. 运行时约需 3 GB 可用空间，桌面与模拟器另需 1.1 GB。
3. 安装 [APK](https://github.com/Droid-Deck/DroidDeck/releases/download/0.6.9/DroidDeck-0.6.9.apk)，安装 Linux 运行时，按 **Play** 并登录 Steam。
4. 在 Android 14 及更新版本上，请在开发者选项中开启 **停用子进程限制**（Disable child process restrictions）。在 Android 12 与 13 上，Steam 首次启动会显示 **Fix it for me** 按钮，引导你完成设置。

智能眼镜的设置见[支持矩阵](docs/smart-glasses-support.md)。发布说明见 [docs/releases](docs/releases)。

帮助、预览版构建与设备报告请前往 [Discord](https://discord.gg/JRGAvawjsm)。报告 Bug 请在 **#bug-reports** 发帖，并附上通过 **Share logs** 导出的 zip。

## 构建

需要 Docker、Java 17、Android SDK 与 NDK、通过 rustup 安装的 Rust、GitHub CLI、Python 3 以及 `zstd`。

```bash
tools/build_local.sh
```

APK 输出在 `app/build/outputs/apk/release/`。使用 `tools/deploy_local.sh` 安装。首次运行之后，`./gradlew :app:assembleRelease` 会复用已准备好的输入。单元测试通过 `./gradlew -PskipRust=true :app:testDebugUnitTest` 运行，辅助程序与 preload 测试套件通过 `bash tools/test_local.sh` 运行。

在 Windows 上，参见 `tools/run_windows.ps1` 与 `tools/prepare_windows_runtime.py`。

## 限制

兼容性与性能因设备而异。桌面合成采用软件渲染，Firefox 沙箱在 proot 下有所削弱。

## 致谢与许可证

DroidDeck 采用 [GPL-3.0](LICENSE) 许可证。Steam 与 Proton 归 Valve Corporation 所有。VITURE、RayNeo、XREAL、Rokid、REDMAGIC、Nintendo Switch 与 Joy-Con 为各自所有者的商标。本项目与上述公司均无关联。

<details>
<summary><b>运行时、Wine 与 Proton</b></summary>

- 运行时、shim、输入与控制器相关工作，建立在 maxjivi05 的 WinNative 与 Bannerlator 之上。
- mmap 环形缓冲区控制器输入由 ribbit384 实现（[WinNative #345](https://github.com/WinNative-Emu/WinNative/pull/345)）。
- [FEX](https://github.com/FEX-Emu/FEX)（MIT）负责运行 x86 代码。DroidDeck 的构建把平台名称写入 `AT_PLATFORM`，以便 VHSgunzo 的 [sharun](https://github.com/VHSgunzo/sharun) AppImage（MIT）能通过其 [userland-execve](https://crates.io/crates/userland-execve) 加载器（MIT）启动。FEX 的代码磁盘缓存已在其源码中核对。
- x86 AppImage，以及运行时无法在 Android 上解包的 arm64 AppImage，均使用 VHSgunzo 的 [uruntime](https://github.com/VHSgunzo/uruntime)（MIT）解包，并原样附带其许可证。
- 链接 OpenSSL 3 的 x86 程序使用 [Steam Linux Runtime 4.0](https://gitlab.steamos.cloud/steamrt/steamrt) 中的副本。x86 Linux 游戏会获得由 [pressure-vessel](https://gitlab.steamos.cloud/steamrt/steam-runtime-tools) 写入的 `/etc/asound.conf`。
- `WriteProcessMemory` 钩子会回退到 `/proc/<pid>/mem`，与 Wine 11 之前 [Wine](https://gitlab.winehq.org/wine/wine) 的 `server/ptrace.c` 做法一致。
- Wine 的写监视使用 [Proton](https://github.com/ValveSoftware/wine) 的 `dlls/ntdll/unix/virtual.c`（LGPL-2.1-or-later）中的 userfaultfd 支持。libblsession 提供 arm64 系统调用号，并从 `/proc/self/pagemap` 回应 `PAGEMAP_SCAN`。
- libblsession 在按 [Linux](https://github.com/torvalds/linux) `arch/arm64/kernel/signal.c` 布局的栈帧上原位运行 Wine 的 arm64 `SIGUSR2` 上下文处理程序（`dlls/ntdll/unix/signal_arm64.c`，LGPL-2.1-or-later），并向 `dlls/ntdll/unix/process.c` 中的 `fill_vm_counters` 提供内核自己的 `/proc/<pid>/status`。
- droiddeck-ntsync 遵循 [Linux](https://github.com/torvalds/linux) 在 `Documentation/userspace-api/ntsync.rst` 中定义的 ntsync 语义（GPL-2.0）。
- proot 快速路径按 [PRoot](https://github.com/proot-me/proot) 的 `path/binding.c`（GPL-2.0-or-later）方式排列绑定。
- DiRT 3 与 DiRT Showdown 依照 RoccoC 的 DiRTTelemetryErrorFix（MIT）保留其遥测套接字，该修复基于 Carsten Gottschling 的方案。
- 游戏使用 [Mesa](https://gitlab.freedesktop.org/mesa/mesa) Turnip 的逐渲染通道内存选择。Steam 客户端保持 `TU_DEBUG=sysmem`。
- Black Ops III 与 Dark Souls Remastered 在没有 [umu-protonfixes](https://github.com/Open-Wine-Components/umu-protonfixes)（BSD-2-Clause）的情况下启动，其 `311210.py` 与 `570940.py` 在 ARM64 Proton 下会失败。
- [ReSkate](https://github.com/Dingo-Shenanigans/ReSkate)（GPL-3.0，由 Dingo Shenanigans 开发）使用其 README 中的启动选项与 D3D12 特性级别 12_1。
- arm64 客户端的登录，是通过以 [deadlock-api-ingest](https://github.com/deadlock-api/deadlock-api-ingest) 的 `ConnectCache` 格式读取 Steam 已保存的令牌来核对的。

</details>

<details>
<summary><b>商店</b></summary>

- Epic Games 与 GOG 建立在 [Phobos](https://github.com/phobos665) 对 [Legendary](https://github.com/legendary-gl/legendary) 和 [gogdl](https://github.com/Heroic-Games-Launcher/heroic-gogdl) 的改编之上。GOG 安装脚本的注册表项，按 [Heroic Games Launcher](https://github.com/Heroic-Games-Launcher/HeroicGamesLauncher)（GPL-3.0）的方式读取。
- Epic 官方 Linux 启动器预览通过 FEX 运行。该配置不在 Epic 列出的 Linux 支持范围内。
- Battle.net 由 [@kayakyakr](https://github.com/kayakyakr) 开发。它在 Proton 中运行暴雪自己的客户端（从暴雪下载，未随附），并显示来自 [Battle.net 商店](https://shop.battle.net)的主题图。没有 Steam 时，其游戏会通过 [Proton](https://github.com/ValveSoftware/wine) `dlls/winebus.sys` 中的 `PROTON_SPOOF_STEAMINPUT_VIDPID`，把手柄识别为 Xbox 360 控制器。
- Battle.net 安装按 [@barncastle](https://github.com/barncastle) 的 [Battle.Net-Installer](https://github.com/barncastle/Battle.Net-Installer) 方式驱动暴雪的 Agent，请求细节来自 [wowdev.wiki](https://wowdev.wiki/Agent)。仅阅读、未取用代码的有：[@mubaraknumann](https://github.com/mubaraknumann) 的 [unifideck](https://github.com/mubaraknumann/unifideck)、[@djmittens](https://github.com/djmittens) 的 [neo-mittens](https://github.com/djmittens/neo-mittens) `bnetswitch`、[@BCD1210](https://github.com/BCD1210) 的 [soju](https://github.com/BCD1210/soju) 以及 [Lutris](https://github.com/lutris/lutris) 的 `product.db` 读取器。
- 《魔兽世界》插件导入会从临时副本替换插件文件夹，做法与 [WowUp](https://github.com/WowUp/WowUp) 相同；版本目录读取方式与 [WoWDeck](https://github.com/sigboe/wowdeck) 一致。两者都未取用代码。
- 《魔兽世界》插件下载窗口通过其 MMOUI API 搜索 [WoWInterface](https://www.wowinterface.com)。[CurseForge](https://www.curseforge.com/wow) 在用户浏览器中打开，不使用 API 与密钥。
- Xbox 游戏加载 [@domsvoboda](https://github.com/domsvoboda) 的 [xodus-nexus](https://github.com/domsvoboda/xodus-nexus) 模块（LGPL-2.1-or-later），包括 `wintypes.dll`。Minecraft 的 GameInput 配置已对照 [@Wyze3306](https://github.com/Wyze3306) 的 [BedrockOnLinux](https://github.com/Wyze3306/BedrockOnLinux)（MIT）核对。
- Xbox Cloud Gaming 播放器使用 [@unknownskl](https://github.com/unknownskl) 的 [Greenlight](https://github.com/unknownskl/greenlight)（MIT）的 WebRTC 客户端，其 offer 调整与锐化功能遵循 [@redphx](https://github.com/redphx) 的 [Better xCloud](https://github.com/redphx/better-xcloud)（MIT）。参见 `tools/xcloud-player/NOTICE.md`。
- itch.io 的 butler 包随附 butler 的 `7z.so`，即 itch.io 基于 Igor Pavlov 的 [7-Zip](https://www.7-zip.org) 构建的版本（LGPL-2.1，RAR 代码带 unRAR 限制），未作修改。其他压缩包使用 Apache Commons Compress 与运行时的 bsdtar（libarchive）。
- 商店通过 DroidDeck 自有的 bubblewrap shim 使用 [Flatpak](https://github.com/flatpak/flatpak)（LGPL-2.1）。Chromium 与 Electron 应用直接启动，因为 [@refi64](https://github.com/refi64) 的 [zypak](https://github.com/refi64/zypak)（BSD-3-Clause）始终使用 Flatpak 的 spawn portal。
- 文件夹同步运行 [Syncthing](https://github.com/syncthing/syncthing)（MPL-2.0），基于 [@researchxxl](https://github.com/researchxxl) 的 [syncthing-android](https://github.com/researchxxl/syncthing-android) 分支（MPL-2.0）构建。

</details>

<details>
<summary><b>音频、视频与 HUD</b></summary>

- Steam 音频经由 [@The412Banner](https://github.com/The412Banner) 的 [DirectAudio](https://github.com/The412Banner/directaudio) PulseAudio 汇（LGPL-2.1-or-later），由 `tools/directaudio/sink` 构建。
- DroidDeck HUD 为 [@The412Banner](https://github.com/The412Banner) 的 [FusionHUD](https://github.com/The412Banner/FusionHUD)（GPL-3.0，含署名条款）。其读数与 Valve 的 [MangoHud](https://github.com/flightlessmango/MangoHud) 构建（MIT）一致，帧时间来自 gamescope 补丁 0125。
- HUD 的 CPU、GPU 负载、温度与电池读数来自 [@xXJSONDeruloXx](https://github.com/xXJSONDeruloXx) 的工作。
- LSFG 帧生成来自 Camille LaVey 与 [Eden](https://eden-emu.dev) 项目，遵循 [lsfg-vk](https://github.com/PancakeTAS/lsfg-vk)，由 [@maxjivi05](https://github.com/maxjivi05) 移植。它需要你自己拥有的 [Lossless Scaling](https://store.steampowered.com/app/993090/) 副本，且不随附其任何着色器。
- 远程游玩通过 Android 的 MediaCodec 或 Steam 运行时中的 [FFmpeg](https://ffmpeg.org)（LGPL-2.1-or-later）解码。高通低延迟设置遵循 [Moonlight](https://github.com/moonlight-stream/moonlight-android)（GPL-3.0）。
- [PipeWire](https://gitlab.freedesktop.org/pipewire/pipewire)（MIT）套接字使用其 `module-access.c` 中的设置，[WirePlumber](https://gitlab.freedesktop.org/pipewire/wireplumber)（MIT）让这些客户端保持完整权限。

</details>

<details>
<summary><b>桌面</b></summary>

- KDE 的 pacman 仓库快照自 [Arch Linux ARM](https://archlinuxarm.org)，并通过 [pacman](https://gitlab.archlinux.org/pacman/pacman)（GPL-2.0-or-later）安装。
- [KWin](https://invent.kde.org/plasma/kwin)（GPL-2.0-or-later）经过修补，以传递其 `wayland_backend.cpp` 中仍标为 TODO 的滚动数据。
- 仅阅读、未取用代码的有：[Qt](https://code.qt.io/cgit/qt/qtbase.git) 的 `qwaylandinputdevice.cpp` 与 `qguiapplication.cpp`、[Konsole](https://invent.kde.org/utilities/konsole) 的 `TerminalDisplay.cpp`，以及 [Mesa](https://gitlab.freedesktop.org/mesa/mesa) 的 `u_threaded_context.c` 与 `zink_resource.c`。
- Steam 键盘的窗口规则遵循 Valve 的 `steamdeck-kde-presets`，并参考 [Bazzite](https://github.com/ublue-os/bazzite) 的 `0001-steam-virtual-keyboard.patch`。
- Steam 的浏览按钮通过 PyGObject（LGPL-2.1-or-later）打开 GTK 3 文件选择器，并由 [gamescope](https://github.com/ValveSoftware/gamescope)（BSD-2-Clause）经 `GetAppIdFromReaper` 显示在 Steam 之上。
- AYN Odin 2 的幻影第二显示屏，依据 [@SapphireRhodonite](https://github.com/SapphireRhodonite) 的 [WatermelonDS 0.0.5](https://github.com/SapphireRhodonite/WatermelonDS/releases/tag/0.0.5) 说明与 [iiSU #363](https://github.com/iisu-network/iiSU/issues/363) 得到确认，未取用其代码。

</details>

<details>
<summary><b>智能眼镜</b></summary>

- 建立在 WinNative 的 VITURE 工作之上。协议已对照 [@wheaney](https://github.com/wheaney) 的 [XRLinuxDriver](https://github.com/wheaney/XRLinuxDriver)、[@badicsalex](https://github.com/badicsalex) 的 [ar-drivers-rs](https://github.com/badicsalex/ar-drivers-rs) 与 [go-macos/iokit](https://pkg.go.dev/github.com/go-macos/iokit/viture) 核对。可选的 VITURE SDK 未包含在内。
- VITURE 协议 V2 帧结构改编自 Holger Bartnick 基于 [@elasticjava](https://github.com/elasticjava) 的 [viture-v2](https://github.com/elasticjava/viture-v2)（MIT）的工作。参见 `docs/third-party/viture-v2`。
- RayNeo 支持改编自 [@informalTechCode](https://github.com/informalTechCode) 的 [RayNeo-Air-Series-3DoF-Scaffold](https://github.com/informalTechCode/RayNeo-Air-Series-3DoF-Scaffold)（Apache-2.0），并参考 [@verncat](https://github.com/verncat) 的 [RayNeo-Air-3S-Pro-OpenVR](https://github.com/verncat/RayNeo-Air-3S-Pro-OpenVR)（MIT）与 [Monado](https://gitlab.freedesktop.org/monado/monado)（BSL-1.0）。参见 [docs/third-party/rayneo](docs/third-party/rayneo/README.md)。
- XREAL 1S 参考 XRLinuxDriver 与 @wheaney 的 [xrealOneDeviceKit](https://github.com/wheaney/xrealOneDeviceKit)、[@rohitsangwan01](https://github.com/rohitsangwan01) 的 [xreal_one_driver](https://github.com/rohitsangwan01/xreal_one_driver)（MIT）以及 [@0xcaff](https://github.com/0xcaff) 的 [xr-tools](https://github.com/0xcaff/xr-tools)。
- Rokid 遵循 ar-drivers-rs、@badicsalex 的 [Void Computing 文章](https://voidcomputing.hu/blog/worse-better-prettier/) 与 Monado 的 Rokid 驱动。
- REDMAGIC 显示相关结论参考了 [@mekhontsev](https://github.com/mekhontsev) 的 [MagicDesk 调查](https://github.com/mekhontsev/magicdesk/blob/main/docs/xr-resolution-fix.md) 与 [@Coding-BR](https://github.com/Coding-BR) 的[反编译 Nubia 内核](https://github.com/Coding-BR/android_kernel_nubia_sm8850_qwjujube)。

</details>

<details>
<summary><b>VR、Joy-Con 与手部追踪</b></summary>

- Joy-Con VR 移植了 Google [GVR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk)（Apache-2.0）的手臂模型，并改编了 Valve [OpenVR](https://github.com/ValveSoftware/openvr) `handskeletonsimulation` 示例（BSD-3-Clause）的手部骨骼，该示例遵循 Moshi Turner 在 Monado 中的工作。Index 偏移来自 Monado 的 `vive_poses.c`（BSL-1.0）。
- Joy-Con 研究：Daniel J. Ogorchock 的 Linux [hid-nintendo](https://github.com/torvalds/linux/blob/master/drivers/hid/hid-nintendo.c)、[@dekuNukem](https://github.com/dekuNukem) 的 [Nintendo_Switch_Reverse_Engineering](https://github.com/dekuNukem/Nintendo_Switch_Reverse_Engineering)、[SDL](https://github.com/libsdl-org/SDL) 与 AOSP。已对照 [ALVR](https://github.com/alvr-org/ALVR)、[OpenComposite](https://gitlab.com/znixian/OpenOVR)、[PSMoveSteamVRBridge](https://github.com/HipsterSloth/PSMoveSteamVRBridge)、[@F8LEFT](https://github.com/F8LEFT) 的 [JoyConVr](https://github.com/F8LEFT/JoyConVr)、[@krnelpan1c](https://github.com/krnelpan1c) 的 [Daydream-Controller-SteamVR](https://github.com/krnelpan1c/Daydream-Controller-SteamVR)、[@yoyobuae](https://github.com/yoyobuae) 的 [joyconvr](https://github.com/yoyobuae/joyconvr)、[Moonlight](https://github.com/moonlight-stream/moonlight-android) 与 [Dolphin](https://github.com/dolphin-emu/dolphin) 核对。参见 [docs/third-party/joycon](docs/third-party/joycon/README.md)。
- 摄像头手部追踪在 [CameraX](https://developer.android.com/jetpack/androidx/releases/camera) 帧（Apache-2.0）上运行 Google [MediaPipe](https://github.com/google-ai-edge/mediapipe) 的 Pose Landmarker (Lite)、BlazePose GHUM 3D 与 Hand Landmarker（Apache-2.0），并已关闭使用情况报告。已对照 JoyConVr 的相机应用（MIT）核对。
- 追踪数学：GVR 手臂模型的加速度计模式、Eric Foxlin 的零速更新、Frosio、Pedersini 与 Borghese 的加速度计自动校准、卡尔曼滤波（Kalman，1960）、Horn 的四元数方法（1987）以及 1€ 滤波（Casiez、Roussel 与 Vogel，2012）。
- 手势遵循 [ALVR](https://github.com/alvr-org/ALVR) 的 `hand_gestures.rs`（MIT）与 Meta Quest 的菜单捏合，并在 [HaGRID](https://github.com/hukenovs/hagrid) 照片（CC BY-SA 4.0，未随附）上调优。
- 手套模型遵循 OpenVR 的渲染模型参考，使用表面网格（Gibson，1998；Mikola Lysenko）生成网格，并采用 Inigo Quilez 的平滑最小值、距离场环境光遮蔽与多边形距离（MIT）。参见 [docs/third-party/handtracking](docs/third-party/handtracking/README.md)。

</details>

## Star 历史

<a href="https://www.star-history.com/#Droid-Deck/DroidDeck&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=Droid-Deck/DroidDeck&type=Date&theme=dark">
    <img alt="Star 历史图表" src="https://api.star-history.com/svg?repos=Droid-Deck/DroidDeck&type=Date" width="100%">
  </picture>
</a>
