# Obsidian + Claude AI 个人知识库完整搭建指南

> 基于 [Karpathy LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) 方法论 | 4 阶段 12 步 | 30 分钟搭好 | 不用写代码

---

## 这份指南能帮你干什么

**怎么用：** 把仓库里的 `CLAUDE.md` 复制到你的 Obsidian vault 根目录，按下面的步骤走一遍就行。或者直接把这份 README 扔给 AI 助手，让它一步步帮你配好目录结构、规则文件、索引和日志——你跟着确认就行。

一句话：**让 AI 帮你管笔记，你只管往里扔素材和提问。**

你看的文章、链接、截图等内容，扔进去。AI 自动整理成一本你的私人百科全书。以后想查什么，问它，它从你自己的笔记里找答案，不是从网上瞎编。

- 预计搭建时间：30-60 分钟（包含安装所有东西）
- 日常使用：每次 1-3 分钟（扔素材进去就行）

---

## 适合谁用

**核心用户：** 已经在用 Obsidian 记笔记，或者愿意用的人。不需要会写代码，但需要愿意折腾 30 分钟把环境搭好。

**特别适合：**
- 每天看大量内容但记不住的人
- 笔记越来越多但找不到的人
- 想让 AI 帮忙整理知识但不知道怎么开始的人

**不适合：**
- 只想用手机记笔记的（Obsidian 电脑端体验远好于手机）
- 不愿意装软件、注册账号的
- 笔记量极少，每月不超过 5 条的

---

## 为什么是这套方案

市面上知识管理工具很多。选这套组合的理由很简单：

**Obsidian** 是目前最适合做个人知识库的笔记软件。文件存在你自己电脑上，不怕哪天服务商跑了。Markdown 格式，10 年后换别的工具也能打开。

**Claudian** 是让 Claude 直接住进你的 Obsidian vault 里的插件。不是那种聊天框贴过去再贴回来的用法，是 AI 直接能读你的笔记、写新笔记、改旧笔记。你的 vault 就是它的工作目录。

**Karpathy 方法论** 是底层逻辑。Andrej Karpathy（OpenAI 创始团队成员）提了一个理念：知识应该被「编译」一次，然后持续更新。不是每次提问都从头翻原始资料，而是 AI 帮你维护一本随时可查的百科。

三个东西加一起 = 你有了一个 AI 知识管理员。你负责扔素材和提问，它负责整理、归类、交叉引用、定期体检。

---

## 全局流程图

```
安装 Obsidian → 装 Claudian 插件 → 注册 Claude 账号
    ↓
配置 CLAUDE.md（知识库规则文件）→ 建好目录结构
    ↓
日常使用三个动作：
  扔素材（ingest） → AI 自动整理成 wiki 文章
  提问（query）    → AI 从你的笔记里找答案
  体检（lint）     → AI 检查知识库健康状态
```

---

## 你需要准备什么

