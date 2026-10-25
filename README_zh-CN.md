<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="artwork/droiddeck-banner-dark.svg">
    <img alt="DroidDeck" src="artwork/droiddeck-banner-light.svg" width="100%">
  </picture>
</p>

DroidDeck 将 SteamOS 体验带到 Android：在你的 Adreno 掌机上运行 Valve 的 Steam 客户端（Big Picture 模式），并通过 Valve 的 ARM64 Proton 运行 Windows 游戏。

<p align="center"><a href="https://discord.gg/JRGAvawjsm"><img src="https://img.shields.io/badge/Discord-Join%20the%20community-5865F2?logo=discord&logoColor=white" alt="Join the DroidDeck Discord"></a></p>

<p align="center"><img src="docs/releases/media/0.2.0/launch-into-steam.gif" width="80%" alt="Tapping DroidDeck on the Android home screen and landing in Steam Big Picture"></p>

> 注意：DroidDeck 没有独立的网站。不要点击任何声称是 DroidDeck 团队的网站上的下载链接。

## 要求与安装

在受支持的 Adreno 设备（730 或更新，或 8xx）上使用 Android 9 或更新版本。Adreno 6xx 是实验性的：DirectX 11 使用 DXVK 2 且可能运行，而 DirectX 12 游戏仍可能崩溃。不支持 Mali、Xclipse、PowerVR 和 Adreno 710。无需 root。为运行时预留约 3 GB，为桌面和模拟器再预留 1.1 GB。从 [Releases](https://github.com/Droid-Deck/DroidDeck/releases) 安装 APK，安装 Linux 运行时，然后按 **Play** 并登录。Steam 会在首次启动时下载。安装 **Desktop & apps** 以使用桌面和模拟器。**Store** 使用 Flatpak 从 Flathub（ARM64 构建）安装 Linux 应用和游戏；还有安装 Appimage 和设置脚本的附加选项。**Stores**（设置 › Stores › “Show Stores in the rail”）登录 GOG、Epic Games 和 Amazon Games，并把游戏下载到 Games 存储。Epic 保留其直接 API 安装路径。打开 Epic Stores 会准备 Epic 的官方 Linux 启动器预览。安装后，从 DroidDeck 启动的 Epic 游戏使用该客户端，客户端中安装的游戏会导入到 DroidDeck 的库和快捷方式中。x86-64 客户端通过 FEX 在 ARM 上运行，DroidDeck 保留其 Proton 钩子；此预览和 ARM 设置在 Epic 列出的 Linux 支持范围之外。商店芯片行上的齿轮图标可设置其打开选项卡和下载速度等级。

在 Android 14 及更新版本上，在 Steam 启动前，你必须开启开发者选项中的 **Disable child process restrictions**（禁用子进程限制）。如果开发者设置中没有此选项（Android 12 和 13 设备），Steam 首次启动时会显示一个 “Fix it for me” 按钮，可帮助自动化设置过程。

## 智能眼镜

使用 **设置 → Smart glasses** 在 DroidDeck 菜单、启动器、桌面会话和游戏中设置全应用范围的虚拟屏幕设置。受支持的 VITURE、RayNeo 和 XREAL 连接提供 3DoF；Luma Ultra 增加可选的实验性 6DoF 和立体辅助手部/Joy-Con 追踪。型号支持和 SDK 要求不同：支持设备、控制、设置和限制请见 [智能眼镜支持矩阵](docs/smart-glasses-support.md)。

## 社区

加入 [DroidDeck Discord](https://discord.gg/JRGAvawjsm) 获取帮助、预览版和设备报告。Bug 报告放在其 **#bug-reports** 论坛；附上 **Share logs**（设置，或会话抽屉）导出的 zip，以便日志随附。

## 构建

运行 `tools/build_local.sh`，需要 Docker、Java 17、Android SDK/NDK、通过 rustup 的 Rust、GitHub CLI、Python 3 和已安装的 `zstd`。它准备与 CI 相同的原生库、锁定组件和音频源，然后检查 APK 的资源、JNI 导出和依赖。APK 位于 `app/build/outputs/apk/release/app-release.apk`。设置 `DROIDDECK_PA13_SOURCE_DIR` 指向现有的 PulseAudio 13.0 源码目录可跳过下载。设置 `DROIDDECK_SIGNING_ENV` 指向签名环境文件，可在发布构建之上安装；相对 keystore 路径相对于该文件解析。要在已连接的设备上安装，运行 `tools/deploy_local.sh`。

准备之后，普通的 `./gradlew :app:assembleRelease` 构建可复用已验证的输入。如果原生源码、组件锁定或暂存文件发生变化，打包会停止并提示重新运行辅助程序。生成的音频留在 `app/build/` 下；被跟踪的基础包永不修改。`./gradlew -PskipRust=true :app:testDebugUnitTest` 无需原生准备。若要构建不含商店引擎的 APK，使用 `DROIDDECK_SKIP_RUST=1 tools/build_local.sh`；它也会省略缓存的引擎库。

对于准备好的 Windows 检出，运行 `./tools/run_windows.ps1`（可选 `-Serial <adb-serial>`），或在 Android Studio 中选择 **DroidDeck XR** 并在已连接手机上运行 **app** 模块。机器相关选项在忽略的 `local.properties` 中：`droiddeck.python` 选择 Python 可执行文件，`droiddeck.skipRust=true` 省略可选商店下载引擎，`droiddeck.debugApplicationIdSuffix=.xr` 在签名发布版旁边安装 **DroidDeck XR Dev**。开发应用有自己的运行时和 Steam 登录。本地 Java 选择在 `.gradle/config.properties` 中。

`python tools/prepare_windows_runtime.py` 从锁定的 CI APK、三个本地重建的 preload 库和 `build/windows-setup/` 中校验和锁定的 Gamescope 归档恢复此检出的已审计运行时输入。先启动 Docker Desktop 的 Linux 引擎：准备过程现在会从当前源码重建 DroidDeck 的 ARM64 VR 驱动和 Vulkan 层，并运行其原生回归测试。编译器镜像在首次使用时自动构建。Vulkan 头文件来自 NDK（`--llvm-strip` 旁边），或来自显式的 `--vk-include <NDK-sysroot-include>` 目录。

要复用该构建，传入 `--rebuilt-vr build/windows-setup/vr`。准备过程会将其 `vr-build.json` 与当前 Git 修订版、源文件、Vulkan 头文件、输出校验和和 ARM64 架构进行校验；缺失或过期的构建会失败，而不是回退到捐赠者的 VR 库。脚本检查实际捐赠者 CI 合并树，仍拒绝无关的未审计运行时更改，并在 `app/build/windows-runtime-provenance.json` 中记录组件来源。本地重建时保留这些被忽略的准备文件；`clean` 会移除准备好的资源和其验证清单。

用 `bash tools/test_local.sh` 运行辅助程序和 preload 回归套件。在 macOS 上，它通过 Docker 在 Linux 中运行，与辅助程序的运行时和 CI 一致，而不是针对 macOS 头文件编译 Linux preloads。

## 限制

兼容性和性能因设备而异；硬件验证有限。桌面合成使用软件渲染。Firefox 沙箱在 proot 下被削弱。日志保持应用私有（`files/logs/`，最近 30 个会话）；**Share logs** 打包最新会话，连同商店和工具日志，放入一个清理过的 zip。

## 鸣谢与许可证

GPL-3.0。运行时、垫片、输入和控制器工作建立在 WinNative 和 Bannerlator（maxjivi05）之上。Epic Games 和 GOG 支持分别建立在 [Phobos](https://github.com/phobos665) 对 [Legendary](https://github.com/legendary-gl/legendary) 和 [gogdl](https://github.com/Heroic-Games-Launcher/heroic-gogdl) 的改编之上。LSFG 帧生成来自 Camille LaVey 和 [Eden](https://eden-emu.dev) 模拟器项目的工作，遵循 [lsfg-vk](https://github.com/PancakeTAS/lsfg-vk)，由 [@maxjivi05](https://github.com/maxjivi05) 移植到 WinNative 和 DroidDeck；它需要你自己拥有的 [Lossless Scaling](https://store.steampowered.com/app/993090/) 副本，且不附带任何其着色器。x86 AppImage 以及任何其自身运行时无法解包的应用，都由 VHSgunzo 的 [uruntime](https://github.com/VHSgunzo/uruntime)（MIT）解包，原样附带其许可证。Steam 客户端的音频通过 [@The412Banner](https://github.com/The412Banner) 的 [DirectAudio](https://github.com/The412Banner/directaudio) PulseAudio 汇（LGPL-2.1-or-later）。mmap 环形缓冲控制器输入由 ribbit384 开发（[WinNative #345](https://github.com/WinNative-Emu/WinNative/pull/345)）。游戏用 [Mesa](https://gitlab.freedesktop.org/mesa/mesa) Turnip 渲染。见 [LICENSE](LICENSE)。Steam 和 Proton 属于 Valve Corporation；本项目与 Valve 无关联。

XR 眼镜支持建立在 WinNative 的 VITURE 眼镜工作之上。VITURE 协议细节对照 [XRLinuxDriver](https://github.com/wheaney/XRLinuxDriver)（含其随附的 VITURE 协议头文件）、[ar-drivers-rs](https://github.com/badicsalex/ar-drivers-rs) 和 [go-macos/iokit VITURE 包](https://pkg.go.dev/github.com/go-macos/iokit/viture)；可选的 VITURE SDK 属于 VITURE，未包含。RayNeo 支持改编自 [@informalTechCode](https://github.com/informalTechCode) 的 [RayNeo-Air-Series-3DoF-Scaffold](https://github.com/informalTechCode/RayNeo-Air-Series-3DoF-Scaffold)（Apache-2.0）并参考 [@verncat](https://github.com/verncat) 的 [RayNeo-Air-3S-Pro-OpenVR](https://github.com/verncat/RayNeo-Air-3S-Pro-OpenVR)（MIT）和 [Monado](https://gitlab.freedesktop.org/monado/monado)（BSL-1.0）。XREAL 1S 支持参考 XRLinuxDriver 和 @wheaney 的 [xrealOneDeviceKit](https://github.com/wheaney/xrealOneDeviceKit)、[@rohitsangwan01](https://github.com/rohitsangwan01) 的 [xreal_one_driver](https://github.com/rohitsangwan01/xreal_one_driver)（MIT）和 [@0xcaff](https://github.com/0xcaff) 的 [xr-tools](https://github.com/0xcaff/xr-tools)。VITURE、RayNeo、XREAL 和 REDMAGIC 是其所有者的商标；本项目与它们无关联。

Joy-Con VR 控制器移植了 Google 的 [Google VR SDK for Unity](https://github.com/googlevr/gvr-unity-sdk)（Apache-2.0）手臂模型，改编了 Valve [OpenVR](https://github.com/ValveSoftware/openvr) `handskeletonsimulation` 示例（BSD-3-Clause）的手部骨骼，并使用 Monado `vive_poses.c` 的 Index 控制器偏移（BSL-1.0）。Joy-Con 行为参考了 Linux 的 [hid-nintendo](https://github.com/torvalds/linux/blob/master/drivers/hid/hid-nintendo.c) 驱动、[@dekuNukem](https://github.com/dekuNukem) 的 [Nintendo_Switch_Reverse_Engineering](https://github.com/dekuNukem/Nintendo_Switch_Reverse_Engineering)、[SDL](https://github.com/libsdl-org/SDL) 和 Android Open Source Project。Nintendo Switch 和 Joy-Con 是任天堂的商标；本项目与任天堂无关联。

相机手部追踪运行 Google 的 [MediaPipe](https://github.com/google-ai-edge/mediapipe) Tasks Vision 库及其 Pose Landmarker (Lite) 模型、BlazePose GHUM 3D 和 Hand Landmarker 模型（均 Apache-2.0），处理来自 AndroidX [CameraX](https://developer.android.com/jetpack/androidx/releases/camera)（Apache-2.0）的帧；MediaPipe 的使用情况报告已关闭。手势参考了 [@F8LEFT](https://github.com/F8LEFT) 的 [JoyConVr](https://github.com/F8LEFT/JoyConVr) 相机应用（MIT）、[ALVR](https://github.com/alvr-org/ALVR) 的 `hand_gestures.rs`（MIT）等。见 [docs/third-party/handtracking](docs/third-party/handtracking/README.md)。

Store 使用 [Flatpak](https://github.com/flatpak/flatpak)（LGPL-2.1）安装和运行应用。其沙箱通过 DroidDeck 自己的 bubblewrap 垫片进行，遵循 Flatpak 将 bwrap 环境交给它的方式。
