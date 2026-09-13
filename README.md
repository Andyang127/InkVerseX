# 墨语 X (InkVerse X) 插件套件说明与使用手册

> **面向工程设计、规划测绘与施工深化出图的 AutoCAD 工业级插件矩阵**  
> *基于 C# 托管程序集核心与 AutoLISP/VLX 实用算法库的双引擎架构，100% AutoCAD 原生实体，无任何第三方代理对象。*


| 核心属性项     | 配置规范与支持指标                                                                              | 技术备注                                        |
|:--------------------|:---------------------------------------------------------------------------------------------------------|:----------------------------------------------------|
| **软件主版本** | `v0.1.4` (重磅集成 CAD Auto BatchPrint 批量打印切图，全系自适应双主题)                | 工业级稳定发布版                            |
| **支持平台**    | AutoCAD 2007 ~ 2019 (32/64 位双架构兼容) <br> AutoCAD 2020 ~ 2027 (原生 64 位体系)    | 跨越 10 个主版本世代 (R17~R26)              |
| **运行框架**    | .NET Framework 3.5 ~ 4.8 (经典运行时) <br> .NET 8.0 ~ 10.0 (现代 CoreCLR 独立运行时) | 多目标运行时自适应感知加载             |
| **底层架构**    | C# 托管程序集核心驱动 + AutoLISP / VLX 扩展算法库                                          | 双引擎解耦协同架构                         |
| **图元规范**    | 100% AutoCAD 原生实体 (`Block`, `Polyline`, `MText`, `Table`)                                        | 零自定义代理实体，脱离插件完全可读 |


---

## 安装包下载与使用说明

<div style="background-color: #FEF2F2; border: 1px solid #FCA5A5; border-left: 5px solid #DC2626; padding: 14px 18px; border-radius: 6px; margin: 16px 0; color: #991B1B; line-height: 1.6;"><strong style="color: #991B1B; font-size: 15px;">特别说明：关于暂未开放全部工程源码的说明</strong><br>墨语 X (InkVerse X) 是作者业余独立维护的开源探索项目，本仓库当前主要发布编译测试就绪的安装包与技术文档。暂未开放全部完整工程源码主要基于以下阶段性安排，望同行理解：<br><strong>1. 代码重构与环境解耦：</strong>早期工程包含本地测试桩与绝对路径，作者正利用业余时间系统性清洗排版；<br><strong>2. 跨 10 世代编译依赖极重：</strong>深度适配 AutoCAD 2007 ~ 2027（R17~R26 多运行时）需配置数十 GB 的 ObjectARX SDK 与运行库。为避免因本地环境缺失产生大量编译报错，现阶段优先分发全数测试通过的安装包，确保开箱即用；<br><strong>3. 防范恶意篡改与倒卖牟利：</strong>防范黑灰产抹去作者署名或植入广告弹窗倒卖，保障一线图纸环境纯净与系统安全；<br><strong>4. 后续分批逐步开源：</strong>将按<strong>“通用几何算法 -> 独立轻量模块（输入法/图层） -> 完整工程框架”</strong>成熟一个推送一个。欢迎在 Issues 提出宝贵建议与反馈！</div>