| 东西 | 用途 | 怎么搞 | 费用 |
|------|------|--------|------|
| 一台电脑 | Mac 或 Windows 都行 | 你应该已经有了 | - |
| Obsidian | 笔记软件，你的知识库载体 | [obsidian.md](https://obsidian.md) 下载 | 免费 |
| Claudian 插件 | 让 Claude 住进 Obsidian | Obsidian 社区插件市场 | 免费 |
| Claude 账号 | AI 引擎 | [claude.ai](https://claude.ai) 注册 | Pro $20/月 或 Max $100/月 |
| Claude Code | 命令行工具（Claudian 的引擎） | 终端里装 | 包含在 Pro/Max 里 |

> **费用说明：** Pro（$20/月）可用但有用量上限，重度使用可能撞限速。Max（$100/月）用量宽裕很多。建议先用 Pro 试跑，如果频繁被限速再升级 Max。Obsidian 和 Claudian 都免费。

---

## 阶段一：安装环境

> 目标：把所有软件装好，能跑起来

### Step 1：安装 Obsidian

去 [obsidian.md](https://obsidian.md) 下载对应你系统的版本，装好打开。

打开后会问你要不要创建新 vault。选「Create new vault」，名字随便起，比如叫 `workspace` 或 `knowledge-base`。位置选你常用的文件夹就行。

> **常见坑：** vault 的存放路径（从盘符到 vault 文件夹这一段）里不要有中文和空格，比如 `/Users/你的用户名/Obsidian/workspace`。vault 内部的文件夹（比如 `raw/AI工作流/`）用中文没问题，两者不冲突。

**完成标记：** Obsidian 打开了，能看到一个空的 vault

### Step 2：安装 Claude Code

Claude Code 是命令行工具，Claudian 插件需要它作为引擎。

**Mac 用户：**

打开终端（在启动台搜「终端」或「Terminal」），粘贴这行：

```bash
npm install -g @anthropic-ai/claude-code
```

如果提示 `npm: command not found`，说明你没装 Node.js。先去 [nodejs.org](https://nodejs.org) 下载 LTS 版本装好，再回来跑上面那行。

**Windows 用户：**

打开 PowerShell，同样的命令：

```bash
npm install -g @anthropic-ai/claude-code
```

装完之后在终端输入 `claude --version`，能看到版本号就说明装好了。

> **常见坑：** 很多人卡在 Node.js 这步。如果你之前从来没装过 Node.js，去 nodejs.org 下载 **LTS 版本**（不是 Current），一路下一步就行。装完重启终端再试。

> **常见坑：** 第一次运行 `claude` 命令会要求你登录 Anthropic 账号。跟着提示走就行，浏览器会弹出来让你授权。

**完成标记：** 终端里输入 `claude --version` 能看到版本号

### Step 3：安装 Claudian 插件

回到 Obsidian：

1. 左下角齿轮图标 → Settings
2. 左侧菜单 Community Plugins → 点「Turn on community plugins」（如果是第一次）
3. 点「Browse」→ 搜索「Claudian」
4. 找到 Claudian（作者 Yishen Tu），点 Install → Enable

装好之后左侧应该会多出一个 Claudian 的图标。

> **常见坑：** Claudian 只支持桌面端。手机上看不到这个插件是正常的。

> **常见坑：** 如果搜不到 Claudian，检查一下你的 Obsidian 版本。需要 v1.4.5 以上。左下角 Settings → About 可以看版本号。

**完成标记：** Obsidian 左侧栏能看到 Claudian 图标，点开能看到输入框

### Step 4：配置 Claudian

进入 Settings → Community Plugins → Claudian（齿轮图标），看到设置面板。需要关注的：

**Setup 区：**
- **CLI Path** — Claude Code 的路径。通常自动检测，如果提示找不到，把 Step 2 里装好的 `claude` 路径填进去（Mac 上一般是 `/usr/local/bin/claude`，不确定的话终端输 `which claude` 查看）

**Safety 区：**
- **Safe Mode** — 下拉选 `acceptEdits`（推荐）。AI 每次修改文件前会让你确认。另一个选项 `default` 会更频繁地弹权限确认

**Models 区：**
- Opus 1M / Sonnet 1M 两个开关，默认关。刚开始用不需要开，这是扩展上下文窗口用的，对应更高用量消耗

模型选择不在设置里，而是在 Claudian 对话框顶部——打开对话框后能看到模型下拉菜单，可以切换 Haiku / Sonnet / Opus。日常用 Sonnet 够了，遇到复杂任务再切 Opus。

> **常见坑：** 第一次打开 Claudian 对话框，可能会卡一下，那是在初始化 Claude Code 进程。等 30 秒再试。

**完成标记：** 在 Claudian 对话框里发一条指令，Claude 能正常响应

---

## 阶段二：搭建知识库结构

> 目标：建好目录和规则文件，让 AI 知道怎么帮你管知识

### Step 5：创建目录结构

在你的 vault 根目录下，建这几个文件夹：

```
你的vault/
├── raw/          ← 原始素材（AI 只读，不改）
├── wiki/         ← AI 维护的知识库
└── assets/       ← 配图资源
```

在 Obsidian 里建文件夹很简单：左侧文件列表空白处右键 → New folder

然后在 `wiki/` 里建两个文件：

- `wiki/index.md` — 全局索引，先写个标题 `# Knowledge Base Index` 就行
- `wiki/log.md` — 操作日志，先写个标题 `# Operation Log` 就行

> **关键概念：** `raw/` 文件夹是「只读」的。意思是你往里扔素材，但 AI 绝不会改里面的文件。这是故意设计的，保证你的原始资料永远不被篡改。

**完成标记：** vault 里能看到 `raw/`、`wiki/`、`assets/` 三个文件夹，`wiki/` 里有 `index.md` 和 `log.md`

### Step 6：创建 CLAUDE.md（最核心的一步）

这是整个系统最核心的文件。`CLAUDE.md` 放在 vault 根目录，它告诉 AI：你的知识库结构是什么样的、遇到不同指令该怎么做。

**直接把本仓库的 [`CLAUDE.md`](CLAUDE.md) 文件复制到你的 vault 根目录。** 或者在 vault 根目录新建一个文件叫 `CLAUDE.md`，把模板内容粘进去。

这是精简版。你用起来之后可以慢慢往里加规则。

> **常见坑：** CLAUDE.md 的文件名必须是全大写的 `CLAUDE.md`，不是 `claude.md`。AI 启动时会自动找这个文件。

> **常见坑：** 别一开始就写特别复杂的规则。先用这个简版跑起来，等你理解了每个触发行为是怎么工作的，再根据自己的需要改。

**完成标记：** vault 根目录有 `CLAUDE.md` 文件，内容包含三个触发行为的定义

### Step 7：建第一个主题目录

在 `raw/` 和 `wiki/` 下各建一个主题文件夹。主题就是你最常接触的知识领域。

比如你是做 AI 工作流的，可以建：

```
raw/AI工作流/
wiki/AI工作流/
```

或者你对多个领域感兴趣：

```
raw/AI工作流/
raw/产品设计/
raw/一人公司/
wiki/AI工作流/
wiki/产品设计/
wiki/一人公司/
```

主题目录不用一次建完。以后 ingest 新素材的时候，AI 会自动建需要的目录。

**完成标记：** 至少有一个主题目录建好了

---

## 阶段三：日常使用

> 目标：掌握三个核心操作，让知识库跑起来

### Step 8：扔素材进去（Ingest）

这是你最常用的操作。看到好文章、好视频笔记、好截图，扔进去让 AI 帮你消化。

**操作方法：**

打开 Claudian 对话框，说：

```
把这个加到 wiki：

[粘贴文章内容 / 拖入文件 / 贴链接]
```

或者更简单：

```
ingest 这个：

[素材内容]
```

**AI 会做这几件事：**
1. 把原始素材存到 `raw/` 对应目录
2. 把内容消化成 wiki 文章，放到 `wiki/` 对应目录
3. 如果 wiki 里已经有相关文章，它会合并而不是重复建
4. 更新 `wiki/index.md` 的索引
5. 在 `wiki/log.md` 里记一笔

**素材形式不限：**

| 素材类型 | 怎么扔 |
|---------|-------|
| 文章全文 | 直接复制粘贴到对话框 |
| 网页链接 | 贴 URL，AI 会尝试抓取内容。如果抓不到（部分网站有反爬），手动复制正文粘进来 |
| PDF 文件 | 拖到 raw/ 里，然后告诉 AI「消化这个 PDF」 |
| 视频笔记 | 把你的笔记或转录文字粘进去 |
| 截图 | 拖进 vault，告诉 AI 看一下 |

> **常见坑：** 第一次 ingest 可能要等 1-2 分钟，因为 AI 要读你整个 wiki 的结构才能决定放哪。后面会越来越快。

> **常见坑：** 不要一次扔太多素材。一次一篇文章或一个主题。AI 处理完一个再扔下一个，质量更好。

> **常见坑：** 如果 AI 建了一个你觉得不合适的目录或文件名，别忍着。直接告诉它「这个文章应该放在 XX 目录下」或者「文件名改成 XX」。它会照做并更新所有引用。

**完成标记：** wiki/ 里出现了新的 md 文件，index.md 里多了对应条目

### Step 9：向知识库提问（Query）

知识库最大的价值不是「存」而是「用」。

**操作方法：**

在 Claudian 对话框里问：

```
我的 wiki 里关于 XX 有什么？
```

或者：

```
根据我的 wiki，总结一下 XX
```

又或者对比分析：

```
对比一下 A 和 B 的区别，根据我的笔记
```

AI 会先查 index.md 定位相关文章，再深入读取，最后合成答案。答案里会引用具体的 wiki 页面链接，你可以点过去看原文。

**默认行为：** query 只在对话里回答，不会写新文件。

**想存下来？** 加一句「存下来」或「归档到 wiki」，AI 就会把答案写成一篇新的 wiki 文章。

> **常见坑：** 如果 AI 说「没找到相关内容」，不一定是你没存过。可能是 index.md 没更新。跑一次 lint（下一步）通常能解决。

**完成标记：** AI 能从你的 wiki 内容里合成答案，并引用具体页面

### Step 10：定期体检（Lint）

知识库用久了会有各种小问题：文章写了但索引没收、链接指向被删的文件、同一个概念在两篇文章里说法矛盾。

**操作方法：**

```
lint wiki
```

或者中文：

```
体检
```

**AI 会检查：**
- 索引里有但文件不存在的 → 标记 `[MISSING]`
- 文件存在但索引里没有的 → 补进去
- 内部链接失效的 → 自动修复或报告
- 孤立页面（没任何文章引用它的）→ 报告出来
- 可能的事实矛盾 → 报告出来让你判断

能自动修的它直接修，不确定的报告给你决定。

> **建议频率：** 每周跑一次 lint。知识库文章超过 20 篇之后，链接问题会慢慢冒出来。

**完成标记：** lint 跑完，log.md 里能看到体检记录

---

## 阶段四：进阶用法

> 目标：让知识库从「能用」变成「好用」

### Step 11：自定义 CLAUDE.md 规则

用了一两周之后，你会发现需要给 AI 一些更具体的规则。比如：

**按你的领域定制主题分类：**

```markdown
## 我的主题分类

- AI工作流 — AI 相关的工具、方法、工作流
- 产品设计 — 产品思维、设计方法论、用户体验
- 一人公司 — 创业、商业模式、IP 构建
```

**定义你的命名偏好：**

```markdown
## 命名约定

- wiki 文章用中文命名，描述性的，比如「AI视频工作流」而不是「2026-04-13-video」
- raw 里的文件保持原名不改
```

**加入交叉引用规则：**

```markdown
## 交叉引用

写新文章时，检查是否和已有文章有关联。有就在文末加 See Also
```

> **常见坑：** CLAUDE.md 的规则写得越清晰，AI 的行为越可预测。但别写成论文，300 行以内最好。规则太多 AI 反而容易搞混。

**完成标记：** CLAUDE.md 里有你自己加的规则，AI 的行为符合你的预期

### Step 12：知识库的复合增长

Karpathy 方法论最核心的一个观点：**知识是复合增长的，不是线性堆砌的。**

什么意思？

- **第 1 篇文章**，它就是一篇文章
- **第 10 篇文章**，文章之间开始产生交叉引用
- **第 50 篇文章**，你问一个问题，AI 能从 7-8 篇文章里综合出一个你自己都没想到的答案
- **第 100 篇文章**，你的知识库比你自己的记忆都好用

所以关键是**持续 ingest**。看到好东西就扔进去，哪怕只是一段话、一个观点。AI 会负责把碎片连成网络。

> **最大的坑不是技术问题，是坚持问题。** 很多人搭好了但一周就不用了。建议：把 ingest 变成一个习惯，比如每天下午花 5 分钟把今天看到的好东西扔进去。

**完成标记：** 你的 wiki 文章数量在稳定增长，文章之间有交叉引用

---

## 质量自检

搭完之后过一遍这个清单：

- [ ] Obsidian 能正常打开 vault
- [ ] 在 Claudian 对话框里发指令，Claude 能正常响应
- [ ] vault 根目录有 `CLAUDE.md`
- [ ] 有 `raw/`、`wiki/`、`assets/` 三个目录
- [ ] `wiki/` 里有 `index.md` 和 `log.md`
- [ ] 能成功 ingest 一篇素材（wiki/ 里出现新文章）
- [ ] 能成功 query 一个问题（AI 从 wiki 内容里回答）
- [ ] 能成功跑一次 lint

全部打勾，你的 AI 知识库就搭好了。

---

## FAQ

**Q：Claude Pro $20/月值不值？Max $100/月呢？**

至少需要 Pro 才能用 Claude Code（Claudian 的引擎），这是硬门槛。Pro 每天 ingest 1-2 篇素材、偶尔 query 基本够用。如果你发现经常被限速（提示「usage limit reached」），说明用量上来了，这时候升 Max 是值得的。

**Q：我已经有很多 Obsidian 笔记了，能直接用吗？**

可以。把现有笔记当素材，分批 ingest 进去。建议不要一次全扔，一天处理一个主题的，慢慢来。

**Q：手机上能用吗？**

Obsidian 有手机版，但 Claudian 只支持桌面端。手机上可以查看和编辑笔记，但不能跑 AI 操作。建议：手机上用 Obsidian 随手记碎片，回到电脑上再 ingest。

**Q：数据安全吗？会不会被上传？**

Obsidian 的文件存在你自己电脑上，不经过任何云端。Claudian 调用 Claude 时，会把相关文件内容发给 Claude API 处理，但不会永久存储在 Anthropic 的服务器上。敏感信息（比如密码、API Key）不要放进 vault。

**Q：Claude 的回答不准确怎么办？**

两种情况。一是你的 wiki 里确实没有相关内容，那就需要先 ingest 更多素材。二是 AI 理解有偏差，这时候你可以在 CLAUDE.md 里加规则约束它，或者在对话里纠正它。

**Q：和 Notion AI、语雀 AI 有什么区别？**

最大区别：数据在你手里。Notion 和语雀的 AI 功能依赖他们的服务器，他们关了你就没了。Obsidian + CLAUDE.md 这套方案，你换任何 AI 服务都能继续用，因为底层就是 Markdown 文件。

**Q：CLAUDE.md 写错了会不会把笔记搞乱？**

有可能，但有兜底。如果规则写得不对，AI 可能会把文章放错目录或者错误合并内容。所以 Step 4 里建议把 Safe Mode 设为 `acceptEdits`——AI 每次改文件前都会让你确认，你觉得不对就拒绝，它会重新来。万一真搞乱了，Obsidian 文件就是普通 Markdown，手动改回来就行。

**Q：知识库能多大？有上限吗？**

Obsidian 本身没有文件数量限制，几千篇文章都没问题。AI 的限制在于每次对话的上下文窗口，它通过 index.md 做索引定位，不需要一次读完所有文件。所以知识库越大，index.md 越重要。

---

## 相关资源

- [Karpathy LLM Wiki 原始 gist](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) — 方法论原文
- [Astro-Han/karpathy-llm-wiki](https://github.com/Astro-Han/karpathy-llm-wiki) — 社区 skill 实现
- [Claudian 插件](https://github.com/YishenTu/claudian) — Obsidian 嵌入 Claude Code
- [Claude Code 高阶指南](https://github.com/helloianneo/claude-code-handbook) — 深入理解 Claude Code 的工作原理

---

## 关于作者

**Ian (伊恩)** — 产品设计师 / 一人公司实践者 / AI Builder

用 AI 团队打造一人公司。这份指南是我个人知识库实践的完整沉淀。

- X/Twitter: [@ianneo_ai](https://x.com/ianneo_ai)
- 网站: [ianneo.xyz](https://ianneo.xyz)
- 微信: 17855813746
- 邮箱: hello.neoc@gmail.com

> **🚀 正在开启：内容创作丨AI 工作流围观营，欢迎来玩！** 加微信了解详情。

---

## 贡献

发现错误或有改进建议？欢迎提 [Issue](https://github.com/helloianneo/obsidian-ai-second-brain/issues)。

觉得有用？Star 一下方便后续更新时收到通知。

---

## License

[MIT](LICENSE)
