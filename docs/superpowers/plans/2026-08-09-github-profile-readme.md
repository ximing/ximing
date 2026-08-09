# GitHub Profile README 实现计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 为 `ximing/ximing` 特殊仓库实现定制化 GitHub 个人主页(README + 2 个自动化 workflow),并部署验证上线。

**Architecture:** README.md 为静态 Markdown + 外链动态图片(数据卡片/打字机/徽章);两个 GitHub Actions 分别生成贪吃蛇 SVG(输出到 `output` 分支)和同步博客 RSS 到 README。全部内容遵循 spec:`docs/superpowers/specs/2026-08-09-github-profile-readme-design.md`。

**Tech Stack:** Markdown、GitHub Actions(Platane/snk、gautamkrishnar/blog-post-workflow)、shields.io、github-readme-stats 系列外链服务、gh CLI。

## Global Constraints

- 目标仓库:`ximing/ximing`,本地路径 `/Users/ximing/project/mygithub/ximing`,默认分支 `master`,remote `origin` 已配置
- README 语言:英文为主,中文点缀;不放 Tech Stack shields 徽章行(spec 明确排除)
- 所有支持主题的图片必须用 `<picture>` + `prefers-color-scheme` 明暗双图源;暗色主题参数:stats/streak/langs 用 `tokyonight`,activity-graph 用 `tokyo-night`,snake 用 `palette=github-dark`
- stats / top-langs 卡片使用社区镜像 `https://github-readme-stats-sigma-five.vercel.app/`(官方实例 DEPLOYMENT_PAUSED);trophy 模块已移除(官方实例 DEPLOYMENT_DISABLED)—— 2026-08-09 用户决策
- 访客徽章用 `https://komarev.com/ghpvc/?username=ximing`,**禁止**使用 visitor-badge.glitch.me(Glitch 托管已关停)
- 博客 RSS:`https://www.ximing.ren/rss.xml`,显示 5 篇
- 联系邮箱:`morningxm@hotmail.com`,博客:`https://www.ximing.ren`
- workflow 权限:默认 `GITHUB_TOKEN` + `contents: write`,不创建 PAT
- 定时任务 cron 避开整点与 :30(snake 用 `17 21 * * *`,blog 用 `23 */6 * * *`)
- 提交信息结尾带 `Co-Authored-By: Claude <noreply@anthropic.com>`

---

### Task 1: README.md 静态内容

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: 无(首个任务)
- Produces: `README.md` 含 `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->` 标记对(Task 2 的 workflow 依赖这对标记);`master` 分支上的 README 是 Task 3 追加 snake 区块的基础

- [ ] **Step 1: 编写 README.md**

完整内容如下,原样写入 `README.md`:

````markdown
<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://readme-typing-svg.demolab.com?font=Fira+Code&size=26&duration=3000&pause=800&color=70A5FD&center=true&vCenter=true&width=800&lines=Hi%2C+I%27m+ximing+(%E5%B8%AD%E9%93%AD);I+build+editors%2C+collaboration+tools+%26+JS+engines;Coding+with+curiosity+%C2%B7+%E4%B8%80%E5%B8%AD%E4%B9%8B%E5%9C%B0%2C+%E8%AE%B0%E5%BD%95%2C+%E6%80%9D%E8%80%83">
  <source media="(prefers-color-scheme: light)" srcset="https://readme-typing-svg.demolab.com?font=Fira+Code&size=26&duration=3000&pause=800&color=0969DA&center=true&vCenter=true&width=800&lines=Hi%2C+I%27m+ximing+(%E5%B8%AD%E9%93%AD);I+build+editors%2C+collaboration+tools+%26+JS+engines;Coding+with+curiosity+%C2%B7+%E4%B8%80%E5%B8%AD%E4%B9%8B%E5%9C%B0%2C+%E8%AE%B0%E5%BD%95%2C+%E6%80%9D%E8%80%83">
  <img alt="Hi, I'm ximing" src="https://readme-typing-svg.demolab.com?font=Fira+Code&size=26&duration=3000&pause=800&color=0969DA&center=true&vCenter=true&width=800&lines=Hi%2C+I%27m+ximing+(%E5%B8%AD%E9%93%AD);I+build+editors%2C+collaboration+tools+%26+JS+engines;Coding+with+curiosity+%C2%B7+%E4%B8%80%E5%B8%AD%E4%B9%8B%E5%9C%B0%2C+%E8%AE%B0%E5%BD%95%2C+%E6%80%9D%E8%80%83">
</picture>

</div>

### 👋 About Me

