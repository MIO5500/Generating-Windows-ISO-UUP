# 关于

本项目从[统一更新平台（UUP）](https://docs.microsoft.com/en-us/windows/deployment/update/windows-update-overview)获取最新的 Windows，并生成 ISO 镜像文件。

它将 [UUP dump](https://git.uupdump.net/uup-dump) 项目封装为单个命令。

# 该项目是为 GitHub Actions 所编写
只需fork此仓库，并使用 GitHub Actions 来构建您自己的版本。

GitHub Actions 执行完成后，您将获得一个完成的 ISO 镜像，提供两种形式：

1. 发行版（Release）：将 .iso 文件拆分为多个部分（因为 GitHub 不允许发行版中包含大于 2GB 的文件）。  
   此外，每个发行版还会为不同的系统和架构生成软件包，其中包含一个现成的脚本，该脚本可下载拆分 ISO 的所有部分并还原为完整的 ISO 文件。
2. 构建产物（Artifacts）：单个 zip 文件，但需要登录网站才能下载。

# 您也可以直接在 Windows x64 或 arm64 主机（最低版本 21H2）上执行。

## 支持以下内容：
Windows 版本：
* `windows-10`：Windows 10 19045（即 22H2）
* `windows-11old`：Windows 11 22631（即 23H2）
* `windows-11`：Windows 11 26100（即 24H2）
* `windows-11beta`：Windows 11 26120（即 24H2 Beta）
* `windows-11new`：Windows 11 26200（即 25H2）
* `windows-11dev`：Windows 11 26220（即 25H2 Beta）
* `windows-1126h1`：Windows 11 28000（即 26H1）<details><summary>详细信息</summary>用于 2026 年搭载特定新型芯片（如 Snapdragon X2）的新设备，以启用新的硬件创新——不适用于现有 PC 或一般企业部署。</details>

* `windows-dev`：Windows 11 26300（即 Dev）
* `windows-canary`：Windows 11 Insider Preview（即 Canary）

体系结构：
* `x64`
* `arm64`

版本（Edition）：
* `home`：家庭版
* `pro`：专业版
* `multi`：家庭版 + 专业版

语言：
* `ar-sa`：阿拉伯语（沙特阿拉伯）
* `bg-bg`：保加利亚语（保加利亚）
* `cs-cz`：捷克语（捷克共和国）
* `da-dk`：丹麦语（丹麦）
* `de-de`：德语（德国）
* `el-gr`：希腊语（希腊）
* `en-gb`：英语（英国）
* `en-us`：英语（美国）
* `es-es`：西班牙语（西班牙）
* `es-mx`：西班牙语（墨西哥）
* `et-ee`：爱沙尼亚语（爱沙尼亚）
* `fi-fi`：芬兰语（芬兰）
* `fr-ca`：法语（加拿大）
* `fr-fr`：法语（法国）
* `he-il`：希伯来语（以色列）
* `hr-hr`：克罗地亚语（克罗地亚）
* `hu-hu`：匈牙利语（匈牙利）
* `it-it`：意大利语（意大利）
* `ja-jp`：日语（日本）
* `ko-kr`：韩语（韩国）
* `lt-lt`：立陶宛语（立陶宛）
* `lv-lv`：拉脱维亚语（拉脱维亚）
* `nb-no`：书面挪威语（挪威）
* `nl-nl`：荷兰语（荷兰）
* `pl-pl`：波兰语（波兰）
* `pt-br`：葡萄牙语（巴西）
* `pt-pt`：葡萄牙语（葡萄牙）
* `ro-ro`：罗马尼亚语（罗马尼亚）
* `ru-ru`：俄语（俄罗斯）
* `sk-sk`：斯洛伐克语（斯洛伐克）
* `sl-si`：斯洛文尼亚语（斯洛文尼亚）
* `sr-latn-rs`：塞尔维亚语（拉丁文，塞尔维亚）
* `sv-se`：瑞典语（瑞典）
* `th-th`：泰语（泰国）
* `tr-tr`：土耳其语（土耳其）
* `uk-ua`：乌克兰语（乌克兰）
* `zh-cn`：中文（简体，中国）
* `zh-tw`：中文（繁体，台湾）

附加选项：
* `esd`：使用 ESD 压缩
* `drivers`：从 Drivers 文件夹添加驱动
* `netfx3`：添加 .NET Framework 3.5
* `revision`：系统修订号

## 使用方法

获取最新的 Windows 11 25H2 ISO：

```bash
powershell uup-dump-get-windows-iso.ps1 windows-11new c:/output -architecture x64 -edition pro -lang en-us -esd -drivers -netfx3
```

一切顺利后，ISO 将位于 `output` 目录中，例如 `c:/output/26200.7899.250826-1428.25H2_GE_RELEASE_SVC_PROD3_CLIENTPRO_OEMRET_X64FRE_PL-PL.ISO`。

您还可以下载指定的系统修订版本。例如，如果要构建 25H2 26200.7705 ISO：

```bash
powershell uup-dump-get-windows-iso.ps1 windows-11new c:/output -architecture x64 -edition pro -lang en-us -esd -drivers -netfx3 -revision 7705
```

## 标签结构

```text
  .------------------------------- 操作系统版本
  |    .-------------------------- 系统修订号
  |    |    .--------------------- 发布通道/版本
  |    |    |    .---------------- 系统版本（Edition）
  |    |    |    |   .------------ CPU 体系结构
  |    |    |    |   |  .--------- 语言
  |    |    |    |   |  |  .------ 使用 ESD 压缩（可选）
  |    |    |    |   |  |  | .---- 包含附加驱动（可选）
  |    |    |    |   |  |  | | .-- 包含 .NET Framework 3.5（可选）
__|__ _|__ _|__ _|_ _|_ |_ | | |
26200.7899.25H2.PRO.X64.PL.E.D.N
```

## 相关工具

* [Rufus](https://github.com/pbatard/rufus)
* [Fido](https://github.com/pbatard/Fido)
* [windows-evaluation-isos-scraper](https://github.com/rgl/windows-evaluation-isos-scraper)

## 参考

* [UUP dump 主页](https://uupdump.net)
* [UUP dump 源代码](https://git.uupdump.net/uup-dump)
* [统一更新平台（UUP）](https://docs.microsoft.com/en-us/windows/deployment/update/windows-update-overview)
