# GitHub Profile README 定制设计

- 日期:2026-08-09
- 目标仓库:`ximing/ximing`(GitHub 特殊仓库,README.md 展示在 github.com/ximing 主页)
- 风格路线:平衡丰富型 —— 作品叙事为主(方案 A:创作者叙事),数据卡片为辅,动态模块点缀
- 语言:英文为主,中文点缀
- 参考:知乎《超详细的 GitHub 个人主页美化教程》、abhisheknaiidu/awesome-github-profile-readme、rzashakeri/beautify-github-profile

## 1. 用户画像与设计依据

GitHub 账号 `ximing`(席铭,北京,2013 年注册,230 followers,208 公开仓库):

- **作品主线一:编辑器与实时协作** — fabric-photo(266⭐,Canvas 图片编辑器)、weditor(107⭐,多人协作富文本)、xexcel(浏览器端电子表格)、mdeditor(协作 Markdown 编辑器)
- **作品主线二:JS 虚拟机/解释器** — jsvm3(纯 JS 字节码虚拟机,ES5/ES2015+)、jsvm2(TS 实现的 JS 解释器)
- **作品主线三:AI 产品** — aimo(24⭐,AI 笔记/知识管理,独立产品)、rab(11⭐,AI-first 响应式状态架构)
- 博客 `https://www.ximing.ren`(Gatsby,「一席之地,记录,思考」),RSS 可用:`https://www.ximing.ren/rss.xml`,更新活跃
- 公开邮箱(博客 about 页):`morningxm@hotmail.com`
- Bio 气质:歌词「有太多太多魔力 太少道理」,文艺 + 好奇心

设计决策:

- 用户明确选择的模块:打字机头部、About、Featured Works(策展)、GitHub Stats 全套(stats + streak + top-langs + activity-graph + trophy)、贪吃蛇动画、博客 RSS 同步(5 篇)、博客/邮箱/访客徽章
- 用户明确排除:Tech Stack shields 徽章行、3D 贡献图、社交统计卡(知乎/LeetCode 等)

## 2. 仓库结构

```
ximing/
├── README.md
└── .github/workflows/
    ├── snake.yml          # 贪吃蛇动画生成
    └── blog-posts.yml     # 博客 RSS 同步
```

- 贪吃蛇 SVG 输出到 `output` 分支,不污染默认分支
- 博客列表直接写回默认分支 README.md
- 数据卡片全部为外链图片,无需 workflow

## 3. README.md 版面(自上而下)

### 3.1 Typing Header

- 工具:`readme-typing-svg.demolab.com`
- 轮播三行(英文为主):
  1. `Hi 👋, I'm ximing (席铭)`
  2. `I build editors, collaboration tools & JS engines`
  3. `Coding with curiosity — 一席之地,记录,思考`
- 居中,字号偏大,暗色/亮色各一套颜色参数(走 `<picture>` 双图源)

### 3.2 About Me

英文 bullet 3-4 行 + 结尾一行中文签名:

- 正在构建 aimo —— AI 驱动的笔记与知识管理产品
- 编辑器与实时协作方向:fabric-photo / weditor / xexcel
- 手写 JavaScript 虚拟机:jsvm3 / jsvm2
- 中文签名沿用现有 bio 歌词:「有太多太多魔力,太少道理;太多太多游戏,只是为了好奇」

### 3.3 Featured Works(核心差异模块,手工策展)

三组,每项一行:repo 链接 + shields star 徽章 + 一句话英文描述。

**Editors & Real-time Collaboration**

| 项目 | 描述 |
|------|------|
| fabric-photo | Web-based image editor powered by Canvas(266⭐) |
| weditor | Multi-player collaborative rich-text editor(107⭐) |
| xexcel | Browser spreadsheet: State+Transaction+Plugin architecture, formula engine, xlsx/CSV interop |
| mdeditor | Real-time collaborative Markdown editor |

**JavaScript Engines**

| 项目 | 描述 |
|------|------|
| jsvm3 | Custom bytecode VM in pure JS — ES5/ES2015+ in the browser |
| jsvm2 | A JavaScript interpreter written in TypeScript |

**AI Products**

| 项目 | 描述 |
|------|------|
| aimo | AI-driven note & knowledge management system with vector search(24⭐) |
| rab | AI-first reactive state architecture for React & TypeScript(11⭐) |

star 徽章用 shields.io 动态徽章(`github/stars/ximing/<repo>`),无需手工更新数字。

