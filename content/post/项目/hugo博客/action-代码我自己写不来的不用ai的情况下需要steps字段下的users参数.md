---
title: hugo博客action代码部署：action代码解析
date: 2025-02-20T20:14:22+08:00
draft: false
categories:
  - 项目部署
collections:
  - action为了部署项目 其代码有哪些关键部分
tags:
  - 部署项目的action代码
series:
  - 项目部署
---

# Hugo博客的action代码：解析用aciton配置hugo博客的action代码

好的，这是视频中展示的、由社区贡献者 (peaceiris) 维护的用于通过 GitHub Actions 部署 Hugo 站点的 Actions 工作流 YAML 代码：

```yaml
# 工作流名称
name: deploy

# 触发工作流的事件
on:
  push:
    branches:
      # 仅在 main 分支被推送时触发
      - main

# 定义工作流中的作业
jobs:
  # 定义一个名为 deploy 的作业
  deploy:
    # 指定作业运行的环境为最新的 Ubuntu 系统
    runs-on: ubuntu-latest
    # 定义作业中的步骤
    steps:
      # 步骤1：检出代码
      - name: Checkout
        # 使用官方的 actions/checkout 动作来获取仓库代码
        uses: actions/checkout@v4
        with:
          # 获取完整的 Git 历史记录（对于部署工具 peaceiris/actions-gh-pages 可能需要）
          # 0 表示获取所有历史，而不是默认的浅克隆（只获取最后一次提交）
          fetch-depth: 0

      # 步骤2：设置 Hugo 环境
      - name: Setup Hugo
        # 使用社区维护的 peaceiris/actions-hugo 动作来安装 Hugo
        uses: peaceiris/actions-hugo@v3
        with:
          # 指定要安装的 Hugo 版本为最新版
          hugo-version: 'latest'
          # 明确指定安装 Hugo 的 Extended 版本（支持 SASS/SCSS 等特性）
          extended: true

      # 步骤3：构建网站
      - name: Build Web
        # 运行 Hugo 命令来生成静态网站文件
        # -D 参数表示包含草稿内容（如果本地预览用过，这里保持一致，或按需移除）
        run: hugo -D

      # 步骤4：部署网站到 GitHub Pages
      - name: Deploy Web
        # 使用社区维护的 peaceiris/actions-gh-pages 动作来部署
        uses: peaceiris/actions-gh-pages@v4
        with:
          # 指定用于认证的 Personal Access Token (PAT)
          # ${{ secrets.TOKEN }} 会从仓库的 Secrets 中读取名为 TOKEN 的值
          # （视频中占位符是 你的token变量名，这里假设你设置为 TOKEN）
          PERSONAL_TOKEN: ${{ secrets.TOKEN }}
          # 指定要将静态文件部署到的目标仓库
          # 格式为：你的GitHub用户名/目标仓库名
          # （视频中占位符是 你的github名/你的仓库名，这里假设目标仓库是 letere-gz/hugo-dev）
          EXTERNAL_REPOSITORY: letere-gz/hugo-dev
          # 指定将静态文件推送到目标仓库的哪个分支（通常是 gh-pages 或 main）
          PUBLISH_BRANCH: main
          # 指定包含构建好的静态网站文件的目录（Hugo 默认生成到 public）
          PUBLISH_DIR: ./public
          # 自定义部署时 Git 提交的信息
          commit_message: auto deploy
```

**代码解释 (遵循托勒斯法则/Law of Demeter - 关注“做什么”而非“怎么做”的内部细节):**

1.  **`name: deploy`**
    *   **作用**: 给这个自动化流程（工作流）起个名字，叫做 `deploy`。这个名字会显示在 GitHub 仓库的 "Actions" 标签页里，方便你识别。
    *   **托勒斯法则**: 你只需要知道这是流程的名字，不需要关心 GitHub 内部如何使用这个名字。

2.  **`on: push: branches: [main]`**
    *   **作用**: 定义**触发**这个自动化流程的**条件**。这里设置的是：当你向 `main` 这个分支**推送 (push) 代码**时，这个流程就会**自动开始运行**。
    *   **托勒斯法则**: 你只需要知道“推送到 main 分支”这个动作会启动流程，不需要关心 GitHub 是如何监控推送事件或分支的。

3.  **`jobs: deploy:`**
    *   **作用**: 定义这个流程中要执行的一个或多个**任务组（作业）**。这里只有一个任务组，名字叫 `deploy`。
    *   **托勒斯法则**: 你只需要知道这里开始定义一个主要的工作单元，它包含了一系列要完成的步骤。

4.  **`runs-on: ubuntu-latest`**
    *   **作用**: 指定 `deploy` 这个任务组**运行在什么样的虚拟环境**里。这里指定的是 `ubuntu-latest`，意味着 GitHub 会提供一个最新的 **Ubuntu Linux** 虚拟服务器来运行后续步骤。
    *   **托勒斯法则**: 你只需要知道你的自动化步骤将在一个 Linux 环境中执行，不需要关心这个环境是如何创建和管理的。

5.  **`steps:`**
    *   **作用**: 定义 `deploy` 作业中要**按顺序执行**的**具体步骤**。

