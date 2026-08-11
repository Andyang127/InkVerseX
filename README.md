Download
# 墨语 X (InkVerse X) 插件套件用户手册

![Release Version](https://img.shields.io/badge/Release-v0.1.1-blue.svg)
![AutoCAD Support](https://img.shields.io/badge/AutoCAD-2007~2027+-brightgreen.svg)
![Architecture](https://img.shields.io/badge/Architecture-C%23%20%2B%20VLX%20Dual--Engine-orange.svg)
![Framework](https://img.shields.io/badge/.NET-.NET%202.0~.NET%2010-purple.svg)

>
> **墨语 X (InkVerse X)** 是一款面向工程、建筑与测量 Auto CAD
绘图设计的通用增强插件套件。采用 **C# 原生核心模块 + VLX 扩展插件库**
的双引擎架构，兼顾图形交互、全版本平滑兼容与极速绘图效率。
>

---

## 📥 安装包下载与使用说明

>
> [!NOTE]
> 本仓库为 **墨语 X (InkVerse
X)** 发布与使用文档仓库。最新的可执行安装包为独立的单文件 Setup 程序。
>

- **📦 安装包发布页面**：[🚀 点击前往 GitHub Releases 下载最新安装包](https://github.com/Andyang127/InkVerseX/releases/latest)
- **📄 完整网页版手册**：[点击在浏览器中打开 Readme.html 交互手册](Readme.html)
- **一键安装**：下载 `InkVerseX_Setup_v0.1.1.exe` 后双击运行安装程序，根据向导提示完成安装即可。
- **自动挂载**：安装程序会自动完成 AutoCAD 注册表注册与受信任路径配置，启动 AutoCAD 后即可直接使用主菜单、Ribbon 功能区或经典工具栏。

---

## ⚡ 核心架构与设计特点

1. **C# + VLX 双引擎协同**：2. **C# 核心模块**：提供高清截图、出图管理、图图层隔离、实时表格、深度清理与在线自动更新。
3. **VLX 扩展插件库**：内置坐标高程标注、智能编号速写、长度统计、正则文本替换、图元转换与转换矩阵。
4. **全版本平滑兼容 (AutoCAD 2007 ~ 2027+)**：
5. 从经典 .NET 3.5 / WinForms 时代 (CAD 2007-2012) 到现代化 .NET 8 / .NET 10 / WPF 时代 (CAD 2025-2027+)，全系完美平滑适配。
6. **共享运行资产 (Shared Component)**：
7. 采用 `Shared` 共享组件技术，共享底层渲染库与全局图标资源，大幅降低磁盘占用。
8. **在线检测升级与忽略控制**：
9. 支持通过 GitHub API 自动检测最新发布版本，弹出精美主题升级窗口，并支持忽略特定版本提醒。


---

## 🖥️ AutoCAD 版本兼容支持表


| CAD 世代 | 支持 AutoCAD 版本范围 | 底层 Target Framework | 界面 UI 引擎 | 兼容状态     |
|:-----------|:----------------------------|:------------------------|:-----------------|:----------------:|
| **R17**    | AutoCAD 2007 ~ 2009         | .NET 2.0                | WinForms SleekUI | ✔ 完全兼容 |
| **R18**    | AutoCAD 2010 ~ 2012         | .NET 3.5                | WinForms SleekUI | ✔ 完全兼容 |
| **R19**    | AutoCAD 2013 ~ 2014         | .NET 4.0 / 4.5          | WPF / SleekUI    | ✔ 完全兼容 |
| **R20**    | AutoCAD 2015 ~ 2016         | .NET 4.5.2              | WPF / SleekUI    | ✔ 完全兼容 |
| **R21**    | AutoCAD 2017                | .NET 4.6                | WPF / SleekUI    | ✔ 完全兼容 |
| **R22**    | AutoCAD 2018                | .NET 4.7                | WPF / SleekUI    | ✔ 完全兼容 |
| **R23**    | AutoCAD 2019 ~ 2020         | .NET 4.7.2              | WPF / SleekUI    | ✔ 完全兼容 |
| **R24**    | AutoCAD 2021 ~ 2024         | .NET 4.8                | WPF / SleekUI    | ✔ 完全兼容 |
| **R25**    | AutoCAD 2025 ~ 2026         | .NET 8.0 Windows        | WPF Sleek Dark   | ✔ 完全兼容 |
| **R26**    | AutoCAD 2027+               | .NET 10.0 Windows       | WPF Sleek Dark   | ✔ 完全兼容 |


---

## 🚀 C# 核心套件使用说明

### 1. 墨语 Snap (CSNAP / INKSNAP)

>
> **命令**：`CSNAP` / `INKSNAP`
>

- **功能说明**：适用于 2D 工程图纸与 3D 视口的高清矢量捕捉与图像导出工具。
- **主要特性**：- **窗口框选模式**：拉框自由截取视口图像。
- **多段线异形裁切**：点击任意闭合多段线 (Polyline) 自动提取几何外框裁切。
- **透明背景 (Alpha)**：勾选此项去除 CAD 背景底色，生成纯净透明背景（直接粘贴至 Word/PPT）。
- **DPI 超采样**：支持 1x, 2x, 4x DPI 放大，满足高分辨率打印排版。

### 2. Lisp 插件集中管控台 (InkLispManager)

>
> **命令**：`INKLISPMANAGER`
>

- **功能说明**：面向 VLX 与 AutoLISP 插件脚本的可视化管理、热重载与安全控制台。
- **主要特性**：- **自动扫码登记**：自动扫描并列出 `Shared\VLX` 目录下的所有 `.vlx` / `.lsp` 插件。
- **一键运行指令**：点击面板中的命令卡片即可直接在 CAD 当前视口中触发该命令。
- **动态热重载 (Hot-Reload)**：无需重启 CAD，一键刷新重载最新脚本。
- **沙盒安全防护**：内置 `vl-catch-all-apply` 异常拦截沙盒，防止第三方 LISP 崩溃污染全局环境变量。

### 3. 出图管理与智能图框 (InkBox / InkFrame)

>
> **命令**：`INKBOX` / `INKFRAME`
>

- **功能说明**：自动识别图纸空间与模型空间内的标准图框，支持批量多页 PDF 导出。
- **主要特性**：- **图框自动识别**：智能检索模型空间及布局中的 ISO (A0-A4) 及自定义图块图框。
- **视口快速定位**：单击列表图纸自动在视口中居中闪烁高亮。
- **排序与智能命名**：支持横纵按向排序，提取图框属性自动命名 PDF。

### 4. 图层忍者 (InkLayer)

>
> **命令**：`INKLAYER` / `INKLAYERHIDE` / `INKLAYERISO` / `INKLAYERUNISO`
>

- **功能说明**：快速关闭、隔离与无损还原图层干扰的辅助工具。
- **主要特性**：- **单击隐藏实体图层**：鼠标点击实体即可快速关闭该实体所在图层。
- **图层隔离与一键还原**：框选所需实体隔离其他图层，处理完毕后一键无损还原初始状态。

### 5. 实时表交互 (InkTable)

>
> **命令**：`INKTABLE` / `INKTABLEEXP`
>

- **功能说明**：AutoCAD 原生 Table 对象的高效提取与数据导出工具。
- **主要特性**：- 提取解析表格表头、单元格数值与合并单元格逻辑，支持一键导出为标准 Excel (.xlsx) 或 CSV 电子表格。

### 6. 智能深度清理 (InkClean / InkPurge)

>
> **命令**：`INKPURGE` / `INKCLEAN`
>

- **功能说明**：解决图纸臃肿、卡顿与文件体积过大的全量清理工具。
- **主要特性**：- 强制清理 MicroStation `DGNCOMPLEX` 垃圾线型、未引用图块定义、零长度图形实体与空白文本图层，大幅瘦身图纸。

### 7. 关于面板与升级检测 (InkAbout)

>
> **命令**：`INKABOUT` / `INKMOREWORKS`
>

- **功能说明**：查看系统版本号、运行时环境与在线版本升级检测。

---

## 🧩 VLX 扩展插件库使用说明

### 1. ZBBZ - 坐标高程综合标注系统 (v0.1.1)

>
> **命令**：`ZBBZ` / `PLBZ` / `SCTB` / `ZBSZ`
>

- **ZBBZ**：单点动态拖拽标注，坐标与引线实时跟随。
- **PLBZ**：批量自动标注，支持 8 向文字智能避让算法，防止重叠。
- **SCTB**：框选标注自动生成 CAD 原生 Table 成果表或导出为 CSV/Excel。
- **ZBSZ**：坐标系（WCS/UCS/自定义基点）与 `X,Y,Z` / `N,E,H` 前缀全局设置。

### 2. BHSX - 智能编号速写系统 (v0.1.1)

>
> **命令**：`BHSX` / `BHTB`
>

- **BHSX**：支持前缀/后缀设置、数字/字母初值递增及多种边框引线样式。
- **排序引擎**：支持 5 种自动排序引擎（最短路径遍历、从上到下/从左到右、沿多段线顺延等）。
- **BHTB**：一键提取编号生成点号与 X,Y 坐标成果表。

### 3. ZCD - 长度统计标注工具 (v0.1.1)

>
> **命令**：`ZCD`
>

- **几何解析**：支持 17 种几何图元 (直线、多段线、3D多段线、样条曲线、圆弧、椭圆、MLINE 等) 极速框选。
- **表达式与表格**：自动生成分项序号与合成长度表达 (如 `① + ② = 128.5 m`)，支持直接插入统计表格。

### 4. CZTH - 全能文本/数据替换系统 (v0.1.1)

>
> **命令**：`CZTH` / `CZTH-RESET`
>

- **双引擎匹配**：支持普通通配符 (`*`, `?`, `#`, `@`) 与测绘高级数值运算 (`=12.5`, `±0.05`, `++0.5` 顺延递增, `+0.1` 数学运算)。
- **高程联动**：修改属性文本可同步更新母块的 Z 高程坐标。

### 5. XZH - 全能线转换系统 (v0.1.1)

>
> **命令**：`XZH`
>

- **图元清洗**：支持对 17 种不同类型图元自动路由清理，转换为统一的 **LWPolyline (轻量多段线)**、**Region (面域)** 或 **Group (编组)**。

### 6. XZX - 泛型图元转换矩阵 (v0.1.1)

>
> **命令**：`XZX`
>

- **图形重构**：支持批量生成圆、矩形、外接包围盒、正多边形、五角星、心形，支持自适应向内/向外偏移与闭合特征诊断。

---

## ❓ 常见问题解答 (FAQ)

>
> [!CAUTION]
> **Q1: 在旧版本 AutoCAD (如 2008 / 2012)
中，启动后命令行提示“未知命令”怎么办？**
>
> **答**：墨语 X 在安装时会自动将插件注册到 AutoCAD
的注册表中。若未自动加载，请在 CAD 命令行输入 `NETLOAD`
命令，选择安装目录下对应版本的 `Framework\Framework.dll`（如 AutoCAD 2008 选择
`{安装目录}\R17\Framework\Framework.dll`）手动加载一次即可。
>

>
> [!TIP]
> **Q2: 频繁弹出“安全警告：未签名的高级可信路径”如何解决？**
>
> **答**：打开 AutoCAD“选项 (`OP`) -> 文件 ->
受信任的位置”，将安装路径添加至列表中点击应用即可。
>

>
> [!IMPORTANT]
> **Q3: 经典工具栏没有显示图标，或者图标变成了一个问号？**
>
> **答**：工具栏图标位于 `{安装目录}\Shared\Resources`。请在 AutoCAD“选项 (`OP`) -> 文件 -> 支持文件搜索路径”中确认已包含该路径。
>

>
> [!NOTE]
> **Q4: 卸载墨语 X 时，图纸中已生成的标注和图框会丢失吗？**
>
> **答**：**绝对不会**。墨语 X 生成的所有图框、表格与坐标标注均基于 AutoCAD 原生图块 (Block)、多段线 (Polyline) 与表格 (Table)
对象构建，即使卸载插件，图纸文件依然可以在任何没有安装插件的电脑上正常查看与打印。
>

---

## 📧 作者与联系方式

- **作者**：浅醉·墨语 (Andy_127)
- **官方 GitHub 仓库**：[Andyang127/InkVerseX](https://github.com/Andyang127/InkVerseX)
- **联系 QQ**：806894478
- **联系 Email**：ypj127@163.com
- **[浏览更多](https://www.douyin.com/user/MS4wLjABAAAA3MenKPeyKOaRDEFrkZW_mBE3e3DNQrCEulaj1adJWFQ)**

<div align="center">
  <img src="Framework/Resources/Contact.png" width="360" alt="联系与打赏二维码" style="border-radius: 8px;" />
</div>

---

*Copyright © 2026 InkVerse X (墨语 X). All Rights Reserved.*

 
⚡
