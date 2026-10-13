# 护眼配置清单：Windows Terminal 与 Visual Studio

## Windows Terminal（PowerShell）

配色用 Solarized Light，字号 14，行高 1.4，关闭复古效果。设置路径：设置 → 配置文件 → 默认值（或 Windows PowerShell）→ 外观，改完点右下角保存。在"默认值"里改一次，所有配置文件（PowerShell 7、CMD 等）都会生效。

| 项 | 推荐值 | 说明 |
| --- | --- | --- |
| Color scheme | Solarized Light | 米黄底、灰蓝字，列表里对眼睛最温和的一个；觉得偏黄就换 One Half Light |
| Font face | Cascadia Mono 或 Cascadia Code | 两者字形相同，Code 多连字；中文多可装更纱黑体，填 Sarasa Mono SC |
| Font size | 14 | 4K 屏可到 16 |
| Line height | 1.4 | 1.5 更松但每屏行数少，终端里 1.4 是平衡点 |
| Cell width | 0.6 | 保持默认 |
| Font weight | Normal | 浅底上觉得笔画细就改 Medium；不要选 Light 以下 |
| Builtin Glyphs | On | 保持 |
| Full-color Emoji | On | 保持 |
| Retro terminal effects | Off | 发光和扫描线对眼睛最差 |
| Automatically adjust lightness of indistinguishable text | Always | 解决 Solarized 把亮色变灰、部分输出看不清的问题 |
| Cursor shape | Filled box 或 Underscore | 粗光标不用费眼睛找 |
| Opacity | 100% | 不开亚克力和透明 |
| Padding | 10 | 文字离窗口边缘留空 |
| Bell notification style（高级页） | 声音 + 任务栏闪烁 | 配合 Claude Code 的 terminal\_bell 通知 |

备选方案：Solarized 的"亮色"一排映射成了灰色，如果某些工具的输出变成灰字，换 One Half Light，它的亮色仍是正常颜色。晚上要用深色时，列表里只有 One Half Dark 合适。

字体字号的 JSON 写法（设置页左下角"打开 JSON 文件"，放在对应 profile 里）：

```json
"font": {
    "face": "Cascadia Code",
    "size": 14,
    "weight": "normal"
}
```

临时缩放：Ctrl + = 放大，Ctrl + - 缩小，Ctrl + 0 恢复，只对当前窗口有效。

## Claude Code CLI

Claude Code 不管字体和终端配色，这两样由 Windows Terminal 决定。它自己要改的是主题、通知、动画和输出风格四项。

| 项 | 做法 | 作用 |
| --- | --- | --- |
| 主题 | 会话里输入 `/theme`，选 auto 或 light | auto 自动跟随终端的浅色/深色背景 |
| 通知 | settings.json 里设 `preferredNotifChannel: "terminal_bell"` | Windows Terminal 默认收不到桌面通知，改用终端铃声；配合终端的"声音 + 任务栏闪烁" |
| 动画 | settings.json 里设 `prefersReducedMotion: true` | 关掉 spinner 和渐变光效，减少余光里的持续运动 |
| 输出风格 | 会话里输入 `/output-style`，选 Concise | 解释文字变短，减少每天要读的量 |
| 全屏渲染 | 画面闪烁或滚动跳动时输入 `/tui fullscreen` | 没有闪烁可以不开 |

settings.json 的位置：`C:\Users\你的用户名\.claude\settings.json`，没有就新建，两台电脑各放一份：

```json
{
  "preferredNotifChannel": "terminal_bell",
  "prefersReducedMotion": true
}
```

如果觉得 diff 的红绿底色刺眼，可以做一个自定义主题调淡。文件放在 `C:\Users\你的用户名\.claude\themes\soft-light.json`，保存后 `/theme` 里就能选到：

```json
{
  "name": "soft-light",
  "base": "light",
  "overrides": {
    "diffAdded": "#e6f4ea",
    "diffRemoved": "#fce8e6"
  }
}
```

