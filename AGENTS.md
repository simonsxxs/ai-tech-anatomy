# AGENTS.md — AI 技术解剖室 · Agent 接管指南

> 本文件写给未来在任何环境接管本仓库的 AI Agent。人类读者请看 [README.md](./README.md)。
> 首次接管的 Agent：通读本文后，你应能独立完成"新增一篇文档并发布到双平台"的完整流程。

## 一、仓库定位

**AI 技术解剖室**（ai-tech-anatomy）：把 AI 领域的热门技术、安全事件、自动化产品做"人人能懂、证据可复核"的深度拆解，附带实操教程。作者是 **simon（一个不懂代码的 AI 极客）**，文科背景，通过 Vibe Coding 与 Agent 协作完成所有工作——与其协作时用平实中文沟通，不要堆砌术语。

- GitHub：https://github.com/simonsxxs/ai-tech-anatomy
- Gitee（镜像，显示名"AI技术解剖室 ai-tech-anatomy"）：https://gitee.com/simonsxx/ai-tech-anatomy
- 两平台内容必须保持一致，每次改动**双平台都要推**。

## 二、目录结构与命名规范

```
├── README.md            # 首页导读：仓库定位、文档索引、阅读建议、免责声明、署名
├── AGENTS.md            # 本文件
├── 拆解报告/            # 深度拆解类文档，按 NN-中文名 编号
│   └── NN-专题名/
│       ├── 中文文档名.pdf      # 文件名清洗：全角冒号"："→"_"，去掉"(1)"等副本后缀
│       ├── PREVIEW.md          # 在线阅读页（逐页图片 + 锚点）
│       └── pages/              # PDF 逐页渲染的 PNG
└── 教程/                # 实操教程，同样按 NN-中文名 编号，结构同上
```

- 新文档先看同类板块最后一个编号，顺延；编号一经使用不复用。
- 每篇文档一个独立目录，三件套（PDF / PREVIEW.md / pages/）齐全；PREVIEW 命名可带主题后缀（现有 PREVIEW_TECH.md、PREVIEW_WECHAT.md），新篇默认用 PREVIEW.md 即可。

## 三、新增文档的标准流程

以拿到一份新 PDF 为例：

1. **入库**：`cp` 进对应板块目录（拆解报告或教程），按规范清洗文件名。
2. **渲染**：`pdftoppm -png -r 150 <pdf> <目录>/pages/page`（需 poppler；macOS 当前机器已装）。150 DPI 是目前全仓标准，兼顾清晰与体积。
3. **通读内容**：用 `pdftotext` 提取全文和分页标题，确定章节锚点；确认内容已脱敏、含作者署名，主题适合公开。**含敏感信息或非用户原创的内容，先向用户确认再发布。**
4. **写 PREVIEW.md**：参照现有文件的风格——
   - 顶部：标题、一句话定位、PDF 下载链接、快速跳转锚点行
   - 正文：每页一个二级标题（按内容命名）+ 页码标记 `<div align="center">— 第 N 页 —</div>` + 图片
   - 文末：署名块（见第五节）
5. **更新首页 README**：在对应板块按编号加入口（标题、一句话介绍、在线阅读链接、PDF 链接、适合谁读），保持现有排版风格。
6. **提交推送**：`git commit` 后 `git push origin main` + `git push gitee main`。
7. **验证**（见第六节），全部通过后才向用户汇报完成。

## 四、平台与凭据

### GitHub（origin）
- 账号 `simonsxxs`，用 `gh` CLI 操作（当前机器已登录，token scopes 含 repo/delete_repo）。
- 改名/描述：`gh repo rename` / `gh repo edit --description`。
- 注意：GitHub 仓库名**不允许中文字符**，中文名靠简介与 README 呈现。

### Gitee（gitee 远程，SSH 推送）
- 推送地址：`git@gitee.com:simonsxx/ai-tech-anatomy.git`
- API 令牌存在 **macOS 钥匙串**（git credential，host=gitee.com，username=simonsxx），取法：
  ```bash
  printf "protocol=https\nhost=gitee.com\n\n" | git credential fill
  ```
  返回的 `password` 即令牌，**不要写进任何文件或日志**。其他环境没有这把钥匙串时，向用户索取并建议存入该环境的安全存储。

### Gitee API 已知坑（实测踩过，勿重复踩）
1. **创建仓库时 `private=false` 经常不生效**，仓库会建成私有 → 创建后必须再 PATCH 一次并 GET 复查。
2. **PATCH 必须带 `name` 参数**，否则报 `{"messages": ["name is missing"]}`。
3. **PATCH 的 `name` 只改显示名（human_name），改不了 URL path**；path 目前无法通过 API 修改。当前显示名为「AI技术解剖室 ai-tech-anatomy」，路径为 `ai-tech-anatomy`，不要再动。
4. PATCH 偶发返回空响应但实际已生效（也遇到过真失败）→ **任何写操作后一律 GET 复查**，不要信单次响应。
5. 仓库当前为**公开**状态；改可见性、改名、删库属于敏感操作，必须用户明确授权。

## 五、必须遵守的规范

- **署名**：README、每篇 PREVIEW 文末必须带署名块：
  > 作者：simon，一个不懂代码的 AI 极客
  > 微信交流群：`simonsxxs`
  > GitHub：https://github.com/simonsxxs
  > Gitee：https://gitee.com/simonsxx