- 🔭 Building **[aimo](https://github.com/ximing/aimo)** — an AI-driven note & knowledge management product
- 📝 Long-time maker of editors & real-time collaboration tools (canvas / rich-text / spreadsheet)
- 🌱 Hand-rolling JavaScript engines for fun — bytecode VMs & interpreters in pure TS/JS
- 📍 Beijing · ✍️ Writing at [一席之地,记录,思考](https://www.ximing.ren)

> 有太多太多魔力,太少道理;太多太多游戏,只是为了好奇。

### ⭐ Featured Works

**Editors & Real-time Collaboration**

- [fabric-photo](https://github.com/ximing/fabric-photo) ![stars](https://img.shields.io/github/stars/ximing/fabric-photo?style=flat-square) — Web-based image editor powered by Canvas
- [weditor](https://github.com/ximing/weditor) ![stars](https://img.shields.io/github/stars/ximing/weditor?style=flat-square) — Multi-player collaborative rich-text editor
- [xexcel](https://github.com/ximing/xexcel) ![stars](https://img.shields.io/github/stars/ximing/xexcel?style=flat-square) — Browser spreadsheet: State + Transaction + Plugin architecture, formula engine, xlsx/CSV interop
- [mdeditor](https://github.com/ximing/mdeditor) ![stars](https://img.shields.io/github/stars/ximing/mdeditor?style=flat-square) — Real-time collaborative Markdown editor

**JavaScript Engines**

- [jsvm3](https://github.com/ximing/jsvm3) ![stars](https://img.shields.io/github/stars/ximing/jsvm3?style=flat-square) — Custom bytecode VM in pure JS, running ES5/ES2015+ in the browser
- [jsvm2](https://github.com/ximing/jsvm2) ![stars](https://img.shields.io/github/stars/ximing/jsvm2?style=flat-square) — A JavaScript interpreter written in TypeScript

**AI Products**

- [aimo](https://github.com/ximing/aimo) ![stars](https://img.shields.io/github/stars/ximing/aimo?style=flat-square) — AI-driven note & knowledge management with vector search
- [rab](https://github.com/ximing/rab) ![stars](https://img.shields.io/github/stars/ximing/rab?style=flat-square) — AI-first reactive state architecture for React & TypeScript

### 📊 GitHub Stats

<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats-sigma-five.vercel.app/api?username=ximing&show_icons=true&theme=tokyonight&hide_border=true">
  <img alt="ximing's GitHub stats" src="https://github-readme-stats-sigma-five.vercel.app/api?username=ximing&show_icons=true&hide_border=true">
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://streak-stats.demolab.com?user=ximing&theme=tokyonight&hide_border=true">
  <img alt="GitHub streak" src="https://streak-stats.demolab.com?user=ximing&hide_border=true">
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-stats-sigma-five.vercel.app/api/top-langs/?username=ximing&layout=compact&theme=tokyonight&hide_border=true">
  <img alt="Top languages" src="https://github-readme-stats-sigma-five.vercel.app/api/top-langs/?username=ximing&layout=compact&hide_border=true">
</picture>
<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://github-readme-activity-graph.vercel.app/graph?username=ximing&theme=tokyo-night&hide_border=true">
  <img alt="Activity graph" src="https://github-readme-activity-graph.vercel.app/graph?username=ximing&theme=github&hide_border=true">
</picture>

</div>

### 📝 Latest Blog Posts

<!-- BLOG-POST-LIST:START -->
<!-- BLOG-POST-LIST:END -->

### 📫 Connect

[![Blog](https://img.shields.io/badge/Blog-ximing.ren-663399?style=flat-square&logo=rss&logoColor=white)](https://www.ximing.ren)
[![Email](https://img.shields.io/badge/Email-morningxm%40hotmail.com-0078D4?style=flat-square&logo=microsoftoutlook&logoColor=white)](mailto:morningxm@hotmail.com)
![Visitors](https://komarev.com/ghpvc/?username=ximing&style=flat-square&color=663399)
````

注意:snake 区块**故意不在本任务加入**,因为 `output` 分支尚不存在,图片会是裂图;Task 3 生成产物后再追加。

- [ ] **Step 2: 验证所有外链图片 URL 可访问**

把 README 中所有 `src` / `srcset` / shields 图片 URL 逐个 curl,全部应返回 200 且 `content-type` 为 image:

```bash
cd /Users/ximing/project/mygithub/ximing
grep -oE 'https://[^")]+' README.md | grep -vE '^(https://github.com/ximing|https://www.ximing.ren|mailto)' | sort -u | while read -r u; do
  code=$(curl -sL -o /dev/null -w "%{http_code}" --max-time 20 "$u")
  echo "$code $u"
done
```

Expected: 每行都以 `200` 开头(typing-svg、github-readme-stats ×2、streak-stats、activity-graph、github-profile-trophy、img.shields.io ×10、komarev)。若有 4xx/5xx,修正对应 URL 参数后重跑。

- [ ] **Step 3: 自查 README 结构**

确认:`<!-- BLOG-POST-LIST:START -->` 与 `<!-- BLOG-POST-LIST:END -->` 标记成对存在且拼写精确(workflow 靠它定位,错一个字符就会失败);无 Tech Stack 徽章行;无 snake 引用。

```bash
grep -c 'BLOG-POST-LIST' README.md   # Expected: 2
grep -c 'github-contribution-grid-snake' README.md   # Expected: 0
```

- [ ] **Step 4: Commit**

```bash
cd /Users/ximing/project/mygithub/ximing
git add README.md
git commit -m "feat: profile README 静态内容(typing header/about/featured works/stats/connect)

Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

### Task 2: 博客 RSS 同步 workflow + 首次推送上线

**Files:**
- Create: `.github/workflows/blog-posts.yml`
- Modify: 无 README 改动(复用 Task 1 的标记对)

**Interfaces:**
- Consumes: Task 1 README 中的 `<!-- BLOG-POST-LIST:START -->` / `<!-- BLOG-POST-LIST:END -->` 标记对
- Produces: 远端 `master` 分支(Task 3 的 workflow 和 push 依赖它);README 博客区块被 Action 自动写入 5 篇文章

- [ ] **Step 1: 编写 workflow**

写入 `.github/workflows/blog-posts.yml`:

```yaml
name: Latest Blog Posts
on:
  schedule:
    - cron: "23 */6 * * *"
  workflow_dispatch:
permissions:
  contents: write
jobs:
  update-readme-with-blog:
    name: Update README with latest blog posts
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: gautamkrishnar/blog-post-workflow@v1
        with:
          feed_list: "https://www.ximing.ren/rss.xml"
          max_post_count: 5
```

- [ ] **Step 2: Commit 并推送到 GitHub(首次推送,主页上线)**

```bash
cd /Users/ximing/project/mygithub/ximing
git add .github/workflows/blog-posts.yml
git commit -m "ci: 博客 RSS 同步 workflow

Co-Authored-By: Claude <noreply@anthropic.com>"
git push -u origin master
```

Expected: push 成功;`gh api repos/ximing/ximing --jq .default_branch` 返回 `master`。

- [ ] **Step 3: 开启 Actions 写权限**

```bash
gh api -X PUT repos/ximing/ximing/actions/permissions/workflow \
  -f default_workflow_permissions=write
gh api repos/ximing/ximing/actions/permissions/workflow --jq .default_workflow_permissions
```

Expected: 输出 `write`。

- [ ] **Step 4: 手动触发 workflow 并等待完成**

```bash
gh workflow run "Latest Blog Posts" --repo ximing/ximing
sleep 15
gh run list --repo ximing/ximing --workflow "Latest Blog Posts" --limit 1
```

若状态不是 `success`,用 `gh run view --repo ximing/ximing --log-failed` 查日志修复(常见原因:RSS 不可达、标记拼写不匹配)。等到 `success` 再继续。

- [ ] **Step 5: 验证 README 被写入 5 篇文章**

```bash
gh api repos/ximing/ximing/contents/README.md -H "Accept: application/vnd.github.raw" | sed -n '/BLOG-POST-LIST:START/,/BLOG-POST-LIST:END/p'
```

Expected: START/END 之间有 5 行 `- [标题](链接)` 格式的文章列表。若为空但 workflow success,检查标记拼写。

---

### Task 3: 贪吃蛇 workflow + README snake 区块

**Files:**
- Create: `.github/workflows/snake.yml`
- Modify: `README.md`(在 `### 📊 GitHub Stats` 之后、`### 📝 Latest Blog Posts` 之前插入 snake 区块)

**Interfaces:**
- Consumes: Task 2 的远端 `master` 分支、已开启的 Actions 写权限
- Produces: `output` 分支上的 `github-contribution-grid-snake.svg` / `github-contribution-grid-snake-dark.svg`,README 通过 `https://raw.githubusercontent.com/ximing/ximing/output/...` 引用

- [ ] **Step 1: 编写 workflow**

写入 `.github/workflows/snake.yml`:

```yaml
name: Generate Snake
on:
  schedule:
    - cron: "17 21 * * *"
  workflow_dispatch:
  push:
    branches: [master, main]
jobs:
  generate:
    permissions:
      contents: write
    runs-on: ubuntu-latest
    timeout-minutes: 5
    steps:
      - name: Generate snake SVGs
        uses: Platane/snk/svg-only@v3
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg
            dist/github-contribution-grid-snake-dark.svg?palette=github-dark
      - name: Push SVGs to output branch
        uses: crazy-max/ghaction-github-pages@v3.1.0
        with:
          target_branch: output
          build_dir: dist
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

注意:本 workflow 的 `push` 触发会在本次 push 时自动运行一次,但为保证确定性,Step 4 仍手动触发并等待。

- [ ] **Step 2: 先 push workflow 并触发,生成 output 分支**

```bash
cd /Users/ximing/project/mygithub/ximing
git add .github/workflows/snake.yml
git commit -m "ci: 贪吃蛇贡献图 workflow

Co-Authored-By: Claude <noreply@anthropic.com>"
git push
gh workflow run "Generate Snake" --repo ximing/ximing
sleep 20
gh run list --repo ximing/ximing --workflow "Generate Snake" --limit 1
```

等到最近一次运行 `status: completed, conclusion: success`。失败则 `gh run view --repo ximing/ximing --log-failed` 排查。

- [ ] **Step 3: 验证 output 分支产物**

```bash
gh api repos/ximing/ximing/contents?ref=output --jq '.[].name'
curl -sL -o /dev/null -w "%{http_code}\n" https://raw.githubusercontent.com/ximing/ximing/output/github-contribution-grid-snake.svg
curl -sL -o /dev/null -w "%{http_code}\n" https://raw.githubusercontent.com/ximing/ximing/output/github-contribution-grid-snake-dark.svg
```

Expected: 列出两张 SVG;两个 URL 都返回 `200`。此时再改 README,避免裂图。

- [ ] **Step 4: README 插入 snake 区块**

在 `README.md` 中,把这一段插在 `</div>`(GitHub Stats 区块结尾)与 `### 📝 Latest Blog Posts` 之间:

```markdown

### 🐍 Contribution Snake

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/ximing/ximing/output/github-contribution-grid-snake-dark.svg">
  <source media="(prefers-color-scheme: light)" srcset="https://raw.githubusercontent.com/ximing/ximing/output/github-contribution-grid-snake.svg">
  <img alt="github contribution grid snake animation" src="https://raw.githubusercontent.com/ximing/ximing/output/github-contribution-grid-snake.svg">
</picture>
```

- [ ] **Step 5: Commit 并 push**

```bash
cd /Users/ximing/project/mygithub/ximing
git add README.md
git commit -m "feat: 贪吃蛇贡献图区块

Co-Authored-By: Claude <noreply@anthropic.com>"
git push
```

注意:这次 push 会再次触发 snake workflow(push 触发器),属预期行为,无需等待。

---

### Task 4: 端到端验证

**Files:**
- 无文件改动(纯验证)

**Interfaces:**
- Consumes: Task 1-3 的全部产物(远端 README、output 分支、两个 workflow 的成功运行记录)
- Produces: 验证报告(截图 + 检查结果)

- [ ] **Step 1: 远端内容核对**

```bash
gh api repos/ximing/ximing/contents/README.md -H "Accept: application/vnd.github.raw" | grep -cE 'BLOG-POST-LIST|github-contribution-grid-snake|readme-typing-svg|github-readme-stats|streak-stats|activity-graph|github-profile-trophy|komarev'
```

Expected: 输出 ≥ 12(各模块关键 URL 均在远端 README 中)。

- [ ] **Step 2: workflow 健康检查**

```bash
gh run list --repo ximing/ximing --limit 5
```

Expected: "Latest Blog Posts" 与 "Generate Snake" 最近一次运行 conclusion 均为 `success`。

- [ ] **Step 3: csi 截图核对渲染效果**

用 csi(session:`github-profile-verify`,新标签页)打开 `https://github.com/ximing`,等页面加载后截图保存,用 Read 查看截图,逐项核对 spec §7 验证清单:

1. 打字机头部显示三行轮播文案(截图只能捕获其中一行,有动画即正常)
2. About Me、Featured Works 三组 8 个项目链接 + star 徽章显示
3. 四张数据卡片(stats/streak/langs/activity-graph)全部渲染、无裂图
4. 贪吃蛇动画渲染
5. 博客列表 5 篇文章(标题为中文属正常)
6. Connect 行:Blog / Email / Visitors 徽章
7. 页面无横向滚动条

若有裂图:对该图片 URL 单独 curl 诊断;若是第三方服务限流(stats 类常见),等几分钟后刷新重试,仍失败则向用户报告,不擅自改设计。

- [ ] **Step 4: 向用户报告**

输出验证结果摘要 + 截图要点 + 主页链接 `https://github.com/ximing`,提示用户可在浏览器切换明/暗主题确认双图源生效。
