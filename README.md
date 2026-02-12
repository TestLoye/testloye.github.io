# Blog | Your Name

欢迎访问 **Your Name's Blog**！这是一个基于 GitHub Pages 的个人博客项目，通过 GitHub Issues 作为后端存储，结合 Cloudflare Workers 处理 OAuth 登录与评论系统。

## 功能特性

* **文章管理**：通过 GitHub Issues 管理博客文章，支持封面、摘要、参考文献和争议标记。
* **互动评论系统**：支持 GitHub 账号登录、实时 Markdown 预览、段落引用（自动关联 #ID）。
* **多节点支持**：支持多个站点节点切换，自动检测网络延迟。
* **增强阅读体验**：内置阅读进度条、自动生成目录 (TOC)、参考文献高亮、文章内链预览。
* **用户交互**：支持全局通知弹窗、键盘快捷键（如 `Esc` 关闭弹窗）。
* **自动化统计**：显示博客运行天数、用户 IP 地址及最新的版本提交记录。

## 文件结构

```
.
├── index.html          # 主入口文件
├── components/         # 模块化页面组件
│   ├── about.html      # 关于页面
│   ├── post.html       # 文章详情页结构
│   └── publish.html    # 发布/反馈页面
├── static/
│   ├── css/            # 样式表 (main, post, markdown 等)
│   └── js/             # 业务逻辑 (post.js 处理评论与渲染)
└── music/              # 本地音乐资源

```

## 部署方法

### 1. 准备 GitHub 仓库

1. 创建一个新的公开仓库（`yourname.github.io`）。
2. 将本项目所有文件上传至仓库。
3. 进入仓库 **Settings > Pages**，将 **Build and deployment** 设置为 `Deploy from a branch`，选择 `main` 分支。

### 2. 配置 GitHub OAuth App (用于评论登录)

1. 前往个人设置 **Settings > Developer settings > OAuth Apps > New OAuth App**。
2. **Homepage URL**: 填写你的博客网址 (例如 `https://yourname.github.io`)。
3. **Authorization callback URL**: 同样填写你的博客网址。
4. 保存后获取 `Client ID` 和 `Client Secret`。

### 3. 部署 Cloudflare Worker (后端中转)

由于 GitHub API 的限制，需要一个中转服务处理登录和评论：

1. 在 Cloudflare 创建一个新的 Worker。
2. 配置 Worker 环境变量：
* `CLIENT_ID`: 你的 OAuth App Client ID。
* `CLIENT_SECRET`: 你的 OAuth App Client Secret。


3. 部署 Worker 代码，并获取分配的 `Worker URL`。

### 4. 修改前端配置

打开 `static/js/post.js` (或相关的配置文件)，修改以下变量：

```javascript
const WORKER_URL = 'https://你的-worker-名称.workers.dev/'; 
const CLIENT_ID = '你的-OAuth-App-ID'; 

```

### 5. 发布文章

1. 在你的 GitHub 仓库中开启 **Issues** 功能。
2. 发表一个新的 Issue：
* **Title**: 文章标题。
* **Labels**: 设置分类标签（如 `MEMO`）。
* **Body**:
* 使用 `[Cover] URL` 指定封面。
* 使用 `[Summary] 内容` 编写摘要。
* 使用 `[Content]` 标记正文开始。





## 主要功能说明

### 1. 评论引用功能

在文章详情页中，鼠标悬停在任意段落上，点击出现的“引用”按钮，系统会自动将内容以 Markdown 格式填充至评论框，并自动附带文章的 `#编号` 以建立双向链接。

### 2. 实时预览

评论框支持即时 Markdown 预览，用户在输入时即可看到最终渲染效果。

## 贡献

欢迎提交 Issue 或 Pull Request 来改进此项目！提交前请确保代码严谨且经过完整测试。

## 许可证

此项目基于 [MIT License](LICENSE) 开源。