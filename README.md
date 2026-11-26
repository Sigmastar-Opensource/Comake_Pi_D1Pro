# Comake Pi D1Pro 快速上手指南

## 1. 平台简介

### 1.1 Comake

[Comake](https://www.comake.online) 是一站式端边侧 AI 开发服务平台，提供从选型到量产的全链路支持。"Comake Pi" 是 Comake 自研的物理 AI 开发板系列，覆盖从轻智能 AIoT（1T 算力）到视频边缘计算（8T 算力）的场景需求。

### 1.2 Git Web

[Git Web](https://git.sigmastar.com.cn:9090/user/login) 是 Sigmastar 对外提供的代码托管平台。

**平台定位**：

- **代码分发**：对外发布 SDK、示例代码、工具等，供用户下载使用
- **议题反馈**：用户可通过议题功能提交问题、建议或技术咨询

**账号体系**：

[Git Web](https://git.sigmastar.com.cn:9090/user/login) 平台的账号与 Sigmastar Comake 社区（`https://www.comake.online`）打通，用户需先注册 Comake 社区账号后方可登录 Git Web。

> [!NOTE]
> **注意**：本平台为对外只读平台，**不支持用户创建个人仓库或直接提交代码**。

### 1.3 Comake Pi D1Pro

Comake Pi D1Pro 是一块基于 SigmaStar SSD2366G 主控芯片的开发板。其主控采用 4 核 ARM Cortex-A53 + RISC-V 协处理器，板载 8Gbit DDR4-3200 内存与 64GB eMMC 5.1。以下是具体的硬件规格描述。

#### 1.3.1 开发板接口模块示意图

![Comake Pi D1Pro 开发板接口模块示意图](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/demoboard_front.png)

#### 1.3.3 接口概览

| 接口（模块） | 名称 | 规格 | 用途（外设） |
| --- | --- | --- | --- |
| SSD2366G | 主控 SoC | 4 核 ARM Cortex-A53 + RISC-V 协处理器，Core/CPU 电压可动态调节 | 为设备提供核心计算与控制能力 |
| DDR4 | 内存 | 8Gbit DDR4-3200（Nanya NT5AD512M16C4-JR） | 为 SSD2366G 主控提供高速运行内存 |
| eMMC | 存储 | 64GB eMMC 5.1（8bit HS400，1.8V） | 板载系统与数据存储，支持 eMMC 启动 |
| CON1 DC 12V | 电源接口 | DC 12V 直流电源输入 | 为开发板提供直流电源输入（DC 12V 电源适配器） |
| SW1 | DC 电源开关 | 船型开关 | 通断适配器输入；整板各路电源还需按 S5（POC）才会打开 |
| S5 POC | POC 开机按键 | 接 RTC_IO3 | 短按开机；长按约 8 秒强制关机 |
| S6 RST | 复位按键 | 接 PM_RESET | 经 R125/R129 复位 SoC |
| S7 | 启动选择拨码开关 | 6 位拨码开关 | 选择启动/升级方式；eMMC 8bit 启动第 4 位 ON（M[3:0]=1010），USB 强烧第 3、4 位 ON（M[3:0]=1000） |
| CON8 ARM | CPU Debug UART 接口 | ARM 调试串口，TTL 电平，波特率 1500000 | 开发板标准调试串口，用于底层固件烧录、调试、日志记录（SGS Debug Tool 小板、Windows PC） |
| CON9 RISC-V | RISC-V Debug UART 接口 | RISC-V 调试串口 | RISC-V 核调试与日志输出 |
| J3 JTAG | JTAG 接口 | ARM EJ JTAG | ARM 核调试 |
| CONG1 | 千兆网络接口 | RJ45 接口，支持 10 / 100 / 1000Mbps 自适应 | 连接网络，支持网络通信与 TFTP 升级（网线） |
| CONR1 | 百兆网络接口 | RJ45 接口 | 连接网络，支持百兆网络通信（网线） |
| CONU1 | USB2.0 Type-C 接口 | Type-C 16P，接 SoC USB2.0 P0 | Device 模式烧录固件，或 Host 模式接 U 盘（USB 数据线、U 盘） |
| CONU2 | USB3.0 Type-C 接口 | Type-C 24P，接 USB2.0 P1 和 USB3.0 超速对 | 连接高速 USB 外设（USB 数据线） |
| CONP1 PCIe | PCIe 接口 | PCIe x4 物理插槽（实际 x1） | 连接 PCIe 设备，扩展高速外设（PCIe 扩展卡） |
| M5 | 80P 座 | 80Pin 扩展座，引 MIPI TX、PM_I2C、复位和 1V8 / 3V3 / 5V_PERI | 预留给 MIPI 转 HDMI 子板 |
| M6 HSPB | 扩展座 | 40P 扩展座，引 HSPB（GPIOG_00~25），1.8V 电平 | 引出 GPIO 等信号用于扩展外设（扩展模块） |
| M7 HSADB | 扩展座 | 40P 扩展座，引 HSADB（GPIOF_01~20），3.3V 电平 | 引出 GPIO 等信号用于扩展外设（扩展模块） |
| CON4 MIPI | MIPI 屏接口 | 40Pin FPC（0.5mm 上接） | 连接 MIPI 屏输出画面（FPC 排线、MIPI 屏） |
| CON5 | 触摸屏接口 | 6Pin FPC 触摸屏接口 | 连接触摸屏，实现触摸输入与界面交互（FPC 排线、触摸屏） |
| J1 MIPI Sensor | MIPI Sensor 接口 | 36Pin FPC（0.5mm 下接），支持 MIPI RX 2lane | 连接摄像头采集图像（FPC 排线、摄像头） |
| MIC1 | 麦克风接口 | 2.0mm 2P 模拟麦克风接口 | 采集音频信号输入系统（麦克风） |
| SPK_R1 | 喇叭接口 | 2.0mm 2P 喇叭接口，建议接 4Ω，经 CS8138S（UA1）功放 | 输出音频，驱动喇叭播放（喇叭） |
| CON10~CON13 | CAN 接口 | 4 路隔离 CAN，TPT71044 收发器，3P 端子：1=H、2=L、3=GNDI | 工业 CAN 总线通信 |
| CON14/CON15 | RS485 接口 | 2 路隔离 RS485，TPT7481 收发器，3P 端子：1=A、2=B、3=GNDI，收发方向由 RTS 控制 | 工业 RS485 总线通信 |
| CON19 RS232 | RS232 接口 | DB9 公头：2=RXD、3=TXD、5=GND，经 TPT3232 转换，非隔离 | 串口通信 |
| CON16~CON18 | DAC 输出接口 | DAC8760（16bit），经 TPT7641 隔离；CON16 为 4-20mA 电流输出，CON17 为 HART，CON18 为电压输出 | 模拟量输出（工业控制） |
| JP1/JP3 GND | 测试排针 | GND 测试点 | 测试与调试 |

---

## 2. 准备工作

### 2.1 安装 Docker

**Step 1: 安装依赖**

    sudo apt-get update && sudo apt-get install -y qemu-user-static binfmt-support docker.io

**Step 2: 验证 aarch64 跨架构支持**

    update-binfmts --display | grep "aarch64"

预期输出：

    qemu-aarch64 (enabled):
     interpreter = /usr/libexec/qemu-binfmt/aarch64-binfmt-P

若未启用，执行：

    sudo update-binfmts --enable

**Step 3: 配置 Docker 用户权限（root 用户可跳过）**

将当前用户加入 `docker` 用户组，避免每次执行 `docker` 命令都需要 `sudo`：

    sudo usermod -aG docker $USER

> **注意**：执行上述命令后，需重新登录或执行 `newgrp docker` 使配置生效。

### 2.2 获取 Git 下载凭证

在 [Comake 社区](https://www.comake.online)注册账号后，访问 [Comake 用户中心](https://www.comake.online/account)，记录以下信息以便进行 Git 认证：

- 邮箱：注册 Comake 账号时用到的邮箱

    ![登记邮箱](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/login.png)

- Token：在 “HTTP 凭据” 界面点击 “获取下载凭证” 按钮生成

    ![获取下载凭证](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/git-key.png)

### 2.3 配置 Git 环境

1. 将 Git 下载凭证写入本地 `.netrc` 文件（让 Git 自动使用凭据进行认证，无需每次手动输入用户名和密码）：

        echo "machine git.sigmastar.com.cn login <邮箱> password <Token>" >> ~/.netrc
        chmod 600 ~/.netrc

2. 安装 `repo` 多仓库管理工具（下载脚本会通过它拉取 SDK 源码）：

        sudo wget https://mirrors.tuna.tsinghua.edu.cn/git/git-repo -O /usr/bin/repo
        sudo chmod a+x /usr/bin/repo

3. 设置 Git 全局用户名和邮箱：

        git config --global user.name "你的用户名"
        git config --global user.email "你的邮箱"

---

## 3. 下载 SDK

### 3.1 克隆下载脚本

```bash
git clone https://git.sigmastar.com.cn:9090/sigmastar/download_scripts.git
cd download_scripts
```

### 3.3 Linux SDK

Linux SDK 提供精简的 64-bit Linux 系统（无桌面），适合产品化部署、资源受限或不需要图形界面的场景。Linux SDK 还支持直接下载预编译的固件镜像，无需本地编译。

**快速上手：**

```bash
# 一键下载所有资源（Docker 镜像、工具链、SDK、工具集等）
bash D1Pro_linux_setup.sh all
```

执行 `all` 后的目录结构：

```
.
├── D1Pro_linux_setup.sh
├── docker_versions.yaml
├── docs/                # 文档
├── hw_ref_design/       # 硬件参考设计
├── image/               # 预编译固件镜像（images.tar.gz）
├── sgs_model_zoo/       # 算法模型库
├── SourceCode/          # SDK 源码（详见第 4 章）
│   ├── ipl/
│   ├── boot/
│   ├── tf-a/
│   ├── optee/
│   ├── kernel/
│   ├── sdk/
│   ├── project/
│   ├── rtos/
│   └── riscv/
└── tools/               # SDK 工具集
    ├── BWLATool/
    ├── CalibrationTool/
    ├── FlashTool/
    ├── GenScalerTbl/
    ├── MakeBin/
    ├── PQTool/
    ├── SystemTool/
    ├── ToolChain/        # 交叉编译工具链
    ├── UsbDevelopTool/
    └── ...
```

**命令速查表：**

| 命令 | 说明 |
|------|------|
| `bash D1Pro_linux_setup.sh list_version` | 查看可用发布版本号 |
| `bash D1Pro_linux_setup.sh all` | 一键下载最新发布版本全部资源 |
| `bash D1Pro_linux_setup.sh all <version>` | 一键下载指定发布版本全部资源 |
| `bash D1Pro_linux_setup.sh docker` | 下载最新发布版本 Docker 镜像 |
| `bash D1Pro_linux_setup.sh docker <version>` | 下载指定发布版本 Docker 镜像 |
| `bash D1Pro_linux_setup.sh sdk_toolchains` | 下载交叉编译工具链 |
| `bash D1Pro_linux_setup.sh sdk` | 下载最新发布版本 SDK 源码 |
| `bash D1Pro_linux_setup.sh sdk <version>` | 下载指定发布版本 SDK 源码 |
| `bash D1Pro_linux_setup.sh image` | 下载最新发布版本烧录固件 |
| `bash D1Pro_linux_setup.sh image <version>` | 下载指定发布版本烧录固件 |
| `bash D1Pro_linux_setup.sh list_tools` | 查看可用工具列表 |
| `bash D1Pro_linux_setup.sh tools` | 下载全部工具 |
| `bash D1Pro_linux_setup.sh tools <tool_name>` | 下载单个工具 |
| `bash D1Pro_linux_setup.sh model_zoo` | 下载最新发布版本算法模型库 |
| `bash D1Pro_linux_setup.sh model_zoo <version>` | 下载指定发布版本算法模型库 |
| `bash D1Pro_linux_setup.sh docs` | 下载最新发布版本文档 |
| `bash D1Pro_linux_setup.sh docs <version>` | 下载指定发布版本文档 |
| `bash D1Pro_linux_setup.sh hw_ref_design` | 下载硬件参考设计资料 |
| `bash D1Pro_linux_setup.sh build_image` | 使用对应 SDK 发布版本的 Docker 镜像编译系统镜像 |
| `bash D1Pro_linux_setup.sh build_image <version>` | 使用指定发布版本的 Docker 镜像编译系统镜像 |

**使用 `latest` 参数：**

将 `latest` 作为 `<version>` 传入，可获取开发中的最新代码，而非固定的发布版本：

| 命令 | `latest` 行为 |
|------|--------------|
| `bash D1Pro_linux_setup.sh all latest` | 一键下载最新全部资源 |
| `bash D1Pro_linux_setup.sh docker latest` | 下载最新 Docker 镜像 |
| `bash D1Pro_linux_setup.sh sdk latest` | 下载最新 SDK 源码 |
| `bash D1Pro_linux_setup.sh image latest` | 下载最新发布版本烧录固件（与不传 `<version>` 等效） |
| `bash D1Pro_linux_setup.sh model_zoo latest` | 下载最新算法模型库 |
| `bash D1Pro_linux_setup.sh docs latest` | 下载最新发布版本文档（与不传 `<version>` 等效） |
| `bash D1Pro_linux_setup.sh build_image latest` | 使用最新 Docker 镜像编译系统镜像 |

---

## 4. SDK 目录结构

下载完成后的 `SourceCode/` 目录结构如下：

```
SourceCode/
├── ipl/                 # IPL 第一阶段引导程序
│   ├── arch/            #   各芯片的启动代码
│   ├── common/          #   通用功能代码
│   ├── configs/         #   各项目配置文件
│   ├── drivers/         #   外设驱动
│   └── include/         #   头文件
│
├── boot/                # U-Boot 引导程序
│   ├── arch/            #   架构相关代码（ARM）
│   ├── board/           #   板级支持（SigmaStar 平台）
│   ├── cmd/             #   U-Boot 命令实现（含 estar 系统升级命令）
│   ├── configs/         #   板级默认配置文件（pceres_comake_d1pro_emmc_arm64_defconfig）
│   ├── drivers/         #   外设驱动（USB、MMC、NET 等）
│   ├── dts/             #   设备树源文件
│   ├── include/         #   头文件
│   ├── tools/           #   辅助工具
│   └── Makefile         #   U-Boot 编译入口
│
├── tf-a/                # ARM Trusted Firmware，安全启动固件
│   ├── bl1/bl2/bl31/bl32/  #   各启动阶段固件
│   ├── common/          #   通用代码
│   ├── drivers/         #   驱动
│   ├── fdts/            #   设备树
│   └── include/         #   头文件
│
├── optee/               # OP-TEE 安全执行环境
│   ├── optee_os/        #   可信 OS 内核
│   ├── optee_client/    #   客户端库
│   ├── optee_examples/  #   示例 TA 应用
│   └── optee_test/      #   测试套件
│
├── kernel/              # Linux 内核
│   ├── arch/arm64/      #   ARM64 架构代码（dts/sgs/pceres-comake-d1pro.dts）
│   ├── drivers/         #   内核驱动
│   ├── include/         #   内核头文件
│   ├── net/             #   网络协议栈
│   ├── sound/           #   音频子系统
│   └── Makefile         #   内核编译入口
│
├── sdk/                 # 核心媒体 SDK
│   ├── driver/          #   多媒体驱动与 SensorDriver
│   ├── linux/           #   用户空间库（MI SDK 的 Linux 相关代码）
│   └── verify/          #   示例代码与测试程序
│       ├── common/          #   sample_code 公共组件
│       ├── prebuild_libs/   #   部分 sample_code 所需第三方库
│       ├── sample_code/     #   demo 代码
│       └── release_feature/ #   release 特性 demo（dla 等）
│
├── project/             # 编译工程入口
│   ├── board/           #   板级配置（pceres）
│   ├── configs/         #   defconfig 配置文件
│   ├── image/           #   镜像打包脚本与输出
│   ├── kbuild/          #   内核编译集成
│   ├── release/         #   发布输出（SDK 库与头文件）
│   ├── scripts/         #   工程脚本
│   ├── tools/           #   调试工具（iqtool 等）
│   └── makefile         #   顶层 Makefile（编译入口）
│
├── rtos/                # RTOS 系统源码（RISC-V 核 normal 模式）
│   └── proj/            #   工程（sc：application/driver/middleware/system）
│
└── riscv/               # RISC-V 核 TCM 模式系统源码
    └── pm_riscv/        #   电源管理固件（make/doc/source）
```

---

## 5. 编译固件

下载完成后，使用脚本一键编译：

### 5.2 Linux SDK

```bash
# 编译 64-bit Linux 系统镜像
bash D1Pro_linux_setup.sh build_image
```

其中 `SourceCode/project/image/output/images` 目录生成的 `SgsUsbImage_xxxx(date).bin` 为 USB 升级包，制作与升级方法详见[固件升级](#6-固件升级)。

---

## 6. 固件升级

### 6.1 硬件接线

![开发板硬件接线示意图](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/Peripheral.jpg)

1. 开发板 CON1 DC 12V 电源接口接入 DC 12V 电源适配器。
2. 将 USB 数据线一端接入开发板 CONU1 USB2.0 Type-C 接口，另一端接入 PC（CONU1 对应 USB2.0 P0，支持 Device 模式烧录）。
3. 将 MIPI 屏 FPC 排线接入开发板 CON4 MIPI 屏接口。
4. 开发板 CONG1 千兆网络接口接入网线，确保开发板与 PC 连接至同一网段。

### 6.2 串口接线

串口是嵌入式开发的基础调试手段，可在无网络或系统未启动时使用。以 MobaXterm 为例：

1. 开发板 CON8 CPU Debug UART 接口连接 SGS Debug Tool 小板，小板 Type-A USB 口连接至 Windows。

    ![串口接线示意图](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/connection.jpg)

2. 打开 MobaXterm 工具，依次点击 Session、Serial，Serial Port 根据设备管理器选择（如 COM5），Speed (bps) 选择 1500000，最后点击 OK 连接串口。

    ![MobaXterm 串口配置](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/mobaxterm.png)

> **注意**：D1Pro 调试串口波特率为 1500000，需按实际值选择；上图为工具配置示意。

### 6.3 进入 USB 升级模式

根据 eMMC 当前状态，选择对应方式让开发板进入 USB 升级模式：

- **空片升级**：eMMC 处于出厂空白状态，或 U-Boot 等固件已被完全擦除。默认处于 USB 升级模式，直接将 USB 线接入 CONU1 USB2.0 Type-C 接口（空片升级仅支持 port0），PC 即可识别到 USB 存储设备。

- **非空片升级**：eMMC 上已有可运行的 U-Boot。

    1. 打开串口调试，使用 SGS Debug Tool 小板连接开发板 CON8 CPU Debug UART 接口（波特率 1500000）。
    2. 开发板上电，进入 U-Boot，输入 `ufu command` 进入 USB 升级模式。

- **强制升级**：将 S7 启动选择拨码开关第 3、4 位拨至 ON（M[3:0]=1000），开发板上电后强制进入 USB 强烧模式，可用于异常状态下的强制升级恢复。升级完成后需将 S7 第 4 位恢复为 ON（M[3:0]=1010），断电重启后才可正常使用。

### 6.4 整包固件升级

USB 升级包可通过 SDK 中的 `make image-usb-factory` 或脚本 `./image/makefiletools/script/make_usb_factory_sgs.sh -f` 生成（在 `SourceCode/project` 目录下执行），输出在 `SourceCode/project/image/output/images/SgsUsbImage_xxxx(date).bin`。

打开 “USB Factory Tool.exe”，选择 `SgsUsbImage_xxxx(date).bin`，待已连接设备全部识别后，点击 “start” 开始整包升级。

![整包固件升级](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/usb-5.png)

### 6.5 单分区升级

1. 制作升级包时不带 `-f` 参数（或执行脚本时在 `Full or Optional Upgrade ? (Y/N)` 提示中输入 N），按提示选择需要升级的分区。

2. 打开 “USB Factory Tool.exe”，选择该升级包，点击 “start” 升级所选分区。

3. 若需在多设备场景下只升级指定设备，将升级模式切换为 **Manual**，选中设备后再点击 “start”。

    ![单分区升级](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/usb-7.png)

### 6.6 验证

**Linux SDK：** 升级完成后，通过串口终端（波特率 1500000）连接开发板，能正常进入终端即可。

> 至此，你已经成功跑通了 Comake Pi D1Pro 的完整开发流程。

---

## 7. Git Web 平台使用指南

### 7.1 登录 Git Web

1. 打开浏览器，访问 [Git Web](https://git.sigmastar.com.cn:9090/user/login)

2. 输入登录凭据

    ![Git Web 登录](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/gitweb-login.png)

    - **用户名**：Comake 社区注册邮箱
    - **密码**：Comake 社区账户密码

3. 点击「登录」按钮

> 如登录失败，请确认 Comake 社区账号已激活，且用户名和密码输入正确。

### 7.2 浏览仓库

登录后，点击顶部导航栏「探索」即可查看所有对外公开的代码仓库。你也可以通过搜索栏直接搜索仓库名称。

在仓库页面可以查看：

- 文件目录与内容
- 提交历史
- 版本标签（Tags）与发布（Releases）
- 分支列表

### 7.3 克隆仓库

在仓库页面，点击「复制网址」按钮，复制 HTTPS 地址：

```bash
git clone https://git.sigmastar.com.cn:9090/<namespace>/<repo-name>.git
```

执行后输入登录凭据：

- **用户名**：Comake 社区注册邮箱
- **密码**：Comake 社区账户密码

### 7.4 下载压缩包

如果不想使用 Git 命令行，可直接下载代码压缩包：进入仓库页面，点击「...」→ 选择 下载ZIP 或 下载TAR.GZ。

![下载源码压缩包](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/gitweb-download.png)

### 7.5 更新本地代码

当平台上的仓库有更新时，在本地仓库目录执行：

```bash
git pull
```

### 7.6 提交反馈（议题）

议题是你向 Sigmastar 技术团队反馈问题的渠道。你可以通过议题提交：

- **Bug 报告**：代码或文档中的错误
- **功能建议**：希望新增或改进的功能
- **技术咨询**：使用过程中遇到的技术问题

#### 7.6.1 创建议题

1. 登录后进入目标仓库
2. 点击顶部「议题」选项卡
3. 点击「创建议题」

    ![创建议题](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/gitweb-issue-1.png)

4. 填写议题

    - **标题**：简要概述问题（如 "XXX 示例在 Linux 环境编译失败"）
    - **描述**：详细说明问题（参考下方模板）

5. 点击「创建议题」确认按钮

    ![填写议题](https://cdn.jsdelivr.net/gh/Sigmastar-Opensource/Comake_Pi_D1Pro@main/mymedia/gitweb-issue-2.png)

#### 7.6.2 议题描述模板

建议按照以下模板填写，以便技术团队快速定位和处理：

```markdown
### 问题类型
<!-- 请选择：Bug 报告 / 功能建议 / 技术咨询 -->

### 环境信息
- 操作系统：
- 代码版本 / Tag：
- 工具链版本（如适用）：

### 问题描述
<!-- 请详细描述遇到的问题 -->

### 复现步骤（Bug 类必填）
1.
2.
3.

### 期望行为
<!-- 描述你期望的正确行为 -->

### 实际行为
<!-- 描述实际观察到的行为 -->

### 附加信息
<!-- 可附上日志、截图、命令行输出等 -->
```

#### 7.6.3 关注议题进度

提交议题后，你可以：

- 在议题页面与 Sigmastar 技术人员留言沟通
- 当议题被标记为「已关闭」时，表示问题已处理完成
