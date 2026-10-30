# 琪亚娜 Codex 桌宠

本地定制基于 AgentPaw v2.1.1（源码提交 f063e239cedc62a38b37ec6f279ef1103f8fe151）。
上游：https://github.com/vista-zhangg/agentpaw
原上游：https://github.com/myunwang/LLMPET
保留根目录 MIT LICENSE、上游作者与 assets/ 中的独立素材署名。

琪亚娜形象是基于《崩坏3》琪亚娜·卡斯兰娜官方“月咏星谣”皮肤形象进行的非官方二次改造与再设计，不应表述为完全原创或与官方形象无关的全新二创角色；角色及原始形象相关权利归原权利人。用户提供的官方脸部截图用于眼瞳、睫毛和鼻部结构参考，局部稿由内置 ImageGen 编辑后配准到用户确认的全身图；没有加入官方角色音频。旧版图像记录位于 assets/kiana/GENERATION.md。

本次 Live2D 版采用用户指定聊天中最终确认的白紫色全身立绘。原图及后续局部生成素材的来源、提示词和原像素保留方式见项目 live2d/GENERATION.md。先前白橙 Q 版素材作为旧版本保留。

模型由 PSD2Live 3.6.0 制作，包含眼睛开合、小幅头部 XY、轻微侧倾、呆毛及轻微呼吸。播放器暂时固定独立眼球移动；v22 修复头颈接合后启用 Z ±2。PSD2Live 为独立 GPL-3.0 制作工具，不作为桌宠运行依赖捆绑。编辑源与运行模型分别保存。

v24 增加四个独立发片（17 层模型）与桌宠控制器中的阻尼跟随。可见发丝沿用已确认原图，隐藏领口与服装后的发丝使用内置 ImageGen 补画；生成记录见项目 live2d/art-review/头发拆层_生成记录.md。自定义跟随由本桌宠 kiana-hair-follow.js 驱动，模型参数可供其他播放器另行绑定。

运行端使用 Live2D Cubism Core for Web 5.1.0、pixi-live2d-display 0.4.0、PixiJS 6.5.10 和 @pixi/unsafe-eval 6.5.10。保留 live2d-vendor 中的上游版权、许可文件和脚本许可声明；Core 与 Framework 遵循各自的 Live2D 条款，不能由桌宠 MIT 许可替代。Core 的获取来源为 https://cubism.live2d.com/sdk-web/cubismcore/live2dcubismcore.min.js 。没有接入 AI 对话或语音模型。

额度仅通过本机 codex app-server 查询；认证由 Codex 自身管理。本定制默认关闭其他 Agent hooks、自动更新、参考费用常驻显示及休息提醒；配置与历史使用独立的 ~/.kiana-codex-pet 目录。
