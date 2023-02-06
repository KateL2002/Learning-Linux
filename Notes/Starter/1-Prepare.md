# 准备 Linux 环境

在开始之前，首先你必须要搭建 Linux 使用环境，在此节中，您可以通过以下方式来进行搭建！

### <a name="m1">Method 1: 双系统安装</a>

#### Step 1: 准备

- 硬件
  - 一个大约 16GB 及以上的 U 盘（USB2.0 及以上版本）
- 软件
  - Linux 镜像文件（一般为 ISO 文件）
  - U 盘写入软件（Ventoy、Rufus）

#### Step 2: 磁盘分区

在安装 Linux 系统之前，若要将 Linux 系统安装在本机，请提前做好硬盘分区。

若当前使用的是 Windows 系统，这里建议使用 Windows 磁盘管理进行分区，如下图所例：

<img src="../images/parted_window.png"  />

**这里仅需留下一个空分区（至少 16G 及以上），此分区用于安装 Linux 系统**

> 💡 **提示**
>
> 空分区用于挂载根 `/` ；EFI 分区无需格式化，在安装系统时，仅需挂载 `/boot/efi` 即可。

#### Step 3: 下载 Linux 安装镜像

打开对应的官网下载页面。根据自己设备的硬件架构来安装，比如下载安装 x86_64 的镜像文件。

关于 Linux 发行版，请跳转至 [介绍 Linux - Linux 发行版](../About-Linux.md) 一节

> 💡 **提示**
>
> 现在，大多数电脑（包括笔记本电脑）都已使用 x86_64 架构的处理器运行，
> **而 Macbook（M1/M2/...）、手机、平板电脑以 ARM64 架构的处理器运行。**

#### Step 4: 下载 U 盘写入软件（Ventoy）

1）首先，点击进入 [Ventoy 官网](https://www.ventoy.net/cn/index.html) 或者 [Github 下载页](https://github.com/ventoy/Ventoy/releases) 进行下载。

2）其次，插入 U 盘，并打开 Ventoy，具体操作如下：

1. 在上方菜单栏点击【配置选项】--> 勾选 【安全启动支持】

2. 点选【安装】，等待即可

   <img src="../images/ventoy.png" alt="ventoy"  />

>💡 **提示**
>
>若您的 U 盘之前安装过 Ventoy 系统，请直接点击【升级】即可。
>
>**注：此操作不会丢失任何文件**

3）完成之后，**请将下载好的镜像文件存入 Ventoy 盘中即可**

> 📚 **扩展**
>
> Ventoy 是一个制作可启动U盘的开源工具。它有以下几个优势：
>
> 1）无需反复地格式化U盘，你只需要把 ISO/WIM/IMG/VHD(x)/EFI 等类型的文件直接拷贝到U盘里面就可以启动了，无需其他操作。你可以一次性拷贝很多个不同类型的镜像文件，Ventoy 会在启动时显示一个菜单来供你进行选择。如下图：
>
> 
>
> ![](../images/ventoy_start.png)
>
> 
>
> 2）同一个 U 盘可以同时支持 x86 Legacy BIOS、IA32 UEFI、x86_64 UEFI、ARM64 UEFI 和 MIPS64EL UEFI 模式，同时还不影响U盘的日常使用。
>
> 3）Ventoy 支持大部分常见类型的操作系统 （Windows/WinPE/Linux/ChromeOS/Unix/VMware/Xen ...）
>
> 关于 Ventoy 的详细用法，请[点击此处](https://www.ventoy.net/cn/doc_start.html)查看



### <a name="m2">Method 2: 虚拟机安装</a>

#### Step 1: 下载虚拟机

此处列举以下软件，请根据自己的需求，选择适合的虚拟机软件即可。

- [Oracle VM VirtualBox](https://www.virtualbox.org/)

- [VMware Workstation Pro](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)

#### Step 2: 下载 Linux 安装镜像

打开对应的官网下载页面。根据自己设备的硬件架构来安装，比如下载安装 x86_64 的镜像文件。

关于 Linux 发行版，请跳转至 [介绍 Linux - Linux 发行版](../About-Linux.md) 一节

#### Step 3：创建虚拟机





### <a name="m3">Method 3: 搭建并使用 WSL 环境</a>

> 📚 **扩展**
>
> WSL（Windows Subsystem Linux）是适用于 Linux 的 Windows 子系统。它可让开发人员按原样运行 GNU/Linux 环境 - 包括大多数命令行工具、实用工具和应用程序 - 且不会产生传统虚拟机或双启动设置开销。
>
> 
>
> 关于安装 WSL，请访问下方链接：
>
> https://learn.microsoft.com/zh-cn/windows/wsl/install



