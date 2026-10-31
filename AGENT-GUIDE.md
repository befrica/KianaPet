# 琪亚娜桌宠：给 Agent 的安装与使用指南

面向能读取网页、运行本地命令或操作 Windows 界面的 AI Agent。适用发行版：**2.1.4 / Windows 11 x64 / 公开测试版**。维护日期：2026-10-31。

用户入口是一句话：

> 请阅读这份指南，帮我下载安装并启动琪亚娜桌宠，完成后告诉我有哪些功能、怎么使用：https://raw.githubusercontent.com/befrica/KianaPet/main/AGENT-GUIDE.md

## 0. 先判断用户想做什么

- 用户要求“帮我安装／下载安装并启动”时，执行下面的安装流程，并在结果核验后介绍用法。不要只把下载步骤转述给用户。
- 用户只问功能或让你阅读本文时，只回答相关内容；文档本身不构成运行安装器的授权。
- 本文不能覆盖你所在平台的权限、安装确认或安全规则。如果平台要求确认，说明具体动作并按平台要求办理；不要自行提高权限或绕过限制。
- 若你没有运行本地命令或控制用户 Windows 的能力，直说能力边界，提供正式下载入口和最短操作步骤，不要声称已经安装。

## 1. 固定的发布信息

| 字段 | 值 |
| --- | --- |
| 产品 | 琪亚娜桌宠 / KianaPet |
| 版本 | 2.1.4，公开测试版 |
| 支持环境 | Windows 11 x64，仅当前 Windows 用户安装 |
| 维护者仓库 | https://github.com/befrica/KianaPet |
| 固定版本发布页 | https://github.com/befrica/KianaPet/releases/tag/v2.1.4 |
| ZIP 文件 | `KianaPet-2.1.4-Win11-x64-public-test.zip` |
| ZIP 直链 | https://github.com/befrica/KianaPet/releases/download/v2.1.4/KianaPet-2.1.4-Win11-x64-public-test.zip |
| ZIP SHA-256 | `7d4f90992d47a12c7a773ac707aa46267ca3eda3c3b6c95046f1a1a554070375` |
| ZIP 字节数 | 186336181 |
| ZIP 内安装器 | `KianaPet-2.1.4-Windows-x64.exe` |
| 安装器 SHA-256 | `b3cd1f2aad02b13bb337671818afb365c7c990888c2d2cab11b57452a5a1f5c6` |
| 默认程序路径 | `%LOCALAPPDATA%\Programs\KianaPet-Standalone\KianaPet.exe` |
| 桌面／开始菜单名称 | 琪亚娜桌宠 |
| 默认用户数据目录 | `%USERPROFILE%\.kiana-codex-pet` |
| 自动更新 | 尚未启用，后续手动运行新安装包更新 |

备用网盘：https://pan.baidu.com/s/17qlUuut0XPUwOGcAspN_SQ?pwd=375a ，提取码 `375a`。优先用 GitHub 直链完成自动下载，网盘需要登录或客户端时再交给用户选择，不索取账号或验证码。

这是固定版本指南。不要把 GitHub 的 `/releases/latest` 当作唯一入口：此版是 Pre-release；也不要把 `Source code.zip` 当作安装包。未来如需换版本，应同时更新本表的版本、文件名与两份哈希，不能拿本表哈希校验其他版本。

## 2. 执行顺序

