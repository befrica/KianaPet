# 版本来源核对

当前准备发布：琪亚娜桌宠 2.1.4，Windows 11 x64 公开测试版，仅当前用户安装。维护者 GitHub 账号为 `befrica`。发布仓库：https://github.com/befrica/KianaPet。百度网盘地址见 README；GitHub Release 状态以仓库页面为准。

1. 从维护者公布的发布页进入下载，不以文件名、头像或版本号单独判断来源。
2. 比对下载文件的 SHA-256 与发布页记录。ZIP 见 ZIP-SHA256.txt；内部安装包见 SHA256SUMS.txt。
3. 2.1.4 安装包 SHA-256：`b3cd1f2aad02b13bb337671818afb365c7c990888c2d2cab11b57452a5a1f5c6`。
4. RELEASE-IDENTITY.json 记录定制构建指纹与许可摘要，可辅助排查混包；它可以被复制或重写，不能代替可信签名。

当前安装包未进行代码签名，未启用应用内在线更新。哈希一致仅说明文件与所比对记录一致；应同时确认记录来自维护者认可的渠道。修改版应标注修改者及来源，不能冒充作者原版。

Windows PowerShell 可运行以下只读命令计算下载 ZIP 的哈希：

```powershell
Get-FileHash -Algorithm SHA256 -LiteralPath '.\KianaPet-2.1.4-Win11-x64-public-test.zip'
```
