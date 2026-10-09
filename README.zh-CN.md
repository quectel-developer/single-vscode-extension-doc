# Quectel RTOS

[![Quectel](https://img.shields.io/badge/Quectel-IoT%20Solutions-orange)](https://www.quectel.com.cn) [![License](https://img.shields.io/badge/license-Apache%202.0-blue)](https://www.apache.org/licenses/LICENSE-2.0) [![VS Code](https://img.shields.io/badge/VS%20Code-1.85.0+-blueviolet)](https://code.visualstudio.com/)

<!-- 需要保持相对地址的链接刻意写成 HTML：未配置重写基准地址时，`vsce` 会因为一个相对
     markdown 链接直接让打包失败。dev/verify-docs.mjs 会检查这一点。 -->

<a href="README.md">English</a> &nbsp;·&nbsp; **简体中文**

**Quectel RTOS** 将三套移远嵌入式开发工具链集成到一个 VS Code 扩展中。安装一次后，即可通过面板顶部的 **SOLUTION** 栏切换工具链 —— 无需安装第二个扩展，也无需重载窗口。

| 产品 | 用途 | 宿主平台 |
|---|---|---|
| **UniKnect** | 基于 UniKnect SDK 的 MCU 固件开发 —— 通过 ST-Link 编译、烧录与调试，并提供集成串口控制台 | Windows、Linux |
| **UniRTOS** | UniRTOS 工程创建、编译、清理、烧录与调试，使用移远烧录工具 | Windows |
| **QuecPython** | 移远模组上的 MicroPython 开发 —— 文件传输、交互式 REPL、固件烧录与设备端文件系统 | Windows |

只有当前选中产品的面板、状态栏按钮和编辑器集成处于活动状态。切换产品时会释放上一个产品的资源，所选产品会在重启后保持不变。

---

## 目录

本指南面向插件使用者，帮助你快速上手、理解面板，并找到对应工具链的说明：

- [快速开始](#快速开始) · [面板](#面板) · [环境要求](#环境要求) · [设置项](#设置项) · [配套扩展](#配套扩展)
- 产品章节：[UniKnect](#uniknect--mcu-开发) · [UniRTOS](#unirtos--模组-opencpu-开发) · [QuecPython](#quecpython--模组-micropython-开发)
- [打包说明](#打包说明) · [About Quectel](#about-quectel) · [许可证](#许可证)

---

## 快速开始

1. 从扩展市场或 `.vsix` 文件**安装本扩展**。
2. 在 VS Code 中**打开你的工程目录**。
3. 点击活动栏中的 **Quectel RTOS 图标**打开面板。
4. 在 **SOLUTION** 栏中**选择工具链**。
5. 通过面板按钮执行流程，例如 **Install SDK → Configure → Build → Flash → Debug**。

部分操作会在首次运行时自动下载所需工具（见[首次使用时自动下载](#首次使用时自动下载)），这一次需要联网；之后即可离线运行。

---

## 面板

三套产品共用**同一个**面板和同一套交互方式，因此切换产品后布局与操作习惯保持一致：

| 控件 | 位置 | 作用 |
|---|---|---|
| **SOLUTION** 栏 | 面板顶部 | 显示当前产品。鼠标悬停可展开另外两个产品，点击即可切换 |
| **分组** 栏 | 每个分组 | 折叠或展开该分组。右侧数字表示该分组包含的按钮数量 |
| **语言**（地球图标） | 首组标题栏 | 在本窗口内切换扩展自身文案的中英文 |
| **布局**（图层 / 列表图标） | 首组标题栏 | 在卡片网格与逐行列表之间切换；逐行列表会显示完整按钮名，选择会被记住 |
| **恢复按钮** （回退图标） | 首组标题栏 | 当你拖动调整过按钮顺序后出现，用于恢复默认顺序 |

按钮支持**拖动排序**，且每个产品的顺序单独保存。

### 产品切换

**SOLUTION** 栏静止时是一个按钮：显示当前产品、名称和高亮边框。右侧折角箭头旁的数字表示产品数量，提示还有其他可选项。

鼠标悬停时，另外两个产品会在下方展开。展开区域会**把面板其余内容向下推开**，而不是浮在其上，因此不会遮挡下方控件。也支持键盘操作：按 Tab 进入该栏即可展开，再继续按 Tab 在产品间移动。

切换产品会在原位替换面板内容、状态栏按钮与编辑器集成。视图本身不会被销毁，因此侧边栏不会闪烁或重置。

### 面板语言

**每次打开窗口都跟随 VS Code 当前语言自适应，地球图标的切换仅对当前窗口生效。**

1. **编辑器的显示语言** —— 中文版 VS Code 无需任何配置即为中文面板，英文版即为英文面板；
2. **点击地球图标**，在本次会话剩余时间内切换语言。

这个临时覆盖**不会被记住**，所以下一个窗口仍从编辑器语言开始，也就没有需要重置的设置项。该设置只影响扩展自身的文案：VS Code 的菜单、设置页和通知跟随编辑器的显示语言，那是 VS Code 的设置。

---

## UniKnect —— MCU 开发

面板包含四个分组，显示顺序如下。

### WELCOME

| 按钮 | 作用 |
|---|---|
| **Install SDK** | 克隆 UniKnect SDK。会询问**版本**和**安装路径**。若已安装 SDK，同一按钮还会提供 **Update**，拉取并合并到该目录 |
| **Configure** | 选择目标芯片型号并配置编译环境 |
| **Samples** | 浏览 SDK 自带的示例工程 |
| **Open** | 在当前窗口打开已有工程。详见[打开已有工程](#打开已有工程) |
| **Code Repo** | 在浏览器中打开 UniKnect 的 GitHub 仓库 |
| **Terminal** | 打开工作台共用终端 |

### BUILD & DEBUG

| 按钮 | 作用 |
|---|---|
| **Build** | 编译工程 |
| **Clean** | 清除编译产物 |
| **Flash** | 在检查 ST-Link 驱动后将固件烧录到目标板 |
| **Debug** | 启动 `cortex-debug` 调试会话 |
| **Serial** | 打开 **串口面板** —— 串口参数配置、CLI 命令构造器，以及设备返回内容的实时日志 |

### MEMORY REPORTS
以两个仪表盘显示编译产出的 ROM 与 RAM 占用，并在每次编译后自动刷新，无需重载。

### AI ASSISTANT

| 按钮 | 作用 |
|---|---|
| **Copilot** | 快速打开/关闭 GitHub Copilot Chat |
| **API Key** | 打开 OpenAI 兼容服务设置，配置 base URL 与 API Key |

### 打开已有工程

**Open** 会弹出目录选择框，并把选中的目录设为**当前窗口**的工作区。

- 扩展**不会**校验该目录是否为 UniKnect 工程。若选错目录，VS Code 会回到原来的位置，不会丢失任何东西。

### 状态栏按钮

UniKnect 处于活动状态时，状态栏会在 VS Code "无问题"项右侧依次显示**六项**：

| 项目 | 悬停提示 | 点击 |
|---|---|---|
| **Build** | `Build project` | 编译 |
| **Flash** | `Flash firmware` | 烧录 |
| **Debug** | `Start debugging` | 调试 |
| **Serial** | `Open serial panel` | 打开串口面板 |
| **ROM** | ROM usage in KB | 编译产出报告后显示 ROM 占用 |
| **RAM** | RAM usage in KB | 编译产出报告后显示 RAM 占用 |

四个操作按钮使用与面板按钮**相同的命令 ID**，因此行为完全一致。两个占用读数不可点击 —— 它们把面板仪表盘中的数值放到状态栏，便于编译期间观察；在编译报告生成前显示 `--`，因为 `0%` 会暗示一个尚未生成的二进制文件已经存在。

#### 一键调试

一键启动 `cortex-debug` 调试，支持断点、单步执行、变量/监视（Watch）、调用栈等信息查看等。

---

## UniRTOS —— 模组 OpenCPU 开发
- 目前仅支持Windows环境

**Commands** 视图覆盖完整流程：

| 按钮 | 作用 |
|---|---|
| **Welcome** | 打开引导页（工具链、Python、Git、驱动、CLI） |
| **New Project** | 从零创建工程 |
| **New Project From Demo** | 基于内置示例工程创建工程 |
| **Open Existing Project** | 打开已有工程。 |
| **Clean** | 清除编译产物 |
| **Build** | 使用 UniRTOS 工具链编译工程 |
| **Flash** | 通过 `FlashToolCLI.exe` 将固件烧录到目标板 |
| **Debug** | 启动调试会话 |
| **Document Center**、**GitHub**、**Forum**、**Official Website** | 在浏览器中打开对应资源 |

欢迎页上的**引导流程**会按所需顺序完成工具链、Python、Git、移远驱动与 UniRTOS CLI 的安装指引。

---

## QuecPython —— 模组 MicroPython 开发
- 目前仅支持Windows环境

一个面板包含三个分组。

### WELCOME

| 控件 | 作用 |
|---|---|
| 固件状态条 | 显示当前选中的固件镜像，未选择时显示提示 |
| **Select Firmware** | 选择要烧录的固件镜像 |
| **Flash Firmware** | 将选中的固件烧录到模组 |
| **Clear**（分组标题栏） | 清除当前固件选择 |

### QUICK ACCESS

| 按钮 | 作用 |
|---|---|
| **Projects** | 内置工程页面 |
| **Current Project** | 当前打开的工程 |
| **My Projects** | 你保存的工程 |

### FILE SYSTEM (USR)

设备端文件系统以卡片形式与其他两个分组并列展示。每个节点的右键菜单提供 **Run Script**、**Delete File**、**Export File**、**Delete Directory**，分组标题栏提供 **Refresh**。

模组连接后，还可从命令面板执行：连接 / 断开串口、下载文件或文件夹到设备、格式化文档、设置终端行结束符、切换十六进制转换、清空终端。

扩展内置了 QuecPython 各模块的 `.pyi` 存根，Pylance 可直接解析 `import machine` 等导入。

---

## 环境要求

- **VS Code 1.85.0 或更高版本。**
- **UniKnect 不依赖任何预装工具。** 它所需的内容要么随扩展提供，要么在首次使用时自动获取 —— Git（优先使用系统已安装版本，没有则自动下载便携版）、ST-Link 驱动与烧录工具。因此在一台只安装了 VS Code 的机器上即可使用，并同时支持 Windows 与 Linux。
- **Git** —— UniRTOS 与 QuecPython 在克隆仓库和执行部分 CLI 命令时需要。请从[官网](https://git-scm.com/downloads)安装，并确保其已加入系统 `PATH`（Windows 安装时勾选 *"Git from the command line and also from 3rd-party software"*）。安装后重启 VS Code，并在终端中用 `git --version` 验证。
- **UniRTOS 与 QuecPython 仅支持 Windows。** 在 Linux 与 macOS 上它们会以禁用状态显示并给出原因。在 Linux 上请确保工程的 `build.sh` 具有可执行权限（`chmod +x build.sh`）。
- 烧录与硬件调试需要**调试探针** —— UniKnect 使用 ST-Link。其驱动会由扩展自动安装，见下文。

### 首次使用时自动下载

有六个 Windows 工具**没有**打包进安装包：它们合计约 120 MB，而扩展其余内容不足 8 MB，且每个工具只服务于一个操作。首次执行相关操作时会自动下载，并在窗口角落显示进度通知（含百分比与进度条）：

| 工具 | 下载时机 | 大小 |
|---|---|---|
| `QuecPythonDownload.exe`、`get_com.exe`、`QPYcom.exe` | QuecPython：点击 **Flash Firmware** | 97.3 MB |
| `FlashToolCLI.exe`（Eigen_718） | UniRTOS：打开 **Flash** 页面 | 11.7 MB |
| 便携版 Git | UniKnect：**Install SDK**，且本机尚无该压缩包 | 6.3 MB |
| ST-Link 驱动（STSW-LINK009） | UniKnect：**Flash** 或 **Debug**，且本机尚无该压缩包 | 5.1 MB |

每个工具都会被解压到它在完整安装包中本应所在的位置，因此运行行为保持一致。每次下载前都会**先判断是否需要**，并在 **Quectel RTOS** 输出通道中记录结果 —— 已存在于本机的工具不会重复下载，直到扩展升级到新版本。

**UniKnect 的 Flash 与 Debug 会先检查 ST-Link 驱动**，缺失时下载并安装，之后才开始编译。检查每次点击都会执行，并且没有"不再询问"选项；但已安装的驱动可通过两种方式识别 —— Windows 已安装驱动列表，或已连接且绑定到正确驱动的探针 —— 因此常见情况下不会产生额外动作。两种判断按成本从低到高执行，以保持点击响应速度。若判断有误而你的探针确实可用，可选择 **Continue Anyway** 继续编译；关闭提示则中止。驱动安装成功即视为检查通过，不需要拔插目标板。所有判断过程都会写入 **Quectel RTOS** 输出通道。

---

## 设置项

| 设置项 | 默认值 | 说明 |
|---|---|---|
| `quectel.uniknect.sdkPath` | `""` | UniKnect SDK 的安装或克隆路径，**Install SDK** 也会写入此项 |
| `unirtos.mirrorSource` | `github` | UniRTOS CLI 命令使用的仓库镜像源 |
| `QuecPython.defaultLineTerminator` | `\r\n` | 追加到每条发送到模组的命令末尾，支持转义字符 |
| `QuecPython.translateHex` | `true` | 是否转换接收到的十六进制数据。设为 `false` 则输出原始十六进制 |
| `QuecPython.outputChannelToFile` | `false` | 是否同时将 QuecPython 终端输出写入日志文件 |
| `QuecPython.showRunCommandInEditorContextMenu` | `true` | 是否在编辑器右键菜单中显示 **Run Code** |
| `QuecPython.userProjects` | `[]` | 你保存的 QuecPython 工程列表 |

---

## 配套扩展

本扩展同时以**扩展包（Extension Pack）**形式声明了以下配套扩展，安装时 VS Code 会提示一并安装。它们都不是必需项：各产品在缺少它们时仍可正常工作，配套扩展只补充下列集成能力。

| 扩展 | 补充的能力 |
|---|---|
| `marus25.cortex-debug` | UniKnect 与 UniRTOS 中 **Debug** 使用的调试适配器。缺少时会提示未找到调试配置 |
| `GitHub.copilot-chat` | **AI Assistant** 的对话能力，以及串口日志工具的宿主（提示词中通过 `#quectelSerialLog` 引用） |
| `johnny-zhao.oai-compatible-copilot` | **API Key** 配置的 OpenAI 兼容服务 |
| `ms-vscode.cpptools` | SDK 与固件源码的 C/C++ 智能感知 |
| `ms-vscode.vscode-serial-monitor` | VS Code 自带的串口监视器，可与内置串口面板并用 |
| `ms-python.autopep8` | QuecPython 脚本的格式化 |
| `ms-ceintl.vscode-language-pack-zh-hans` | 编辑器自身的中文（简体）界面。扩展自带中文文案，不依赖此项 |

---

## 打包说明

README 与 CHANGELOG 不使用相对 Markdown 链接，以便 `vsce package` 无需额外的链接重写参数即可打包。

如需生成本地 VSIX 安装包：

```bash
npm install
npm run compile    # 生成清单 + 类型检查 + lint + 打包
npm run test       # 行为测试套件
npm run verify     # 文档、样式作用域、启动链、依赖变更
vsce package       # 扩展安装包
```

生成的安装包不包含大型 Windows 工具；这些工具会按上文说明在首次使用时自动下载。

---

## About Quectel

**移远通信（Quectel）** 是全球领先的物联网解决方案供应商。本扩展是移远开发者工具链的一部分，旨在让基于移远模组及更广泛平台的嵌入式开发更加友好与顺畅。

- **移远官网**：[https://www.quectel.com.cn/](https://www.quectel.com.cn/)
- **移远开发者社区**：[https://forumschinese.quectel.com/](https://forumschinese.quectel.com/)
- **UniKnect SDK**：[https://github.com/quectel-develop/uniknect-c](https://github.com/quectel-develop/uniknect-c)

## 许可证

Apache 2.0 —— 详见 <a href="LICENSE">LICENSE</a>。

---

**祝你在 Quectel RTOS 上开发愉快！**