改完在会话里输入 `/config` 可以确认各项状态。来源：[Configure your terminal for Claude Code](https://code.claude.com/docs/en/terminal-config)、[Accessibility settings](https://code.claude.com/docs/en/accessibility)。

## Visual Studio

主题装 Theme Pack 后选 Solarized Light，字体 Cascadia Code，字号 14，和终端对齐。Visual Studio 和 Windows Terminal 的字号单位都是磅，数字一样大小就一样，没有换算关系。

| 步骤 | 路径 | 设置 |
| --- | --- | --- |
| 1. 整体主题 | 工具 → 选项 → 环境 → 常规 → 颜色主题 | 先选"浅色"或"使用系统设置"（跟随 Windows 浅色模式） |
| 2. 装柔和主题 | 扩展 → 管理扩展，搜索 Visual Studio Theme Pack（微软官方），装完重启 | 颜色主题下拉里选 Solarized Light |
| 3. 字体字号 | 工具 → 选项 → 环境 → 字体和颜色，"显示其设置"选"文本编辑器" | 字体 Cascadia Code 或 JetBrains Mono，大小 14 |
| 4. 单项颜色 | 同一页下方"显示项"列表 | "纯文本"控制正文颜色和编辑器背景；注释、关键字、字符串可单独调前景色 |

行距说明：Visual Studio 的行距比终端紧，同样 14 号字代码会显得密一些。先用 14 看一周，觉得密再降到 13；如果你的版本在 工具 → 选项 → 文本编辑器 里有行距选项，调到 1.3 左右。

想用 Theme Pack 里没有的主题，同一作者的 Visual Studio Theme Converter 可以把任意 VS Code 主题转过来。菜单和面板的字体不在这里调，用 Windows 的显示缩放。

如果用的是 VS Code：Ctrl+K 再 Ctrl+T 打开主题选择器，Solarized Light 是内置的；字号在设置里搜 `editor.fontSize`，下面这段放进 settings.json：

```json
{
  "editor.fontFamily": "JetBrains Mono",
  "editor.fontSize": 15,
  "editor.lineHeight": 1.5,
  "editor.minimap.enabled": false,
  "editor.cursorBlinking": "solid",
  "diffEditor.renderSideBySide": false,
  "workbench.colorCustomizations": {
    "diffEditor.insertedTextBackground": "#2ea04320",
    "diffEditor.removedTextBackground": "#f8514920",
    "diffEditor.insertedLineBackground": "#2ea04310",
    "diffEditor.removedLineBackground": "#f8514910"
  }
}
```

## Windows 系统设置

这四项对终端、Visual Studio、Claude 桌面端和浏览器全部生效，两台电脑都要设。

| 项 | 路径 | 设置 |
| --- | --- | --- |
| 浅色模式 | 设置 → 个性化 → 颜色 → 选择模式 | 浅色。Claude 桌面端默认跟随系统会一起变浅；Windows Terminal 的配色是单独指定的，不受影响 |
| 夜间模式 | 设置 → 系统 → 屏幕 → 夜间模式 | 开启并设置计划，下午 4 点到次日早上，强度 30% 到 50%。只改色温不改对比度 |
| 光标闪烁 | 控制面板 → 键盘 → 光标闪烁速度 | 拖到"无"。Windows Terminal 的光标跟随这个设置 |
| 显示缩放 | 设置 → 系统 → 屏幕 → 缩放 | Claude 桌面端没有字号设置，字小就把这块屏的缩放调高一档 |

### 屏幕亮度

换浅色主题后亮度要重新调：黑底时屏幕大部分面积不发光，同样的亮度值换成浅色后进光量会多好几倍，原来的档位一定偏亮。别按百分比猜，每台显示器的百分比对应的实际亮度不同，用白纸当标尺。

1. 拿一张白纸放在屏幕旁边，从 30% 开始，每次加 5% 到 10%，每档停一两分钟。
2. 每档看一眼屏幕上的白，再看一眼纸，再看回来。屏幕明显比纸暗就继续加，差不多了就停。
3. 停下的位置就是这个房间白天的亮度。多数人在亮房间落在 40% 到 60%，晚上降到 20% 到 35%。

合适的标准：白底像一张纸，字是清晰的黑灰色，不发灰、不发光、不用眯眼。太暗的表现是白底发灰、字边缘发虚；太亮的表现是眯眼、看久了想躲。

注意事项：外接显示器用机身按键调，Windows 的亮度滑块经常不管用；显示器菜单里的对比度保持默认，只调亮度；两块屏幕调到一样。如果加到 60% 以上才够，说明环境光太强，通常是窗户在屏幕正前方或正后方，先挪桌子或拉窗帘。

## 用眼习惯

配色和亮度是硬件部分，做完之后剩下的都是习惯，效果比配色大。按性价比排序。

| 项 | 做法 | 说明 |
| --- | --- | --- |
| 休息节律 | 20 分钟计时器，响了看 6 米外 20 秒；每小时离座 5 分钟 | 最重要的一条。Agent 在跑的时候就是休息时间，闭眼或看窗外，不盯流式输出 |
| 眨眼 | 每读完一屏有意识地完整眨几下 | 专注读屏时眨眼从每分钟 15 到 20 次掉到 5 次，是干涩的主因；屏幕边贴便签提醒 |
| 干眼护理 | 无防腐剂人工泪液每天 2 到 4 次；湿度 40% 到 60%；空调风不对脸；晚上热敷 10 分钟 | 下午那次泪液最重要 |
| 距离和姿势 | 屏幕 50 到 70 厘米，上沿与视线齐平或略低；靠背靠住 | 看代码容易前倾，一前倾距离就从 60 厘米变 40 厘米；副屏与主屏夹角不超过 30 度 |
| 减少阅读量 | Concise 输出、按风险分三档评审、Agent 先自审、固定时段集中评审 | 眼睛工作量真正降下来靠这些 |
| 睡眠和户外 | 睡够 7 小时；每天 30 分钟以上在户外看远处 | 看远才是"看绿色护眼"的正确版本，起作用的是距离不是颜色 |
| 记录和复查 | 每天下班给眼睛不适打 1 到 5 分，两周后回看 | 两三周没改善去眼科查干眼、屈光和调节功能，必要时配 60 到 70 厘米的电脑眼镜 |

## Agent 编程工作法

核心一句话：没有 Agent 能自己跑的检查时，你就是验证环节，每个错误都要等你发现。眼睛的负担来自这里。正确的用法是把自己从验证环节挪到规格和检查的设计上。按重要程度排。

| 原则 | 做法 | 为什么 |
| --- | --- | --- |
| 先给检查再给任务 | 提示里直接写验收条件：测试用例、构建通过、截图对比。要证据不要结论：测试输出、运行的命令和返回、结果截图 | 有可运行的检查，Agent 自己迭代到通过；看证据比重跑验证快，对没在看的会话也管用 |
| 探索、计划、写、提交 | 大功能用计划模式；更大的让 Agent 先采访你，把规格写到 SPEC.md，再开新会话执行。一句话能描述的 diff 跳过计划 | 把规格写精确的时间，比盯实现过程的时间回报高 |
| 狠管上下文 | 不相关任务之间 /clear；同一问题纠正两次还不对就 /clear 重来，用更具体的提示；调研交给子代理，只收摘要 | 上下文填满后性能下降，失败的尝试会污染后续输出 |
| 评审交给另一个 Agent | Writer/Reviewer 两个会话，或子代理对照 PLAN.md 评审 diff：需求是否都实现、边界是否有测试、有无越界改动，只报缺口不报风格 | 新上下文不偏向刚写的代码；缺口直接回到实现会话，不用你搬运 |
| 提示具体到文件和模式 | 写症状、可能位置、"修好"的标准；加功能时指向现有的参照实现 | 越精确纠正越少 |
| CLAUDE.md 要短 | 每行问一句"删了会出错吗"，不会就删；必须发生的动作用 hook 不写在 CLAUDE.md | 文件太长重要规则会被忽略；hook 是确定性的，CLAUDE.md 是建议性的 |
| 并行数对齐评审能力 | 2 到 3 个并发，固定时段集中评审 | 瓶颈在你不在机器，每多一个 Agent 就多一份要读的文字 |

评审者的坑：被要求找缺口的 Agent 总会报出一些，哪怕工作没问题，追着每条改会过度设计。告诉它只标记影响正确性或既定需求的缺口，其余算可选。

改法：把"写方案 → 派任务 → 读全部产出"改成下面这个循环。

1. 写规格，同时定义检查（测试、构建、截图）。
2. 派任务，Agent 跑的时候不看。
3. 子代理或第二个会话评审。
4. 看证据和缺口清单。
5. 只对高风险改动（安全、数据、并发、对外接口、不可逆操作）看 diff。

常见失败模式：一个会话塞多个无关任务；反复纠正同一问题；CLAUDE.md 过长；没有验证就信任；无范围的"调查一下"把上下文读满。

来源：[Best practices for Claude Code](https://code.claude.com/docs/en/best-practices)。

## 检查清单

两台电脑各过一遍，做完打勾。

- [ ] Windows Terminal：默认值 → 外观，按上表设好，保存
- [ ] Windows Terminal：高级页，铃声通知样式勾上声音和任务栏闪烁
- [ ] Claude Code：`/theme` 选 auto 或 light
- [ ] Claude Code：settings.json 放好，含 terminal\_bell 和 prefersReducedMotion
- [ ] Claude Code：`/output-style` 选 Concise
- [ ] Visual Studio：装 Theme Pack，选 Solarized Light
- [ ] Visual Studio：字体 Cascadia Code，字号 14
- [ ] Windows：浅色模式
- [ ] Windows：夜间模式计划开启
- [ ] Windows：光标闪烁设为无
- [ ] 两块屏幕亮度、色温一致，白纸测试做过