- **免责声明**：内容定位"仅供安全研究与防御建设"，不收录可直接复用的攻击工具/脚本；涉及灰色的产品拆解必须已脱敏。
- **秘密管理**：Token/密钥不进仓库、不进日志、不进回复；PDF 源文件若来自微信等本地目录，直接 `cp`，不要把私人路径写进仓库文档。
- **介绍区块同步**：README 内的「配套软件」「未公开教程」等介绍区块，必须与私有仓 tech-anatomy-private（[GitHub](https://github.com/simonsxxs/tech-anatomy-private) / [Gitee](https://gitee.com/simonsxx/tech-anatomy-private)）的实际内容保持一致（数量、特性描述、获取引导）；私有仓对应内容有更新时，推送前同步核对公开仓介绍。敏感细节一律只进私有仓，公开仓只放介绍与"联系署名作者获取"引导。
- **跨仓链接双平台并列**：文档里指向另一仓库的链接（如公开仓↔私有仓互相指向），必须同时给出 GitHub 与 Gitee 两个地址并标注平台名——两平台推送的是同一份文件，无法按平台区分链接，读者在哪个平台浏览就点哪个，避免 Gitee 侧访问 GitHub 的网络限制。
- **授权边界**：常规"新增文档 → 推送双平台"是用户已确立的常态授权流程，可直接执行；改名、删库、改可见性、新增第三方内容发布，需当次向用户确认。
- **不擅自扩范围**：不改历史文档的编号与链接结构（外部已分享过链接），除非用户要求。

## 六、发布后验证清单

每次推送后逐项过，全部通过才算完成：

1. 双平台 `git ls-remote` / API 文件树包含新文件；
2. 新增 pages 图片在**两平台匿名访问**（无凭据 curl）均返回 200；
3. README 与 PREVIEW 中的相对链接逐个有效；
4. Gitee 仓库可见性仍为 public（API GET 复查）；
5. 用真实浏览器（如 ego-browser 或用户环境等效工具）打开新 PREVIEW 页，确认图片全部加载、锚点可用；
6. 向用户汇报：新地址、新增内容、验证结果、遗留问题。

## 七、当前文档台账（截至 2026-10-25）

| # | 位置 | 文档 | 页数 |
|---|------|------|------|
| 报告 01 | 拆解报告/01-ARTEX攻击事件全图解 | AI 攻击事件入侵链路全解：架构、链路与防御（图解版） | 18 |
| 报告 02 | 拆解报告/02-ARTEX技术拆解 | AI 驱动渗透框架 ARTEX 技术拆解：从架构到暴露（红队技术版） | 23 |
| 报告 03 | 拆解报告/03-AI控制微信逆向解剖 | AI 是如何控制微信的 · 逆向解剖报告 | 6 |
| 教程 01 | 教程/01-ChatGPT-MCP插件制作教程 | 内部系统接入 ChatGPT：MCP + OAuth 只读插件制作教程 | 7 |
| 教程 02 | 教程/02-自动化招聘AI智能体拆解与复刻 | 拆解一款自动化招聘 AI 智能体（复刻教程） | 39 |
| 教程 03 | 教程/03-系统1比1复刻方法论与工具 | 系统 1:1 复刻：方法论与工具（Markdown 文章，无 PDF） | — |
| 教程 04 | 教程/04-双Agent协作实战教程 | 聪明 Agent 指挥执行 Agent：双 Agent 协作实战教程（图解版 V2.0） | 26 |
| 技能 01 | （已迁至专属仓） | software-decard：软件去卡密自用版 Agent 技能（SKILL.md 加载即用，经白纸实测）——已迁至专属仓 [software-decard（GitHub）](https://github.com/simonsxxs/software-decard) / [Gitee](https://gitee.com/simonsxx/software-decard)，本仓仅保留介绍与链接指引 | — |

> 接管后新增文档请同步更新本表；新增 README 内区块（不占用编号的介绍区块，如配套软件、未公开教程）时，在下文列表中登记一条，并与私有仓实际内容保持同步。

- 配套软件区块：软件 01 招聘自动化智能体 · 自用版（README 内区块，无 PDF 三件套，不占用上表编号）。
- 未公开教程区块：《软件去卡密完全实战教程》45 页（README 内区块，未占用编号，仅介绍不外发，获取引导指向私有仓并联系署名作者）。
- 开源技能区块：技能 01 software-decard 软件去卡密自用版技能（链接指引区块：技能本体在专属仓 software-decard——[GitHub](https://github.com/simonsxxs/software-decard) / [Gitee](https://gitee.com/simonsxx/software-decard) 双平台并列，本仓不存副本）。

## 八、当前维护环境（仅供参考，可能随接管环境变化）

- 本地工作副本（2026-10-12 起实际使用）：`/Users/simon-mac/project/05_skills/ai-tech-anatomy`；历史路径 `/Users/simon-mac/Projects/ai-tech-anatomy` 若仍存在，以 remote 为准
- 渲染工具：pdftoppm（Homebrew poppler）
- 浏览器验证：ego-browser 技能（ego-lite）；无此技能时用等效真实浏览器方案
