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

- 用户明确选择的模块:打字机头部、About Me(重写,突出 AI 与硬核底层工作)、博客 RSS 同步(5 篇)、博客/邮箱/访客徽章。(初版还含 Featured Works、GitHub Stats 全套与贪吃蛇,上线后用户认为内容过多,分两次于 2026-08-09 决定移除并重构,见 §3.2 / §3.4 修订)
- 用户明确排除:Tech Stack shields 徽章行、3D 贡献图、社交统计卡(知乎/LeetCode 等)

## 2. 仓库结构

```
ximing/
├── README.md
└── .github/workflows/
    └── blog-posts.yml     # 博客 RSS 同步
```

- 博客列表直接写回默认分支 README.md
- (snake.yml 与 output 分支已随模块移除)

## 3. README.md 版面(自上而下)

### 3.1 Typing Header

- 工具:`readme-typing-svg.demolab.com`
- 打字机轮播三行(英文为主,第二行于 2026-08-09 随 About Me 重写更新):
  1. `Hi 👋, I'm ximing (席铭)`
  2. `I build AI agents, dev tools & JavaScript engines`
  3. `Coding with curiosity · 一席之地,记录,思考`
- 居中,字号偏大,暗色/亮色各一套颜色参数(走 `<picture>` 双图源)

### 3.2 About Me(2026-08-09 二次修订版)

页面唯一的内容区块,突出两条线:AI 工作 + 有技术壁垒的底层工作(素材来自用户博客 works/blog 目录)。四条 bullet:AI Agent Engineering(v2r-agent / csi / rab)、AI Products(aimo / vane / aipet)、The hard stuff, hand-rolled(jsvm3 字节码虚拟机 / jsvm2 解释器 / 自研二进制协议会话回放 / xexcel 公式引擎)、写作与坐标。(初版结尾有一行歌词签名,用户于 2026-08-09 要求移除)

> 2026-08-09 二次修订:Featured Works 区块移除,其内容并入重写后的 About Me;v2r-agent / vane / aipet / mpe-replay 无公开 GitHub 仓库,文案中作无链接粗体处理。

### 3.3 Featured Works(已移除)

见 §3.2 二次修订。

### 3.4 GitHub Stats(已移除)

> 2026-08-09 二次修订:用户看过实际渲染效果后认为内容过多,决定**整体移除** GitHub Stats 区(stats/streak/top-langs/activity-graph)与贪吃蛇模块(含 snake.yml workflow 和 output 分支)。最终版面只保留:Typing Header、About Me、Latest Blog Posts、Connect(Featured Works 随后亦移除,见 §3.2)。

### 3.5 Snake(已移除)

见 §3.4 的二次修订。模块与配套 workflow、output 分支一并移除。

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

仅存的动态图片均处理明暗:typing svg 用 `<picture>` + `prefers-color-scheme` 双图源(深色/浅色文字参数各一);shields 徽章与访客徽章为自适应配色,无需处理。

> 原 stats/streak/langs/activity-graph/snake 的主题参数表随模块移除作废(见 §3.4 二次修订)。

## 5. 自动化工作流

只保留博客同步一个 workflow(snake.yml 已随模块移除):

### blog-posts.yml

- 触发:`schedule`(每 6 小时,避开整点)、`workflow_dispatch`
- 步骤:`actions/checkout` → `gautamkrishnar/blog-post-workflow@v1`,`feed_list: "https://www.ximing.ren/rss.xml"`,`max_post_count: 5`
- 权限:`contents: write`

### Actions 写权限前置条件

仓库 Settings → Actions → General → Workflow permissions 需为 "Read and write permissions",否则 workflow 提交会 403。部署时通过 `gh api` 检查/设置。

## 6. 容错与降级

- 第三方图片服务(demolab.com / shields.io / komarev.com)宕机只影响单张图片,不影响其它模块 —— 接受该风险,不自托管
- blog-post-workflow 抓取 RSS 失败时不改动 README(该 Action 默认行为),已写入的列表不会被清空
- 首次部署后手动触发各 workflow 一次,确认产物生成,再检查主页渲染

## 7. 验证清单

1. push 后 `gh api repos/ximing/ximing/contents/README.md` 确认文件就位
2. 手动触发 blog-posts.yml,确认 README 的 BLOG-POST-LIST 区间被写入 5 篇文章
3. csi 打开 `https://github.com/ximing` 截图,逐项核对:所有图片可加载、明暗 srcset 均有效、布局无横向滚动、博客列表已填充

## 8. 维护说明

- About Me 中的项目提及为手工维护:新项目要展示需手动编辑 README(设计意图,非缺陷);未公开项目(v2r-agent / vane / aipet 等)只写粗体名字、不加链接
- 博客列表由 blog-posts.yml 自动更新,日常零维护