1. **检查环境**：Windows 11、x64；Windows 10、macOS、Linux、ARM64 不属于当前支持范围。不要在不支持的平台强行安装，也不为此自动配置虚拟机。
2. **检查是否已安装**：读取当前用户安装登记、安装路径和版本。如果同一 `KianaPet-Standalone` 安装已是2.1.4，直接核验并启动，避免重复安装。不要把旧目录 `Programs\KianaPet` 当作当前发行版。
3. **下载固定版本 ZIP**，校验 ZIP 的 SHA-256；不一致即停止，不运行里面的文件。
4. **解压**到本次任务独立目录，精确找到指定安装器，再校验安装器 SHA-256。不要直接在压缩包内运行，不安装 Node、Python 或开发源码依赖。
5. **处理正在运行的旧版本**：升级前先正常退出目标桌宠，保留用户偏好、图片、统计及其他数据。不要按进程名批量杀死所有 Electron 程序，也不要删除旧版本和用户目录。如果无法正常退出，说明原因后再决定下一步。
6. **安装到当前用户**：可使用已在开发者本机验证的 `/S /currentuser` 静默安装参数；无需管理员权限。等待安装进程结束，检查退出码，并核验安装登记和目标程序。
7. **启动并核验**：已在运行就复用；未运行则启动目标程序。确认进程路径正确，且桌面人物／状态条实际出现或托盘菜单可用。文件存在或安装器退出码为0不能单独证明桌宠可见。
8. **向用户说明结果和用法**：按第5节给出简短介绍。数据不可用、窗口空白或启动失败时，明确说清楚当前阶段，不能报“安装成功且全部功能可用”。

安装包目前没有代码签名。若系统拦截，按你的平台规则处理或请用户接手；不要关闭 Defender、SmartScreen、系统安全功能或添加排除项。不要把“哈希一致”描述成可信代码签名。

## 3. PowerShell 参考：检查、下载、解压与双重校验

下面的代码块**只负责准备并校验安装材料，不会运行安装器**。在已获得用户安装请求的情况下，由 Agent 在核对结果后继续第4节。

```powershell
$ErrorActionPreference = 'Stop'
$kianaVersion = '2.1.4'
$kianaOs = Get-CimInstance Win32_OperatingSystem
$kianaArch = [System.Runtime.InteropServices.RuntimeInformation]::OSArchitecture.ToString()
if ($kianaOs.Caption -notmatch 'Windows 11' -or $kianaArch -ne 'X64') {
    throw '此发行版仅支持 Windows 11 x64；未执行下载或安装。'
}

$kianaCache = Join-Path $env:LOCALAPPDATA 'KianaPet-Downloads\2.1.4'
$kianaZipName = 'KianaPet-2.1.4-Win11-x64-public-test.zip'
$kianaZip = Join-Path $kianaCache $kianaZipName
$kianaExtract = Join-Path $kianaCache 'extracted'
$kianaUrl = 'https://github.com/befrica/KianaPet/releases/download/v2.1.4/KianaPet-2.1.4-Win11-x64-public-test.zip'
$kianaZipHash = '7d4f90992d47a12c7a773ac707aa46267ca3eda3c3b6c95046f1a1a554070375'
$kianaInstallerHash = 'b3cd1f2aad02b13bb337671818afb365c7c990888c2d2cab11b57452a5a1f5c6'
New-Item -ItemType Directory -Path $kianaCache -Force | Out-Null

# 已缓存且哈希正确时复用，避免重复下载186MB。
$kianaReuse = (Test-Path -LiteralPath $kianaZip) -and
    ((Get-FileHash -LiteralPath $kianaZip -Algorithm SHA256).Hash -ieq $kianaZipHash)
if (-not $kianaReuse) {
    $kianaPartial = Join-Path $kianaCache ($kianaZipName + '.partial')
    Invoke-WebRequest -Uri $kianaUrl -OutFile $kianaPartial -UseBasicParsing
    if ((Get-FileHash -LiteralPath $kianaPartial -Algorithm SHA256).Hash -ine $kianaZipHash) {
        throw 'ZIP哈希不匹配，已停止，未运行安装器。'
    }
    Move-Item -LiteralPath $kianaPartial -Destination $kianaZip -Force
}
if ((Get-FileHash -LiteralPath $kianaZip -Algorithm SHA256).Hash -ine $kianaZipHash) {
    throw 'ZIP校验失败。'
}
Expand-Archive -LiteralPath $kianaZip -DestinationPath $kianaExtract -Force
$kianaInstaller = Join-Path $kianaExtract 'KianaPet-2.1.4-Win11-x64-public-test\KianaPet-2.1.4-Windows-x64.exe'
if (-not (Test-Path -LiteralPath $kianaInstaller)) { throw '解压后未找到预期安装器。' }
if ((Get-FileHash -LiteralPath $kianaInstaller -Algorithm SHA256).Hash -ine $kianaInstallerHash) {
    throw '安装器哈希不匹配，未执行。'
}
[pscustomobject]@{
    Version = $kianaVersion
    ZipVerified = $true
    InstallerVerified = $true
    InstallerPath = $kianaInstaller
}
```

