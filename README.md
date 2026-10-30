# 心屿 PsyCare · 在线演示站

线上地址：<https://2755715424-byte.github.io/>

这个仓库托管三部分内容：

| 路径 | 内容 | 说明 |
| --- | --- | --- |
| `/` | 心屿 PsyCare 可交互 Demo | 由 Vue 3 构建，所有接口由浏览器内的本地数据层接管 |
| `/about/` | 项目介绍 | 功能、界面、10 套量表、隐私链路、架构与本地运行方式 |
| `/me/` | 作者主页 | 个人介绍与项目入口 |

根路径不是静态介绍页，而是可以直接使用的学生端 / 咨询师端 / 管理端。刷新后仍停在当前 hash 路由，不会 404。

## 演示账号

登录页可一键填入，密码均为 `123456`：

| 账号 | 角色 | 进入 |
| --- | --- | --- |
| `student` | 学生（小满） | 今日概览、测评、AI 助手、树洞、预约与私密对话 |
| `counselor` | 咨询师（林知远） | 工作台、确认预约、与学生对话 |
| `admin` | 平台管理员 | 群体看板、风险预警、资源 / 用户 / 树洞 / 知识库管理 |

也可以不登录，直接完成匿名测评并拿到 `AN-` 开头的报告编号。

## 哪些是真的，哪些是模拟的

真实可交互：

- 10 套量表的完整题目、计分、分级、维度剖面、结果解读与建议。
- 匿名 / 账号两种测评流程，报告生成、历史记录、趋势查询与编号回查。
- 资源库浏览、搜索、详情与基于标签 / 浏览记录的推荐排序。
- 树洞发布、点赞、评论，以及发布前的手机号、证件号、银行卡、邮箱脱敏。
- 预约列表、状态流转、私密对话，以及对话内容自动脱敏。
- 管理端看板的聚合数字、图表、风险预警列表与管理动作。

模拟的部分：

- AI 助手用预置主题回答模拟 SSE 流式输出，回答与来源来自本地资料条目，不会调用真实模型。
- 私密对话中咨询师的回复由本地规则自动生成，不是真人。
- 知识库上传 / 删除 / 重建只修改当前浏览器的统计数据，不会真的切片或写入向量库。
- 所有数据保存在浏览器 `localStorage` 的 `psy_demo_v1` 键中，不会上传到任何服务器。

想恢复初始状态：登录页底部有「重置演示数据」按钮，或在控制台执行 `window.__psyDemo.reset()`。

## 构建

Demo 由主项目的 `frontend/` 构建，输出到本目录：

```bash
cd frontend
npm run build:demo
```

对应 `vite.config.demo.js` + `.env.demo`（`VITE_DEMO=1`），会：

- 使用 hash 路由，刷新任意页面都能命中 `index.html`。
- 输出到 `../personal-site/`，只清理并重写 `app-assets/`，保留 `about/`、`me/`、`assets/`、`.nojekyll`。
- 把 mock 数据层、计分逻辑、SSE / WebSocket 替代实现一起打进产物。

只改了 `about/` 或 `me/` 时不需要重新构建应用。

## 本地预览

在本目录执行：

```bash
python -m http.server 8899
```

打开 <http://localhost:8899/>。不要在 `file://` 下打开，hash 路由与模块脚本需要 HTTP 服务。

## 验证

```bash
# 应用本体：匿名测评、登录、AI 流式回答、资源、树洞、私密对话、管理端、移动端
node tools/uitest/demo-flow.js

# 项目介绍页与作者页：锚点、图片、溢出、控制台错误
node tools/uitest/psycare-site-check.js

# 检查线上地址
SITE_URL=https://2755715424-byte.github.io/ node tools/uitest/demo-flow.js
SITE_URL=https://2755715424-byte.github.io/ node tools/uitest/psycare-site-check.js
```

两个脚本都需要本机 Chrome（默认 `C:/Program Files/Google/Chrome/Application/chrome.exe`，可用 `CHROME_PATH` 覆盖），截图输出到 `tools/uitest/shots/`。

## 发布到 GitHub Pages

仓库名必须是 `<用户名>.github.io`，Pages 从 `main` 分支根目录发布。

```bash
git add .
git commit -m "feat: deploy interactive PsyCare demo"
git push origin main
```

推送后通常 1–2 分钟生效。如果仍是 404，到 **Settings → Pages** 确认 Source 为 `Deploy from a branch`、分支 `main`、目录 `/(root)`。

## 目录结构

```text
personal-site/
├─ index.html               Demo 入口（构建生成）
├─ app-assets/              Demo 的 JS / CSS（构建生成，每次整体替换）
├─ favicon.svg
├─ about/
│  └─ index.html            项目介绍长页
├─ me/
│  └─ index.html            作者主页
├─ assets/                  介绍页与作者页的截图
├─ .nojekyll                关闭 Jekyll 处理
└─ README.md
```

## 说明

- 这是毕业设计的公开演示，不连接 Spring Boot、MySQL 或 Python RAG 服务；需要完整链路时请在本地启动主项目。
- Demo 中的姓名、手机号、邮箱、报告与聊天均为演示数据，页面展示时已做掩码。
- 如果之后要换自定义域名，在仓库根目录加 `CNAME` 文件，并在域名服务商处配置 DNS。
