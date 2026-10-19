# 英语单词本 · 词根版（GitHub Pages 站点）

一个**零依赖、零后端**的英语背单词单页应用，直接托管在 GitHub Pages 上即可使用。

**特点**

- **不预装词汇**：词库完全由你自己录入，数据只存在浏览器 localStorage（不联网、不上传）。
- **词根标签**：内置 182 个医学 / 药学 / 通用词根（同义拼写已合并，`cardi-` 与 `cardio-` 视为同一词根），支持自动补全。
- **自定义词根**：自己添加任意词根与其同义拼写，和内置词根完全等价。
- **分组 / 按词根分组 / 批量处理**。
- **背诵**：卡片翻转，正面显示词根提示，答案面给出词根释义。
- **测验**：若考的词带词根，**所有选项都取自含同一词根的词**（可跨分组）；也支持看英语选汉语 / 看汉语选英语。
- **导入导出**：JSON（含复习进度）与 CSV。

## 目录结构

```
.
├── index.html            # 应用本体（单文件，内联 CSS/JS）
├── favicon.svg           # 站点图标
├── manifest.webmanifest  # 安装到桌面/主屏用的 Web App Manifest
├── robots.txt
├── LICENSE
├── README.md
└── .github/
    └── workflows/
        └── deploy-pages.yml   # 可选：用 Actions 自动发布 Pages
```

## 部署到 GitHub Pages（二选一）

### 方式 A：不用 Actions，直接发布（最简单）

1. 在 GitHub 新建一个**空仓库**（不要勾选初始化 README）。
2. 把这个目录里的文件（**含隐藏文件 `LICENSE`、`robots.txt`**）上传到仓库根目录。
   - 网页端：`Add file → Upload files`，把 `index.html`、`favicon.svg`、`manifest.webmanifest`、`robots.txt` 一起拖上去，Commit。
   - 命令行：
     ```bash
     git init
     git add .
     git commit -m "英语单词本 · 词根版"
     git branch -M main
     git remote add origin https://github.com/<你的用户名>/<仓库名>.git
     git push -u origin main
     ```
3. 仓库 **Settings → Pages**：
   - **Source** 选 `Deploy from a branch`
   - **Branch** 选 `main`，目录选 `/ (root)`
   - 保存，等 1–2 分钟。
4. 访问：`https://<你的用户名>.github.io/<仓库名>/`
   - 如果仓库名就叫 `<你的用户名>.github.io`，则直接访问 `https://<你的用户名>.github.io/`。

### 方式 B：用 Actions 自动发布

仓库里已带 `.github/workflows/deploy-pages.yml`：

1. 推到 `main` 后，进入 **Settings → Pages**，把 **Source** 改成 `GitHub Actions`。
2. 之后每次 push 到 `main` 都会自动重新发布，构建日志在 **Actions** 标签页。
3. 首次运行可能需要在 **Settings → Actions → General** 里允许 workflow 写入（默认通常是允许的）。

> 两种方式二选一即可。已经用方式 A 的，把 Source 改成 Actions 前记得保持一致，避免两条发布链互相覆盖。

## 本地预览

直接双击 `index.html` 即可（`file://` 也能用）。
想更接近线上环境，可以起一个静态服务器：

```bash
# Python 3
python -m http.server 8000
# 或 Node
npx serve .
```

然后打开 `http://localhost:8000/`。

> 注意：`file://` 与 `http://` 属于不同来源，**localStorage 不互通**。本地已录入的数据想带到线上，请用应用内的「导出备份」，再到线上站点「导入」。

## 数据说明（重要）

- 词库保存在**访问者自己浏览器**的 localStorage（键名 `en_vocab_trainer_v1`），不会上传到 GitHub，也不会被站点看到。
- 换设备 / 换浏览器 / 清缓存会导致数据丢失，请用「导出备份」定期保存 JSON。
- 本站是纯静态页面，没有服务端，也不收集任何用户数据。

## 更新站点

改完 `index.html` 后重新 push 即可（Actions 方式）或重新上传覆盖（分支方式）。也可以把最新的 `index.html` 直接覆盖进这个目录再推。

## 许可

MIT，见 [LICENSE](LICENSE)。你可以自由使用、修改、再发布。

## 技术说明

- 单文件实现，无构建步骤、无第三方依赖、无 CDN 请求。
- 兼容现代浏览器（Chrome / Edge / Firefox / Safari），支持离线（页面本身可被浏览器缓存；未注册 Service Worker，因此不保证完全离线可用）。
- 响应式布局，手机上可用。