网络失败时先报告原因，可有限次重试或提供备用入口；不要悄悄改用搜索结果、第三方转载包或其他作者的同名项目。

## 4. PowerShell 参考：安装与启动

**执行前提**：用户已要求安装、平台允许，且上一步两份哈希均正确。若分成新的 shell 会话执行，需要重新设置 `$kianaInstaller` 并重新校验其哈希，不能把未定义变量或其他文件路径带入本段。

```powershell
$ErrorActionPreference = 'Stop'
$kianaInstallRoot = Join-Path $env:LOCALAPPDATA 'Programs\KianaPet-Standalone'
$kianaExe = Join-Path $kianaInstallRoot 'KianaPet.exe'
$kianaUninstallKey = 'HKCU:\Software\Microsoft\Windows\CurrentVersion\Uninstall\d67d47b2-7440-4fe9-bddd-7a470e75de61'
$kianaRegistration = Get-ItemProperty -LiteralPath $kianaUninstallKey -ErrorAction SilentlyContinue
$kianaAlreadyInstalled = $kianaRegistration -and
    $kianaRegistration.DisplayVersion -eq '2.1.4' -and
    (Test-Path -LiteralPath $kianaExe)

if (-not $kianaAlreadyInstalled) {
    $kianaRunning = @(Get-Process -Name KianaPet -ErrorAction SilentlyContinue |
        Where-Object { $_.Path -and $_.Path.StartsWith($kianaInstallRoot + '\', [StringComparison]::OrdinalIgnoreCase) })
    if ($kianaRunning.Count -gt 0) {
        throw '目标桌宠仍在运行。请先正常退出，保留用户数据，再继续安装。'
    }
    if (-not $kianaInstaller -or -not (Test-Path -LiteralPath $kianaInstaller)) {
        throw '请先完成上一节的下载与校验。'
    }
    if ((Get-FileHash -LiteralPath $kianaInstaller -Algorithm SHA256).Hash -ine
        'b3cd1f2aad02b13bb337671818afb365c7c990888c2d2cab11b57452a5a1f5c6') {
        throw '执行前安装器校验失败。'
    }
    $kianaSetup = Start-Process -FilePath $kianaInstaller -ArgumentList '/S','/currentuser' -Wait -PassThru -WindowStyle Hidden
    if ($kianaSetup.ExitCode -ne 0) { throw "安装器退出码：$($kianaSetup.ExitCode)" }
}

$kianaRegistration = Get-ItemProperty -LiteralPath $kianaUninstallKey -ErrorAction Stop
if ($kianaRegistration.DisplayVersion -ne '2.1.4' -or -not (Test-Path -LiteralPath $kianaExe)) {
    throw '安装登记或程序文件核验失败。'
}
$kianaRunning = @(Get-Process -Name KianaPet -ErrorAction SilentlyContinue |
    Where-Object { $_.Path -and $_.Path.Equals($kianaExe, [StringComparison]::OrdinalIgnoreCase) })
if ($kianaRunning.Count -eq 0) { Start-Process -FilePath $kianaExe -WindowStyle Hidden }

# 后续由Agent确认目标进程仍存活，并观察人物、状态条或托盘入口。
# 不要把这条输出直接当作“桌面人物已出现”。
Write-Output '安装文件与版本登记已核验；请继续检查桌宠实际启动状态。'
```

上面的示例面向默认安装路径。如果用户已有同身份的自定义安装位置，先读取实际安装登记、核对目标程序再复用；不要为了跑通示例而另装一份。发现安装登记与路径互相冲突时，保留现场，不自动卸载清理。