- **最新安装包发布页面**：[前往 GitHub Releases 页面下载最新安装包](https://github.com/Andyang127/InkVerseX/releases/latest)
- **完整图文交互使用手册**：
  - **在线直接交互浏览**：[点击在浏览器中打开在线交互手册 (GitHub Pages)](https://andyang127.github.io/InkVerseX/Readme.html) 或 [HTML 在线镜像预览](https://raw.githack.com/Andyang127/InkVerseX/main/Readme.html)
  - **本地离线交互浏览**：克隆/下载至本地后直接双击同级 Readme.html 打开，或在 AutoCAD 命令行输入 <kbd>INKHELP</kbd> 自动唤起

- **套件一键安装部署**：
  1. 从 Release 页面下载 `InkVerseX_Setup_v0.1.4.exe`；
  2. 退出当前运行的 AutoCAD 进程，双击运行安装向导；
  3. 根据向导提示选择需要挂载的 AutoCAD 版本（支持 AutoCAD 2007 ~ 2027 多版本共存并独立勾选）；
  4. 点击“安装”直至完成。

- **独立功能模块单独安装**：
  - 若仅需使用单一功能（例如仅需图层管理或仅需批量打印），可直接使用各子分支工程生成的独立安装包（如 `CadAutoBpFrame_Setup_v0.1.0.exe`、`CadAutoBatchPrint_Setup_v0.1.0.exe`、`CadAutoIme_Setup_v0.4.3.exe` 等）；
  - 每个独立插件均配有专属安装向导，自动配置独立启动注册表，各模块互不依赖干扰。
- **自动挂载与启动生效**：
- 安装向导将自动写入当前用户的 AutoCAD 注册表启动配置（`LOADCTRLS = 2`, `MANAGED = 1`），并自动添加安装路径到受信任位置 (`TRUSTEDPATHS`)；
- 安装完毕后启动 AutoCAD，系统将自动加载核心程序集，并生成顶部经典下拉菜单 (`墨语 X(&X)`)、功能区 Ribbon 选项卡与经典快捷工具栏。

---

## 1. 项目概述

墨语 X (InkVerse X) 是一套面向工程设计、建筑施工、市政规划与地理测绘领域的 AutoCAD 辅助制图增强插件套件。

本套件采用模块化分层解耦架构：

1. **宿主引导装配程序 (`InkVerseX.dll`)**：在 AutoCAD 启动时自动初始化，负责依赖程序集解析 (`AssemblyResolve`)、按需动态扫描加载各业务子插件，并统一构建维护顶部经典下拉菜单、功能区 (Ribbon) 选项卡与快捷工具栏；
2. **8 个 C# 原生独立功能模块**：各模块在物理上为独立子工程，具备独立编译与单独运行能力。v0.1.4 版本正式将 **CAD Auto BatchPrint (批量打印与切图发布)** 纳入核心套件，形成涵盖智能图框排版、图纸批量打印与切图、输入法智能感知、图层点选控制、表格数据提取、视口截图导出、图纸数据库深度清理与 LISP 集中管控的完备 C# 原生插件矩阵；
3. **6 个 AutoLISP / VLX 实用扩展工具**：覆盖坐标高程标注、编号速写、线长测量、文本数值替换、线元清洗转换与几何图形重构。


> [!NOTE]
> **原生图元安全准则**：所有模块均基于 AutoCAD 原生 .NET API 与 ObjectARX 接口进行构建，图纸中生成的所有图元均为 AutoCAD 原生对象 (`BlockReference`, `Polyline`, `DBText`, `MText`, `Table`)，不创建任何第三方自定义代理实体 (Proxy Entity)。在未安装本插件的纯净 AutoCAD 环境中打开图纸，所有图元内容均可正常查阅、测量、编辑与打印。

---

## 2. 核心架构与技术特点

```mermaid
graph LR  
    subgraph Host["宿主引导装配层 (InkVerseX.dll)"]  
        AR["程序集解析器<br>AssemblyResolve"]  
        DS["动态插件扫描引擎<br>Dynamic Loader"]  
        UI["统一界面挂载<br>Ribbon / Menu / Toolbar"]  
    end  
  
    subgraph CSharp["8 大 C# 核心原生托管模块"]  
        M1["BP_Frame 智能图框"]  
        M2["BatchPrint 批量打印切图"]  
        M3["Auto IME 输入法感知"]  
        M4["LayerManager 图层控制"]  
        M5["LiveTable 表格提取"]  
        M6["SNAP 视口截图"]  
        M7["SmartClean 数据库清理"]  
        M8["VlxSuite 脚本管理"]  
    end  
  
    subgraph VLX["6 大 AutoLISP / VLX 扩展算法库"]  
        V1["ZBBZ 坐标高程"]  
        V2["BHSX 智能编号"]  
        V3["ZCD 长度统计"]  
        V4["CZTH 文本数值替换"]  
        V5["XZH 线清洗转换"]  
        V6["XZX 图元重构矩阵"]  
    end  
  
    AR --> DS  
    DS --> CSharp  
    DS --> VLX  
    CSharp & VLX --> UI
```

### 2.1 C# + VLX 双引擎协同机制

- **C# 托管内核**：负责处理复杂图形计算几何、空间拓扑索引、WPF 现代自适应界面、系统底层输入法全局钩子与高分辨率光栅化渲染；
- **VLX 实用算法库**：封装经典的 AutoLISP 制图算法，通过内置沙盒机制安全调用，兼顾执行速度与即用即走的灵活性。

### 2.2 集中共享运行资产机制 (Shared Component)

- 采用集中共享资产架构，将非托管 C++ 渲染核心（`pdfium.dll`）、VLX 脚本库与全局高分辨率图标资源统一归置于安装目录下的 `Shared` 文件夹；
- 各 CAD 世代专用目录（R17 ~ R26）仅存放版本专用的托管 DLL，杜绝多世代重复打包造成的磁盘冗余，大幅缩减安装包体积与内存占用。

### 2.3 外部项目源码零副本联动 (MSBuild Shared Source)

- 各独立子工程（如 `CAD Auto IME`、`CAD Auto SNAP`、`CAD Auto BP_Frame` 等）与墨语 X 宿主工程之间，采用 MSBuild 共享源码项目直接链接模式；
- 在保持各子工程完全独立维护与单独发布的前提下，主工程修改代码后可直接跨世代联动编译，无需手动复制 DLL 副本。

### 2.4 全世代自适应双主题系统

- **AutoCAD 2007 ~ 2014**：强制适配浅色白底商务主题，消除旧版本 CAD 界面中深色弹窗带来的突兀感；
- **AutoCAD 2015 ~ 2027**：动态监听 AutoCAD 的 `COLORTHEME` 系统变量，跟随 CAD 宿主界面的明暗主题切换无缝换肤；
- **5px 极窄滚动条统一**：全套 C# 插件控制台与停靠面板统一升级为 5px 极窄圆角滑动条 (`SlimScrollViewer`)，最大化绘图区视口空间。

### 2.5 在线检测升级与版本管理

- 集成基于 GitHub Releases API 的后台异步升级检测引擎；
- 支持自适应深浅主题的升级提醒界面，并提供特定版本忽略控制选项。

### 2.6 空间拓扑图框识别与批量虚拟打印引擎 (v0.1.4 新增)

- **多源智能图框探测拓扑**：针对工程总图中图签格式混杂、非标图框繁多的痛点，构建了图块属性模式 (BlockFrame)、图层闭合多段线模式 (LayerFrame) 与抗噪空间网格拓扑采样模式 (SmartGrid) 三重识别体系，支持毫秒级自动捕捉全图图框范围与打印比例；
- **DWG 独立切图与版本降转管线**：提供全自动图元裁剪切分管线，依据图框空间范围执行局部 `WBLOCK` 裁切为独立单张 DWG 文件，支持直接降级保存为天正建筑 T3 (AutoCAD 2004 DWG) 格式，打通成果下游施工与审图分发闭环；
- **虚拟打印与多页 PDF 合并集成**：深度集成非托管 PDF 渲染与虚拟打印队列，支持图幅纸张自适应匹配、图纸目录 (TOC) 双向交叉核验，并提供 <kbd>INKPDFMERGE</kbd> 一键将多份单页 PDF 合并为单份完整项目工程图册。

---

## 3. 版本更新日志

### 3.1 当前版本 (v0.1.4)

- **1、新增：CAD Auto BatchPrint 批量打印与切图分拆模块**：

  <div align="center">
    <img src="Resources/update_%20v0.1.4.png" alt="墨语 X 工具箱控制台 v0.1.4 - 批量打印重磅集成" width="380" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 16px rgba(0,0,0,0.15);" />
    <br>
    <sub>墨语 X 工具箱控制台 (v0.1.4) · 批量打印与切图分拆全新入列</sub>
  </div>

  - **三重图框识别引擎**：支持图块属性 (`BlockFrame`)、图层/闭合多段线 (`LayerFrame`) 与抗噪空间网格拓扑 (`SmartGrid`) 三重识别机制，精准自适应标准与非标图框；
  - **DWG 智能裁切与版本降转**：按图框边界将超长总图批量切分为独立单张 DWG 文件，支持自动另存为天正建筑 T3 (AutoCAD 2004 DWG) 通用格式，支持自定义文件命名模板宏 (`{图号}_{图名}_{版本}`)；
  - **批量虚拟打印与一键合并 PDF**：后台静默调用虚拟打印机完成批量出图，自适应纸张规格（A0~A4 横纵方向），支持单页批量出图或通过 <kbd>INKPDFMERGE</kbd> 一键将多页 PDF 拼接为单份项目总图册；
  - **图纸目录 (TOC) 交叉纠错比对**：读取图纸目录表格或文字，与各图框标题栏属性双向模糊交叉比对，自动标记缺失图号、漏图与错别字；
  - **双轨制交互界面**：提供侧边栏快速打印停靠面板 (<kbd>INKPLOT</kbd> / <kbd>SBP</kbd>) 与独立大批量多文件切图发布工作台 (<kbd>INKPLOTDIALOG</kbd> / <kbd>SBPDLG</kbd>)；
  - **全系自适应双主题系统**：全套 C# 插件支持 CAD 2007~2014 浅色白底商务主题与 CAD 2015~2027 动态跟随 `COLORTHEME` 变量无缝热换肤，统一升级 5px 极窄圆角滑动条 (`SlimScrollViewer`)；
- **2、CAD Auto IME 智能输入法感知加固**：
  - **天正单行文字输入法状态处理**：优化输入法决策状态机，从天正单行文字 (<kbd>DHWZ</kbd> / <kbd>TTEXT</kbd>) 切回绘图画布后，主动将输入法复位为英文，减少快捷键操作干扰；
  - **在位文字与表格编辑感知增强**：重构在位多行文字编辑器 (`AcInPlaceEdit`)、表格单元格 (`TABLEDIT`) 与属性块的感知机制，提升唤起中文准确率；
  - **现代输入法驱动加固**：增强 TSF 与 IMM32 双轨管线协调，全面保障 Windows 11 最新系统及微信输入法、搜狗输入法的稳定切换；
- **3、全系 9 大安装包统一打包规范**：统一将 8 个独立插件安装路径规范为 `%APPDATA%\InkVerse\<ModuleName>`，安装包输出至各分支自身目录；综合套件规范为 `%APPDATA%\InkVerseX`；
- **4、增加自动化测试工程 (TEST)**：新增 MSTest 单元测试工程，涵盖 9 大模块共 43 项算法与逻辑用例，构建质量检验 100% 绿灯通过。

### 3.2 历史版本 (v0.1.3)

- **CAD Auto IME 智能输入法深度集成**：正式将智能输入法接入套件，实现视口/命令行空闲保持纯英文、文本录入自动切换中文；
- **外部独立项目零副本源码联动**：重构项目间源码引用链路，打通各独立仓库的开发调试闭环；
- **跨框架全世代兼容性加固**：解决 .NET 8 / .NET 10 与经典 .NET Framework 2.0~4.8 多框架跨版本类型歧义，完成 R17~R26 全世代覆盖。

---

## 4. AutoCAD 与运行时框架兼容表

墨语 X 依据 AutoCAD 各版本的 .NET 运行时基线进行分代编译，支持 AutoCAD 2007 至 2027 全系列版本：


| 世代标识 | 适用 AutoCAD 版本范围 | 架构体系 | 编译目标框架 (Target Framework)     | 界面交互技术 | 兼容验证状态 |
|:-------------|:----------------------------|:------------:|:------------------------------------------|:-------------------|:------------------:|
| **R17**      | AutoCAD 2007 ~ 2009         | 32/64 位    | .NET Framework 3.5 (兼容 2.0 运行时) | WinForms           | 完全兼容       |
| **R18**      | AutoCAD 2010 ~ 2012         | 32/64 位    | .NET Framework 3.5                        | WinForms           | 完全兼容       |
| **R19**      | AutoCAD 2013 ~ 2014         | 32/64 位    | .NET Framework 4.0 / 4.5                  | WinForms / WPF     | 完全兼容       |
| **R20**      | AutoCAD 2015 ~ 2016         | 32/64 位    | .NET Framework 4.5.2                      | WinForms / WPF     | 完全兼容       |
| **R21**      | AutoCAD 2017                | 32/64 位    | .NET Framework 4.6                        | WinForms / WPF     | 完全兼容       |
| **R22**      | AutoCAD 2018                | 64 位       | .NET Framework 4.7                        | WinForms / WPF     | 完全兼容       |
| **R23**      | AutoCAD 2019 ~ 2020         | 64 位       | .NET Framework 4.7.2                      | WinForms / WPF     | 完全兼容       |
| **R24**      | AutoCAD 2021 ~ 2024         | 64 位       | .NET Framework 4.8                        | WinForms / WPF     | 完全兼容       |
| **R25**      | AutoCAD 2025 ~ 2026         | 64 位       | .NET 8.0-windows (现代 CoreCLR)         | Modern WPF         | 完全兼容       |
| **R26**      | AutoCAD 2027+               | 64 位       | .NET 10.0-windows (现代 CoreCLR)        | Modern WPF         | 完全兼容       |


> [!TIP]
> **多版本共存自适应**：当计算机中同时安装了多个不同年份的 AutoCAD 时，安装向导支持独立勾选挂载目标版本。各版本启动时由宿主引导程序自动识别 CAD 核心主版本号，动态路由加载对应世代（R17~R26）的程序集，互不冲突。

---

## 5. 安装路径规范与部署机制

### 5.1 安装路径统一规范

为保证系统环境规范与多插件共存时的可维护性，全套安装包执行以下安装路径标准：

- **墨语 X 综合套件**：统一安装于当前用户数据目录：`%APPDATA%\InkVerseX` (即 `C:\Users\<用户名>\AppData\Roaming\InkVerseX`)；
- **独立功能插件**：统一归整于用户数据目录的子文件夹中：`%APPDATA%\InkVerse\<模块英文名称>` (例如：`%APPDATA%\InkVerse\CadAutoBpFrame`)。

### 5.2 注册表自动挂载机制

通过安装向导部署时，程序将自动完成以下系统配置：

1. **注册表自动加载项**：在当前用户的 AutoCAD 注册表分支 `Software\Autodesk\AutoCAD\<版本代码>\<语言代码>\Applications\InkVerseX` 下写入键值：
   - `LOADCTRLS = 2` (AutoCAD 启动时自动加载)
   - `MANAGED = 1` (托管程序集标识)
   - `LOADER = <安装目录>\<世代>\Plugins\InkVerseX.dll`
2. **受信任位置配置**：自动将插件安装目录及 `Shared` 共享目录添加至 AutoCAD 的安全可信路径列表 (`TRUSTEDPATHS`)，避免启动时弹出安全警告。


### 5.3 手动加载与便携运行

若在未运行安装向导的环境中临时使用，可执行手动加载：

1. 启动对应版本的 AutoCAD；
2. 在命令行输入 <kbd>NETLOAD</kbd> 命令并回车；
3. 浏览至安装目录对应世代文件夹下的 `Plugins\InkVerseX.dll` 并确认；
4. 插件将自动初始化并挂载主菜单与功能区。


### 5.4 完整卸载与清理

通过 Windows 控制面板的“添加或删除程序”执行标准卸载。卸载向导会自动注销注册表中的加载项配置，并清理安装文件。图纸文件本身不包含任何专有格式，不会受到卸载影响。

---

## 6. C# 核心原生模块详细说明

### 6.1 智能图框自适应与排版管理 (CAD Auto BP_Frame)

针对工程图纸出图过程中的图框生成、自适应比例计算、非规则离散图框阵列排齐与标题栏数据集中管理。

- **核心快捷指令**：
  - <kbd>INKBOX</kbd>：呼出智能图框生成向导；
  - <kbd>INKALIGN</kbd>：离散图框矩阵阵列排齐；
  - <kbd>INKFRAMEDATA</kbd>：提取全图图框标题栏属性；
  - <kbd>INKCLONE</kbd>：克隆当前图框配置与视口参数。
- **功能细节**：
- **标准图框与自适应推算**：支持 ISO 标准图框 (A0 ~ A4 横版与竖版) 以及用户自定义加长图幅；提供“框选实体自适应”与“基点定位放置”两种模式。框选图面设计内容后，系统自动计算外接包围盒边界，按常用工程比例（1:50、1:100、1:150、1:200 等）智能推算最佳适配图幅；
- **离散图框网格排齐 (<kbd>INKALIGN</kbd>)**：框选多张散乱图框，设置行间距、列间距与排列方向，快速完成矩阵排齐；
- **标题栏属性集中编辑 (<kbd>INKFRAMEDATA</kbd>)**：自动识别全图图签属性块，在侧边栏表格中集中列出图名、图号、版本、比例、日期等字段，支持双击定位高亮并批量回写图面；
- **视口参数克隆 (<kbd>INKCLONE</kbd>)**：快速将现有图框的图层状态、视口比例复制应用至其他布局。

---

### 6.2 批量打印与切图发布 (CAD Auto BatchPrint) [v0.1.4 新增]

专为工程施工图成果交付场景打造的智能图框识别、DWG 自动切图分拆、PDF 批量虚拟打印与图纸目录交叉比对套件。

```mermaid
graph TD  
A["图面识别请求 (INKPLOT / INKPLOTDIALOG)"] --> B{"图框探测引擎"}  
B -->|"标准属性块"| C["图块探测模式 (Block Frame)"]  
B -->|"图层/闭合多段线"| D["图层几何模式 (Layer Frame)"]  
B -->|"散乱/分解图元"| E["空间网格拓扑模式 (Smart Grid)"]  
C & D & E --> F["图框边界与打印比例自适应匹配"]  
F --> G["图纸目录 (TOC) 交叉比对与纠错 (SBPTOC)"]  
G --> H{"出图任务调度"}  
H -->|"虚拟出图"| I["批量虚拟打印 (PDF / PLT) -> INKPDFMERGE 一键合并"]  
H -->|"图纸切分"| J["按图框 WBLOCK 切分为独立 DWG (可选降级天正 T3)"]
```

- **核心快捷指令**：
  - <kbd>INKPLOT</kbd>：呼出侧边栏快速打印停靠面板 (兼容别名: <kbd>SBP</kbd>, <kbd>SBPLOT</kbd>, <kbd>BP</kbd>)；
  - <kbd>INKPLOTDIALOG</kbd>：打开独立多文件批量处理与切图工作台 (兼容别名: <kbd>SBPDLG</kbd>, <kbd>SBPM</kbd>, <kbd>SBPMWINDOW</kbd>)；
  - <kbd>INKPLOTDETECT</kbd>：在当前视口中毫秒级高亮预览所识别到的图框外框与图幅规格；
  - <kbd>INKPDFMERGE</kbd>：将多份单页 PDF 一键合并为单份完整工程总图册；
  - <kbd>SBPTOC</kbd>：启动图纸目录比对与缺图核验工具 (兼容别名: <kbd>SBTOC</kbd>)。
- **功能细节**：
  - **三重智能图框识别机制**：
    1. **图块探测模式 (Block Frame)**：识别标准属性块，精准捕捉图号、图名、版本、比例等标题栏字段；
    2. **图层几何模式 (Layer Frame)**：依据指定图层及矩形闭合多段线长宽比提取自定义图框；
    3. **空间网格探测模式 (Smart Grid)**：基于空间拓扑网格采样算法，高效过滤干扰杂线，适用于图块被分解 (Explode) 或线层混乱的复杂历史图纸；

- **DWG 独立切图与天正 T3 降转**：自动将模型空间或布局内的各图框裁切为独立的单张 DWG 文件，支持自动另存为天正建筑 T3 (AutoCAD 2004 DWG) 格式，支持 `{图号}_{图名}_{版本}_{出图时间}` 变量宏自动命名；
- **PDF 虚拟打印与一键合并**：后台静默调用打印驱动批量出图，自适应纸张规格（A0~A4 横纵方向），自动计算中心偏移与旋转角度；通过 <kbd>INKPDFMERGE</kbd> 可一键将多张单页 PDF 拼接为单份项目图册；
- **图纸目录 (TOC) 交叉比对与纠错**：框选图纸目录表格或文字，与图框标题栏属性进行双向模糊匹配，自动纠正错别字并高亮标记缺图与漏号；
- **双轨制交互界面**：提供适合单图即时出图的侧边停靠栏 (<kbd>INKPLOT</kbd>) 与适合多文件批量队列发布的主向导工作台 (<kbd>INKPLOTDIALOG</kbd>)。

---

### 6.3 智能中英输入法状态管理 (CAD Auto IME)

解决 AutoCAD 绘图时输入快捷键容易被中文输入法候选框打断的问题，进行上下文感知的自动化中英文切换。

- **核心快捷指令**：
  - <kbd>TOGGLEAUTOIME</kbd>：开启或关闭输入法自动切换功能 (兼容别名: <kbd>OpenCadIme</kbd>, <kbd>InkIme</kbd>, <kbd>CadIme</kbd>)；
  - <kbd>CUSTOMAUTOIME</kbd>：打开输入法参数配置面板 (兼容别名: <kbd>ImeConfig</kbd>, <kbd>ImeSetting</kbd>)。
- **功能细节**：
- **绘图区与命令空闲状态感知**：光标处于绘图视口、拾取点、输入数值或按快捷键时，系统通过 Windows 接口强制保持英文输入状态，避免弹出拼音选词框；
- **在位文字编辑感知**：当用户双击进入单行文字 (<kbd>TEXT</kbd>/<kbd>DTEXT</kbd>)、多行文字 (<kbd>MTEXT</kbd>)、表格单元格 (`TABLEDIT`)、天正文字 (<kbd>TTEXT</kbd>/<kbd>DHWZ</kbd>) 或属性块编辑框时，自动切换为中文输入状态；退出编辑或按 <kbd>ESC</kbd> 键后立即复位回英文；
- **现代输入法协议支持**：全面支持 Windows 微软拼音、微信输入法、搜狗输入法、手心输入法等，提供 TSF 与 IMM32 双轨底层驱动；
- **命令白名单定制**：可在配置面板中自由登记需要唤起中文的自定义命令。

---

### 6.4 图层管理辅助工具 (CAD Auto LayerManager)

提供基于鼠标点选的图层快速显隐、临时隔离、无损还原与锁定控制。

- **核心快捷指令**：
  - <kbd>INKLAYER</kbd>：呼出图层快速管理控制台；
  - <kbd>INKLAYERHIDE</kbd>：点选图元所在图层即刻隐藏 (支持连续点选，回车或 <kbd>ESC</kbd> 退出)；
  - <kbd>INKLAYERSHOW</kbd>：一键开启图纸中所有已隐藏的图层；
  - <kbd>INKLAYERISO</kbd>：点选目标图元，隔离该图元所在图层（其余图层暂存快照并隐藏）；
  - <kbd>INKLAYERUNISO</kbd>：依据暂存快照精确还原隔离前的各图层状态；
  - <kbd>INKLAYERLOCK</kbd>：点选图元将其所在图层锁定；
  - <kbd>INKLAYERCUR</kbd>：点选图元将其所在图层置为当前工作图层；
  - <kbd>INKLAYERALLON</kbd>：一键解除全图所有图层的隐藏、冻结与锁定状态。
- **功能细节**：
- 点选隐藏当前工作图层时提供明确提示，避免误操作；
- 隔离与还原采用状态字典快照存储，不修改用户原始图层设置中的颜色与线型定义。

---

### 6.5 表格与文本数据提取 (CAD Auto LiveTable)

用于提取 AutoCAD 绘图区内的原生 Table 实体与零散文本，输出结构化数据。

- **核心快捷指令**：
  - <kbd>INKTABLE</kbd>：呼出表格提取控制板 (兼容别名: <kbd>INKTABLEEXP</kbd>)。
- **功能细节**：
- 支持框选 AutoCAD 原生表格实体，解析行、列、单元格合并关系及文字内容；
- 支持框选由单行文字、多行文字组成的非标准图面文字阵列，按坐标容差进行网格聚类；
- 支持将提取成果一键导出为通用 CSV 格式，方便使用 Excel 进行后续统计。

---

### 6.6 视口截图与图像导出 (CAD Auto SNAP)

用于工程汇报、技术交底及文档排版的高清位图生成与导出。

- **核心快捷指令**：
  - <kbd>CSNAP</kbd> / <kbd>INKSNAP</kbd>：唤起截图主界面。
- **功能细节**：
- **选区方式**：支持自由矩形框选模式 (<kbd>W</kbd>) 与封闭多段线/实体边界拾取模式 (<kbd>B</kbd>)；
- **透明背景 (Alpha)**：支持生成无背景透明通道 PNG 图片，便于直接嵌入 Office 或汇报幻灯片；
- **超采样分辨率**：支持 1x、2x、4x 分辨率放大输出；
- **多通道导出**：支持一键复制到位图剪贴板（直接在外部软件按 <kbd>Ctrl+V</kbd> 粘贴），以及直接保存为本地 PNG 文件；
- **打印驱动自愈机制**：若当前图纸未配置默认打印驱动，程序自动关联系统内置 PDF 虚拟驱动后重试，避免流程中断。

---

### 6.7 图纸数据库清理与维护 (CAD Auto SmartClean)

针对图纸运行卡顿、文件体积异常膨胀以及外部引用残留数据进行分阶段系统清理。

- **核心快捷指令**：
  - <kbd>INKPURGE</kbd>：执行全流程清理流水线 (兼容别名: <kbd>INKCLEAN</kbd>)。
- **分阶段清理流水线**：
- **Phase 0 (自动备份)**：在执行清理前，自动在图纸同目录下生成一份 `.dwg` 备份副本；
- **Phase 1 (零长度与空白注记图元清理)**：扫描并清除内容为空白的文字实体 (`DBText`/`MText`) 以及几何长度低于 1e-6 的微小冗余线段；
- **Phase 2 (DGN 残留字典清理)**：清除外部图纸复制引入的 `ACAD_DGNLINESTYLECOMP` 复合线型字典和冗余图层过滤器；
- **Phase 3 (级联递归深度清理)**：最多执行 10 轮递归清理，清除非引用的块定义、空图层、孤立线型、文字样式、标注样式与 RegApp 注册应用程序；
- **Phase 4 (注释比例重置与 Audit 拓扑校验)**：重置图纸注释比例列表 (`_-scalelistedit`)，并在后台调用底层数据库校验 (`_audit`) 修复拓扑指针错误。

---

### 6.8 LISP 插件管理与系统维护 (CAD Auto VlxSuite & Host)

针对 VLX 与 AutoLISP 扩展脚本的集中加载、热重载与界面维护。

- **核心快捷指令**：
  - <kbd>INKLISPMANAGER</kbd>：打开 LISP 管理器面板 (兼容别名: <kbd>LispManager</kbd>)；
  - <kbd>INKRESTOREPLUGINS</kbd>：一键重置并恢复内置 6 款 VLX 插件至出厂默认状态；
  - <kbd>INKABOUT</kbd>：查看当前插件版本、CAD 宿主环境与 .NET 运行时状态 (兼容别名: <kbd>InkMoreWorks</kbd>)；
  - <kbd>INKHELP</kbd>：在默认浏览器中打开本地离线 HTML 使用手册 (`Readme.html`)；
  - <kbd>INKMENU</kbd>：重新构建并强制挂载顶部经典下拉菜单；
  - <kbd>INKRIBBON</kbd>：重新构建并刷新功能区 Ribbon 选项卡。
- **功能细节**：
- 自动扫描管理 `Shared\VLX` 目录下的所有脚本；
- 修改脚本后可在管理器中点击“强制重载”，无需重启 AutoCAD；
- 内置沙盒执行防护机制，当脚本发生异常中断时自动恢复 `OSMODE`、`CMDECHO` 等 CAD 系统变量。

---

## 7. AutoLISP / VLX 扩展工具库说明

套件内包含 6 款经典的实用 VLX 工具，除保留各自原始命令外，统一注册了以 `Ink` 为前缀的快捷别名：


| 工具代号 | 工具全称                   | 原始启动命令                                                                                                                         | 套件快捷别名                                                                                                                                     | 核心功能与应用场景                                                                                                            |
|:-------------|:-------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------|:-------------------------------------------------------------------------------------------------------------------------------------------------------|:---------------------------------------------------------------------------------------------------------------------------------------|
| **ZBBZ**     | 坐标高程综合标注系统 | <kbd>ZBBZ</kbd><br><kbd>PLBZ</kbd><br><kbd>SCTB</kbd><br><kbd>ZBSZ</kbd> | <kbd>InkZBBZ</kbd><br><kbd>InkPLBZ</kbd><br><kbd>InkSCTB</kbd><br><kbd>InkZBSZ</kbd> | 单点动态引线跟随标注、批量图元 8 向文字智能避让标注、生成坐标成果表、WCS/UCS 坐标系与前缀设置 |
| **BHSX**     | 智能编号速写系统       | <kbd>BHSX</kbd><br><kbd>BHTB</kbd>                                                                           | <kbd>InkBHSX</kbd><br><kbd>InkBHTB</kbd>                                                                                 | 数字与字母递增编号、5 种空间拓扑排序算法 (最短路径/行列矩阵/曲线顺延)、提取编号坐标成果表     |
| **ZCD**      | 长度统计标注工具       | <kbd>ZCD</kbd>                                                                                                                 | <kbd>InkZCD</kbd>                                                                                                                          | 支持 17 种几何线元混合框选统计、自动分项序号标注、单位换算与直接插入 CAD 原生表格                  |
| **CZTH**     | 文本与数值替换系统    | <kbd>CZTH</kbd><br><kbd>CZTH-RESET</kbd>                                                                     | <kbd>InkCZTH</kbd><br><kbd>InkCZTHReset</kbd>                                                                            | 支持通配符与正则匹配、测绘数值运算 (`=`, `±`, `~`, `++`, `+`, `*`)、联动更新块 Z 轴高程坐标               |
| **XZH**      | 全能线清洗转换系统    | <kbd>XZH</kbd>                                                                                                                 | <kbd>InkXZH</kbd>                                                                                                                          | 识别 17 种线型对象并进行清洗，转换为轻量多段线 (LWPolyline)、面域 (Region) 或编组 (Group)                   |
| **XZX**      | 泛型图元转换矩阵       | <kbd>XZX</kbd>                                                                                                                 | <kbd>InkXZX</kbd>                                                                                                                          | 批量将选中图元按几何中心重构为圆、矩形、外接框、正多边形，提供闭合诊断与安全向内/外偏移     |


---

## 8. 常见问题排查 (FAQ)

### Q1: AutoCAD 启动后，命令行提示“未知命令”或无法自动加载？

> [!TIP]
> **排查方案**：通常由于 AutoCAD 注册表项受到操作系统权限限制未能自动写入。可按以下步骤处理：
> 1. 打开 AutoCAD 命令行，输入 <kbd>NETLOAD</kbd> 并回车；
> 2. 浏览至安装目录对应世代文件夹下的 `Plugins\InkVerseX.dll`（如 AutoCAD 2021 选择 `{安装目录}\R24\Plugins\InkVerseX.dll`）手动加载；
> 3. 加载成功后，插件会在当前 CAD 配置文件中自动修复自启动注册表项。

### Q2: 启动时弹出“安全警告：未签名的高级可信路径”？

> [!NOTE]
> **排查方案**：这是 AutoCAD 自 2014 版本引入的代码安全防护机制：
> 1. 在命令行输入 <kbd>OP</kbd> 打开“选项”对话框；
> 2. 切换至“文件”标签页，展开“受信任的位置”；
> 3. 点击“添加”，将墨语 X 安装目录（如 `%APPDATA%\InkVerseX\...`）添加至列表中并确认即可。

### Q3: 经典工具栏按钮呈现为问号图标？

> [!TIP]
> **排查方案**：图标位图位于安装目录的 `Shared\Resources` 文件夹中：
> 1. 在命令行输入 <kbd>OP</kbd> 打开“选项”对话框；
> 2. 在“文件”标签页展开“支持文件搜索路径”；
> 3. 检查并确认列表中已包含 `{安装目录}\Shared\Resources`；
> 4. 随后在命令行执行 <kbd>INKMENU</kbd> 命令，系统将自动重新构建并刷新工具栏。

### Q4: 卸载插件后，原图纸中的标注和图框会失效或产生代理实体 (Proxy Entity) 吗？

> [!NOTE]
> **解答**：完全不会。墨语 X 产生的所有对象均基于 AutoCAD 核心原生数据结构（原生图块、原生表格、轻量多段线、单行/多行文本），不创建任何第三方自定义代理实体。在任何纯净无插件的 AutoCAD 或其他兼容 CAD 软件中打开图纸，均可完整查看、测量、修改与打印。

### Q5: 智能输入法切换在部分现代第三方输入法下无法自动切换？

> [!TIP]
> **排查方案**：墨语 X 针对 Windows 自带微软拼音与主流第三方输入法（微信输入法、搜狗输入法、手心输入法）进行了深度优化。若使用的是微信输入法或搜狗输入法，请在命令行输入 <kbd>CUSTOMAUTOIME</kbd> 打开配置面板，确认当前输入法类型选择正确，并确保输入法软件自身的“中英文切换快捷键”设置为默认的 Shift 键即可实现稳定感知切换。

---

## 9. 仓库文件结构

```text
InkVerse X（墨语 X）/  
├── InkVerse X Scr/         # 宿主程序源码 (C# 引导与加载工程)  
│   ├── Commands/           # 宿主公共命令 (INKMENU, INKABOUT 等)  
│   ├── Loader/             # 程序集看门狗与动态扫描引擎  
│   ├── Resources/          # 菜单与工具栏图标、说明截图资源  
│   └── UI/                 # 关于面板与更新检测界面  
├── NetSuite/               # 8 大 C# 独立子模块源码链接  
│   ├── CAD Auto BP_Frame/  # 智能图框与排版工程  
│   ├── CAD Auto BatchPrint/# 批量打印与切图工程 (v0.1.4 重磅新增)  
│   ├── CAD Auto IME/       # 智能中英输入法工程  
│   ├── CAD Auto LayerManager/# 图层快捷管理工程  
│   ├── CAD Auto LiveTable/ # 实时表格与数据提取工程  
│   ├── CAD Auto SNAP/      # 高清视口截图工程  
│   ├── CAD Auto SmartClean/# 图纸数据库深度清理工程  
│   └── CAD Auto VlxSuite/  # LISP / VLX 插件管理工程  
├── Installer/              # Inno Setup 打包脚本与安装发布资产  
│   ├── InkVerseX_Installer.iss  # 综合套件安装脚本  
│   ├── build_all_installers.ps1 # 全系一键编译与打包脚本  
│   ├── Readme.html         # 本地离线图文交互使用手册  
│   └── Resources/          # 原生模块图标与 docs/ 高清界面截图  
├── TEST/                   # MSTest 自动化单元测试工程 (43 项算法用例)  
├── Directory.Build.props   # 全局多版本编译属性  
├── InkVerseX.slnx          # 解决方案主工程  
└── README.md               # 项目说明文档
```

---

## 10. 技术支持与联系

- **项目作者**：浅醉·墨语 (Andy_127)
- **联系方式**：QQ: 806894478 | Email: [ypj127@163.com](mailto:ypj127@163.com)
- **仓库**：[https://github.com/Andyang127/InkVerseX](https://github.com/Andyang127/InkVerseX)

<div align="center"><br><img src="Resources/docs/Contact.png" alt="欢迎使用墨语 X 工具箱 - 打赏支持与建议反馈" width="560" style="max-width: 100%; border-radius: 8px; box-shadow: 0 4px 16px rgba(0,0,0,0.08);" /><br><sub>欢迎使用【墨语 X】工具箱 · 打赏支持开发者 · 建议与 BUG 反馈</sub></div>

---

*版权所有 (C) 2026 InkVerse X (墨语 X). 保留所有权利。*