### 3.4 GitHub Stats

四个卡片,顺序:

1. stats 卡 + streak 连续打卡卡(并排,`<p align>` 或表格布局)
2. top-langs 语言卡(compact 布局)
3. activity-graph 活动曲线图(通栏)

工具:github-readme-stats(经社区镜像 `github-readme-stats-sigma-five.vercel.app`,官方实例 DEPLOYMENT_PAUSED)、DenverCoder1/github-readme-streak-stats、Ashutosh00710/github-readme-activity-graph。

> 2026-08-09 修订:实施时确认官方实例状态 —— github-readme-stats.vercel.app 返回 503 DEPLOYMENT_PAUSED,github-profile-trophy.vercel.app 返回 402 DEPLOYMENT_DISABLED(欠费停用,恢复无期)。用户决策:stats/langs 改用社区镜像 sigma-five,移除 trophy 模块。

### 3.5 Snake

- `Platane/snk/svg-only@v3` 生成 `github-contribution-grid-snake.svg` 与 `github-contribution-grid-snake-dark.svg`
- README 用 `<picture>` 明暗自适应引用 `output` 分支 raw URL

### 3.6 Latest Blog Posts

```
<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->
```

gautamkrishnar/blog-post-workflow 自动写入最近 5 篇,feed:`https://www.ximing.ren/rss.xml`,条目标题带日期。

### 3.7 Connect

一行徽章:

- Blog:`https://img.shields.io/badge/Blog-ximing.ren-...` 链到 `https://www.ximing.ren`
- Email:shields 徽章,`mailto:morningxm@hotmail.com`
- 访客计数:`komarev.com/ghpvc/?username=ximing`(antonkomarev/github-profile-views-counter;不采用 visitor-badge.glitch.me,Glitch 托管已关停)

## 4. 明暗主题适配

所有支持主题参数的图片统一用 `<picture>` + `prefers-color-scheme` 双图源:

| 模块 | 暗色参数 | 亮色参数 |
|------|---------|---------|
| stats / streak / top-langs | `theme=tokyonight` | 默认主题 |
| activity-graph | `theme=tokyo-night` | `theme=github` |
| snake | `github-contribution-grid-snake-dark.svg`(palette=github-dark) | `github-contribution-grid-snake.svg` |
| typing svg | 深色文字参数 | 浅色文字参数 |

## 5. 自动化工作流

### snake.yml

- 触发:`schedule`(每天一次,UTC 低峰非整点,如 `cron: "17 21 * * *"`)、`workflow_dispatch`、`push`(默认分支)
- 步骤:`Platane/snk/svg-only@v3` 生成两张 SVG 到 `dist/` → `crazy-max/ghaction-github-pages@v3.1.0` 推到 `output` 分支
- 权限:`contents: write`(默认 GITHUB_TOKEN,无需 PAT)

### blog-posts.yml

- 触发:`schedule`(每 6 小时,避开整点)、`workflow_dispatch`
- 步骤:`actions/checkout` → `gautamkrishnar/blog-post-workflow@v1`,`feed_list: "https://www.ximing.ren/rss.xml"`,`max_post_count: 5`
- 权限:`contents: write`

### Actions 写权限前置条件

仓库 Settings → Actions → General → Workflow permissions 需为 "Read and write permissions",否则 workflow 提交会 403。部署时通过 `gh api` 检查/设置。

## 6. 容错与降级

- 第三方卡片服务(vercel.app / demolab.com)宕机只影响单张图片,不影响其它模块 —— 接受该风险,不自托管
- blog-post-workflow 抓取 RSS 失败时不改动 README(该 Action 默认行为),已写入的列表不会被清空
- 首次部署后手动触发各 workflow 一次,确认产物生成,再检查主页渲染

## 7. 验证清单

1. push 后 `gh api repos/ximing/ximing/contents/README.md` 确认文件就位
2. 手动触发 snake.yml、blog-posts.yml,确认:output 分支出现两张 SVG;README 的 BLOG-POST-LIST 区间被写入 5 篇文章
3. csi 打开 `https://github.com/ximing` 截图,逐项核对:所有图片可加载、明暗 srcset 均有效、布局无横向滚动、博客列表已填充

## 8. 维护说明

- Featured Works 为手工策展:新项目要展示需手动编辑 README(设计意图,非缺陷)
- 数据卡片、star 徽章、贪吃蛇、博客列表全部自动更新,日常零维护