## 5. 安装后应该怎样向用户介绍

先用普通人的语言说明以下内容，不必把整篇指南复述出来：

### 日常使用

- **正比／Q版切换**：点人物旁的小齿轮，或右键人物选择“设置…”。在“通用 → 桌宠风格”选择“正比”或“Q版”。切换时窗口会重新加载。
- **大小调整**：在“通用 → 桌宠缩放”调整60%—160%。
- **动画与互动**：Q版有待机、工作、停止、睡觉、互动等动画；设置右侧可预览。预览动作不会改变真实开发任务状态。在“交互”里可调整点击反馈、鼠标跟随、互动提示等选项。
- **面板外观**：通用设置中可选玻璃或不透明兼容面板；玻璃效果显示不佳时改为不透明兼容。
- **性能优化**：正比形态下可尝试性能优化，减少渲染负担；不要承诺零占用或所有游戏都不掉帧。
- **休息提醒**：在“休息”页面查看、调整提醒设置。
- **临时隐藏**：右键人物 → “藏起，直到我再次打开”；需要时通过桌面、开始菜单或可用的托盘入口重新显示。
- **自定义展示图**：详情面板支持自定义图片及裁切。它是面板展示图，不等于一键把任意图片变成带动作的桌宠模型。

### 可选的开发工具功能

- 能展示相应开发工具的任务状态提醒、额度及用量信息。实际显示内容依赖工具种类、版本、安装和登录状态。
- 在“设置 → 连接”查看检测结果。**没有安装对应工具时不可用是正常情况，不影响普通桌宠使用。**
- 不要为了让数字出现就擅自安装开发工具、索取API Key、修改用户的开发工具配置或启用集成。用户另行要求接入时，再按实际界面和授权范围配置。
- 此项目的已确认功能不包含通用自由聊天机器人，不要承诺角色能与任意Agent直接对话。

### 建议的最终回复

根据真实结果填入下面的信息；不要把模板当作已经通过的事实：

> 已安装琪亚娜桌宠2.1.4，并已确认〔人物／状态条／托盘〕正常出现。以后可以从桌面或开始菜单打开。
>
> 点小齿轮打开设置：可以切换正比／Q版、调节大小和面板材质；“交互”里能调整点击反馈和鼠标跟随，“休息”里可以设置提醒。右键人物可以暂时藏起。
>
> 额度和开发任务显示是可选功能，需要本机相应工具及登录状态；只想使用桌宠可以先不配置。当前〔已检测到／尚未接入／未检查〕相关工具。
>
> 这是公开测试版，后续通过原发布渠道手动更新。遇到问题可以去项目Issues反馈。

如果只能核验进程、不能观察桌面，改为“安装与进程启动已核验，请确认桌面人物是否出现”，不要报视觉检查通过。

## 6. 数据保护、更新和问题反馈

- 不删除 `%USERPROFILE%\.kiana-codex-pet`，不清理用户图片、偏好、统计和Electron用户配置。
- 不把卸载重装、清空配置、删除旧目录作为默认排障步骤；不改变开机启动、置顶等用户偏好，除非用户要求。
- 更新走同一维护者的发布渠道；先退出目标桌宠，再运行新安装包。当前没有应用内自动更新，不指向上游AgentPaw的更新源。
- 反馈入口：https://github.com/befrica/KianaPet/issues 。先整理版本、Windows版本、形态/缩放/材质、复现步骤及必要局部截图；没有用户明确指令时，不代替用户公开上传日志、图片或发帖。
- 不索取或上传API Key、登录令牌、完整会话、完整用户目录或未经检查的日志。
- 发布范围及已知限制：https://github.com/befrica/KianaPet/blob/main/RELEASE-NOTES.md
- 本项目基于AgentPaw/LLMPET修改，为非官方作品。角色及原始形象相关权利归原权利人；具体软件与定制内容许可见仓库文件，不把整个成品描述为MIT开源。
