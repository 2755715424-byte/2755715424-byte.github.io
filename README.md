# Elnera · 个人主页

一个不需要构建步骤的单页个人网站：手写 HTML / CSS / 少量 JavaScript，直接托管到 GitHub Pages。

## 本地预览

在本目录执行：

```bash
python -m http.server 8899
```

然后打开 <http://localhost:8899>。图片和样式都在本目录内，不依赖外部 CDN 或字体服务。

## 发布到 GitHub Pages

### 方式 A：个人主页（推荐）

适合地址就是 `https://<用户名>.github.io/` 的情况。

1. 在 GitHub 新建一个公开仓库，仓库名必须是 `<用户名>.github.io`；当前用户名是 `2755715424-byte`，所以仓库名是 `2755715424-byte.github.io`。
2. 把本目录下的全部文件放到仓库根目录（`index.html`、`assets/`、`.nojekyll`）。
3. 推送到 `main` 分支：

   ```bash
   git init
   git add .
   git commit -m "feat: personal site"
   git branch -M main
   git remote add origin https://github.com/2755715424-byte/2755715424-byte.github.io.git
   git push -u origin main
   ```

4. 打开仓库的 **Settings → Pages**，在 **Build and deployment** 中选择：
   - Source: `Deploy from a branch`
   - Branch: `main`，目录选 `/(root)`
   - 保存后等待 1-2 分钟，访问 `https://2755715424-byte.github.io/`。

### 方式 B：项目主页

适合仓库叫 `personal-site`、地址为 `https://<用户名>.github.io/personal-site/` 的情况。

步骤与方式 A 相同，仓库名换成 `personal-site` 即可。站内资源全部使用相对路径，放在子路径下也能正常加载。

## 上线前要改的地方

- `index.html` 的 `<title>`、`meta description`、`og:title`、`og:description`。
- 首页名字 `Elnera` 是展示名；GitHub 链接已指向 `https://github.com/2755715424-byte`。如需改名，同时替换标题、简介和页脚。
- 三个项目卡片中的项目名、描述、技术标签和截图（替换 `assets/` 下的图片即可）。
- “关于我”“现在”里的个人经历、方向和联系方式。
- 如果暂不公开邮箱，可以只保留 GitHub 入口；确定要公开时再加 `mailto:` 链接。

## 目录结构

```text
personal-site/
├─ index.html          页面结构、样式与交互
├─ assets/
│  ├─ psycare-home.webp
│  ├─ psycare-report.webp
│  └─ psycare-assistant.webp
├─ .nojekyll           告诉 GitHub Pages 不要用 Jekyll 处理
└─ README.md
```

## 说明

- 页面不加载 Google Fonts、图标库或前端框架，国内访问更稳，也更少依赖。
- 已处理 980px 与 720px 两个断点，手机端单列展示。
- 如果之后要换自定义域名，在仓库根目录加一个 `CNAME` 文件，内容写域名，并在域名服务商处配置 DNS。
