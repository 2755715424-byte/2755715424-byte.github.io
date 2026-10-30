# 心屿 PsyCare · 展示站

一个不需要构建步骤的静态站点：手写 HTML / CSS / 少量 JavaScript，直接托管到 GitHub Pages。

线上地址：<https://2755715424-byte.github.io/>

站点由两个页面组成：

| 路径 | 内容 |
| --- | --- |
| `/` | 心屿 PsyCare 产品页：功能、界面、10 套量表、隐私链路、架构、本地运行、作者 |
| `/me/` | Elnera 的个人主页，从产品页页头与页尾的「作者主页」进入 |

产品页是纯展示页：平台需要本地启动后端（Spring Boot 8080 + Vue 5173），页面不含登录入口，也不含真实用户数据。

## 本地预览

在本目录执行：

```bash
python -m http.server 8899
```

然后打开 <http://localhost:8899>。产品页在 `/`，个人页在 `/me/`。图片和样式都在本目录内，不依赖外部 CDN 或字体服务。

## 验证

`tools/uitest/psycare-site-check.js` 用本机 Chrome 检查两个页面：标题、导航锚点跳转是否被 sticky 头部遮挡、图片是否加载、桌面/390/320 三档是否横向溢出、作者页图片与返回链接、控制台与网络错误，并输出整页截图。

```bash
# 仓库根目录
node tools/uitest/psycare-site-check.js
# 检查线上地址（截图加 live- 前缀）
SITE_URL=https://2755715424-byte.github.io/ node tools/uitest/psycare-site-check.js
```

## 发布到 GitHub Pages

### 方式 A：用户主页（当前使用）

1. 仓库名必须是 `<用户名>.github.io`；当前用户名是 `2755715424-byte`，仓库名是 `2755715424-byte.github.io`。
2. 把本目录下的全部文件放到仓库根目录（`index.html`、`me/`、`assets/`、`.nojekyll`）。
3. 推送到 `main` 分支：

   ```bash
   git init
   git add .
   git commit -m "feat: site"
   git branch -M main
   git remote add origin https://github.com/2755715424-byte/2755715424-byte.github.io.git
   git push -u origin main
   ```

4. 用户主页仓库会自动发布，通常 1–2 分钟后可访问 `https://2755715424-byte.github.io/`。如果首页仍是 404，到 **Settings → Pages** 确认 Source 为 `Deploy from a branch`、分支为 `main`、目录为 `/(root)`。

### 方式 B：项目主页

适合仓库叫别名的场景，地址形如 `https://<用户名>.github.io/<仓库名>/`。步骤与方式 A 相同，仓库名换成实际名字即可；站内资源全部使用相对路径，放在子路径下也能正常加载。

## 上线前要改的地方

- `index.html` 的 `<title>`、`meta description`、`og:title`、`og:description`。
- 产品页里的项目数字（量表 10 套、102 个切片、21 条边界用例、k=5）与「本地运行」命令，改动代码后要同步。
- 作者页 `me/index.html` 中的展示名 `Elnera`、个人经历与联系方式；GitHub 链接已指向 `https://github.com/2755715424-byte`。
- 三张截图（`assets/`）：替换同名文件即可，页面里写了 `width`/`height`，换图时同步尺寸。
- 如果暂不公开邮箱，可以只保留 GitHub 入口；确定要公开时再加 `mailto:` 链接。

## 目录结构

```text
personal-site/
├─ index.html              心屿 PsyCare 产品页
├─ me/
│  └─ index.html           Elnera 个人主页
├─ assets/
│  ├─ psycare-home.webp      公共门户
│  ├─ psycare-assess.webp    学生空间 · 心理测评
│  ├─ psycare-report.webp    测评报告
│  └─ psycare-assistant.webp AI 心理助手
├─ .nojekyll               告诉 GitHub Pages 不要用 Jekyll 处理
└─ README.md
```

## 说明

- 页面不加载 Google Fonts、图标库或前端框架，国内访问更稳，也更少依赖。
- 断点：1080px（功能卡片与量表列数）、980px（主视觉单列）、860px（导航换行）、720px（手机排版）、380px（更窄的屏幕）。
- 产品页只做展示，不承诺线上可用：所有数字与截图来自本地演示环境。
- 如果之后要换自定义域名，在仓库根目录加一个 `CNAME` 文件，内容写域名，并在域名服务商处配置 DNS。