6.  **`- name: Checkout`**
    *   **作用**: 给这个步骤起个名字，叫 `Checkout`，方便在日志中查看。
    *   ♈**`uses: actions/checkout@v4`**: **使用**一个**预先写好的、官方提供的“动作” (Action)**，叫做 `actions/checkout` (版本是 v4)。♐这个动作的功能是**把你的仓库代码下载（检出）**到当前的虚拟运行环境中，以便后续步骤可以使用这些代码。
    *   **`with: fetch-depth: 0`**: 这是给 `actions/checkout` 动作传递的一个**参数**。`fetch-depth: 0` 的意思是**下载完整的代码历史记录**，而不是默认只下载最近的一次提交。部署动作 `peaceiris/actions-gh-pages` 有时需要完整的历史记录才能正确工作。
    *   **托勒斯法则**: 你只需要知道这个步骤会把你的代码准备好，并且需要完整的历史。你不需要知道 `checkout` 动作内部是如何通过 Git 命令实现下载的。

7.  **`- name: Setup Hugo`**
    *   **作用**: 步骤名，设置 Hugo 环境。
    *   ♈**`uses: peaceiris/actions-hugo@v3`**: **使用**一个由**社区开发者 (peaceiris) 维护的 Action**，叫做 `actions-hugo` (版本是 v3)。这个动作专门用来**安装和配置 Hugo**。
	【♓这一步的作用是把能够执行 hugo 命令的那个程序安装好 ，因为♐即使你的项目文件上传到了仓库，也仍然需要安装 Hugo 包。你上传到仓库的是你博客的“原材料”（Markdown 文件、主题、配置），而不是制作网站的“工具”（即 Hugo 程序本身））。”
托勒斯法则: 你只需要知道，这一步负责获取你的博客源代码（包括你的 Markdown 文章、主题、配置文件等），并将它们准备好，以便后续步骤使用。它确实使用了你推送到仓库的文件，♈Action 运行环境是一个临时的、干净的 Linux 系统，它默认并没有安装 Hugo 这个“工具”】
    *   **`with:`**: 给 `actions-hugo` 动作传递参数：
        *   **`hugo-version: 'latest'`**: 告诉动作要安装**最新版本**的 Hugo。
        *   **`extended: true`**: 告诉动作要安装 Hugo 的 **Extended（扩展）版本**。这个版本支持 SASS/SCSS 等高级功能，很多主题需要它。
    *   **托勒斯法则**: 你只需要知道这个步骤会自动安装好你需要的 Hugo 版本，不需要关心它具体从哪里下载、如何安装。

8.  **`- name: Build Web`**
    *   **作用**: 步骤名，构建网站。
    *   **`run: hugo -D`**: **直接运行一条命令**：`hugo -D`。这就是你在本地用来**生成静态网站文件**的命令。它会读取你的 Hugo 项目文件，并把最终的网站文件（HTML, CSS, JS, 图片等）生成到默认的 `public` 目录下。`-D` 参数是包含标记为草稿 (draft) 的内容。
    *   **托勒斯法则**: 你只需要知道这个步骤执行了 Hugo 的构建命令，并产出了 `public` 文件夹，就像你在本地操作一样。

9.  **`- name: Deploy Web`**
    *   **作用**: 步骤名，部署网站。
    *   **`uses: peaceiris/actions-gh-pages@v4`**: **使用**另一个由 **peaceiris 维护的 Action**，叫做 `actions-gh-pages` (版本是 v4)。这个动作专门用来**将一个文件夹的内容推送到指定仓库的指定分支**，非常适合部署到 GitHub Pages。
    *   **`with:`**: 给 `actions-gh-pages` 动作传递部署所需的参数：
        *   **`PERSONAL_TOKEN: ${{ secrets.TOKEN }}`**: **提供认证凭证**。这个动作需要权限去推送代码到目标仓库。这里使用的是你之前创建并存储在仓库 **Secrets** 中的 **Personal Access Token (PAT)**。`secrets.TOKEN` 表示去读取名为 `TOKEN` 的那个 Secret 的值。
        *   **`EXTERNAL_REPOSITORY: letere-gz/hugo-dev`**: **指定目标仓库**。告诉动作要把 `public` 目录下的文件推送到 `letere-gz` 这个用户（或组织）下的 `hugo-dev` 仓库。**你需要将 `letere-gz/hugo-dev` 替换成你自己的 GitHub 用户名和用于托管 GitHub Pages 的仓库名** (例如 `your-username/your-username.github.io`)。
        *   **`PUBLISH_BRANCH: main`**: **指定目标分支**。告诉动作要把文件推送到目标仓库的 `main` 分支。**你需要根据你的 GitHub Pages 设置，将其修改为正确的目标分支** (常见的有 `gh-pages` 或 `main`)。
        *   **`PUBLISH_DIR: ./public`**: **指定要部署的内容来源**。告诉动作需要推送的是当前目录下 `public` 文件夹里的所有内容。
        *   **`commit_message: auto deploy`**: **设置部署时产生的 Git 提交信息**。每次部署都是一次 Git 提交，这里设置提交信息为 "auto deploy"。
    *   **托勒斯法则**: 你只需要知道这个步骤负责把 `public` 目录的内容，使用你提供的 Token 认证后，推送到你指定的目标仓库和分支。你不需要关心它内部是如何进行 Git 操作和推送的。

---

> Author:   
> URL: http://example.org/posts/hugo%E5%8D%9A%E5%AE%A2/action-%E4%BB%A3%E7%A0%81%E6%88%91%E8%87%AA%E5%B7%B1%E5%86%99%E4%B8%8D%E6%9D%A5%E7%9A%84%E4%B8%8D%E7%94%A8ai%E7%9A%84%E6%83%85%E5%86%B5%E4%B8%8B%E9%9C%80%E8%A6%81steps%E5%AD%97%E6%AE%B5%E4%B8%8B%E7%9A%84users%E5%8F%82%E6%95%B0/  

