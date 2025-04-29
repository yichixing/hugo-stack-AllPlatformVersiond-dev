---
title: 使用github-action自动部署hugo博客到“page”仓库
date: 2025-02-20T20:14:22+08:00
draft: false
categories:
  - 项目部署
collections:
  - hugo博客项目
tags:
  - 项目部署
---
# part1: 介绍 GitHub Action 自动化部署 Hugo 博客

(00:00-00:09) ***1.1: 介绍目标***
-   视频开篇展示了一个本地运行的 **Hugo** 博客 (`localhost:1313`)。
-   目标是向大家介绍如何通过 **GitHub** 的 **GitHub Actions** 功能来**自动部署**博客。

(00:09-00:32) ***1.2: 手动部署痛点与自动化优势***
-   ♈指出传统方式下，每次修改完文件后，都需要**手动将生成**的 `public` 文件夹**上传**到服务器或托管平台，这个过程比较**麻烦**。
-   ♈使用 **GitHub Actions** 可以实现**自动化**：**自动**帮我们**生成** `public` 目录，并将其**部署**到**静态页面网站**（如 GitHub Pages）。
-   ♈**优点**：只需**提交代码** (`git push`)，后续的**构建**和**部署**过程就能**自动完成**，**方便**很多。

# part2: 创建 GitHub 仓库并推送 Hugo 源码

(00:32-00:41) ***2.1: 创建仓库准备***
-   要实现自动化部署，首先需要一个 **GitHub 仓库**来存放 **Hugo** 项目的**主文件**（源代码）。
-   操作：导航到 **GitHub** 网站，准备**创建**一个新的仓库。

(00:41-01:12) ***2.2: 创建私有仓库***
-   在 GitHub 创建新仓库页面（经历短暂的网络卡顿后），设置仓库信息：
    *   仓库名：`hugo-main`
    *   可见性：**建议**选择**私有（Private）**，因为源代码一般不需要公开。
-   操作：点击 `Create repository` 完成创建。

(01:12-01:24) ***2.3: 上传文件准备***
-   仓库创建完成后，需要将本地的 **Hugo** 项目文件**上传**到这个新仓库。
-   **关键点**：上传前需要注意**忽略**掉一些不需要版本控制的文件。

(01:24-02:04) ***2.4: 配置 .gitignore 忽略生成文件***
-   **说明**：**Hugo** 项目中的某些文件/目录是**自动生成的**，**不应该**提交到源代码仓库，包括：
    *   `public/`：**Hugo** 生成的静态网站文件目录。
    *   `resources/`：**Hugo** 处理后的资源缓存目录。
    *   `hugo.build.lock`：构建锁文件。
    *   `hugo.exe`：（如果在 Windows 下）**Hugo** 可执行文件本身。
-   ♈ **操作**：在本地 Hugo 项目根目录下，**创建**一个名为 `.gitignore` 的文件。
-   ♈**操作**：编辑 `.gitignore` 文件，将上述需要忽略的文件名或目录名（如 `public`, `resources`, `hugo.build.lock`, `hugo.exe`）添加进去，每行一个。
-   ♈**目的**：**保证**上传到 **GitHub** 的是**最干净、最原始**的 Hugo 项目文件。

(02:04-03:10) ***2.5: Git 推送操作***
-   **操作**：在本地 Hugo 项目目录下打开**命令行终端 (cmd)**。
-   ♈**操作**：执行一系列 **Git 命令（仓库首页有）**来初始化仓库、添加文件并推送到远程 **GitHub** 仓库：
    *   `git init`：**初始化**本地 Git 仓库。
    *   `git add .`：**添加**所有文件到暂存区（`.gitignore` 会生效，忽略指定文件）。
    *   `git commit -m "first commit"`：**提交**更改。
    *   `git branch -M main`：**重命名**当前分支为 `main`。
    *   `git remote add origin https://github.com/letere-gz/hugo-main.git`：**关联**远程仓库地址。
    *   `git push -u origin main`：**推送**本地 `main` 分支到远程仓库。
-   **验证**：切换到 **GitHub** 仓库页面刷新，确认项目文件已上传，且 `.gitignore` 中指定的文件（如 `public`）**未被上传**。

# part3: 配置 GitHub Actions Workflow

(03:10-03:59) ***3.1: 参考官方与民间方案***
-   **提及**：作者自己的博客 (`letere-gz.github.io/hugo-stack/`) 提供了关于 **Hugo + GitHub** 免费部署的教程文章。
-   ♈**参考**：查看 **Hugo 官方文档** (`gohugo.io`) 中关于 `Hosting and deployment` -> `Host on GitHub Pages` 的部分。
-   **发现**：官方文档提供了一个使用 **GitHub Actions** 部署的 **workflow** 示例（`Step 6` 中的 `hugo.yml` 文件内容）。
-   **评价**：作者认为官方提供的 **workflow** 文件内容**很长**，配置起来可能有点**麻烦**。

(03:59-04:13) ***3.2: 引入简化版 Workflow***
-   **建议**：作者推荐使用一个**民间大佬（peaceiris）**编写和维护的**简化版**的 **GitHub Actions workflow** 配置。
-   **来源**：该简化配置可以在作者的教程文章中找到（或者直接参考 `peaceiris/actions-hugo` 和 `peaceiris/actions-gh-pages` 这两个 Action）。
-   **优势**：相比官方示例，这个版本**更简洁、更方便**使用。

(04:13-06:15) ***3.3: 解析简化版 Workflow 配置***
-   **展示**：作者展示了其教程文章中的**简化版 workflow YAML 文件**内容，并逐段解释其含义：
    *   `name: deploy`: 定义 **Workflow 的名称**，显示在 Actions 列表里。
    *   `on: push: branches: [ main ]`: 设置**触发条件**，当代码被 `push` 到 `main` 分支时，自动执行此 workflow。
    *   `jobs: deploy:`: 定义一个名为 `deploy` 的**作业 (job)**。
    *  ♈ `runs-on: ubuntu-latest`: 指定该作业运行在 **GitHub** 提供的最新版 **Ubuntu** 虚拟环境上。
    *   `steps:`: 定义该作业包含的一系列**步骤 (step)**。
    *   `- name: Checkout`: 步骤名称。
    *   `uses: actions/checkout@v4`: 使用官方提供的 `checkout` **Action**，将**仓库代码**下载到运行环境中。
    *   `with: fetch-depth: 0`: **获取**完整的 Git 历史记录（对于 Hugo 可能需要）。
    *   `- name: Setup Hugo`: 步骤名称。
    *   `uses: peaceiris/actions-hugo@v3`: 使用 `peaceiris` 提供的 `actions-hugo` **Action** 来**安装和设置 Hugo 环境**。
    *   `with: hugo-version: 'latest', extended: true`: 配置 `actions-hugo`，指定安装**最新 (latest)** 版本的 Hugo，并且是**扩展 (extended)** 版本（支持 Sass/SCSS）。
    *   `- name: Build Web`: 步骤名称。
    *   `run: hugo -D`: **执行 Hugo 命令** (`hugo` 默认会构建，`-D` 表示包含草稿内容，但在部署时通常不需要 `-D`，作者这里可能写错了，通常只用 `hugo`)，**生成静态网站文件**到 `public` 目录。
    *   `- name: Deploy Web`: 步骤名称。
    *  ♈ `uses: peaceiris/actions-gh-pages@v4`: 使用 `peaceiris` 提供的 `actions-gh-pages` **Action**，将构建好的网站**部署到 GitHub Pages**。
    *   `with:`: 配置 `actions-gh-pages`。
    *   `PERSONAL_TOKEN: ${{ secrets.你的token变量名 }}`: **关键配置**，用于**授权** Action 推送代码到目标仓库。需要一个**个人访问令牌 (PAT)**，并将其存储为仓库的 **Secret**。`你的token变量名` 需要替换成实际设置的 **Secret 名称**。
    *   `EXTERNAL_REPOSITORY: 你的github名/你的仓库名`: **关键配置**，指定**目标仓库**，即用来**托管 GitHub Pages 的仓库地址**（例如 `letere-gz/hugo-dev`），**必须修改**为用户自己的 Pages 仓库。
    *   `PUBLISH_BRANCH: main`: 指定将 `public` 目录的内容推送到**目标仓库**的哪个**分支**（通常是 `gh-pages` 或 `main`，取决于 Pages 设置）。
    *   `PUBLISH_DIR: ./public`: 指定要**发布**的**目录**，即 Hugo 构建生成的 `public` 目录。
    *   `commit_message: auto deploy`: **自动部署**时 Git 提交的**消息**。

# part4: 创建和配置 Personal Access Token (PAT)

(06:15-06:49) ***4.1: Token 的作用与必要性***
-   **重申**：Workflow 中的 `PERSONAL_TOKEN` 参数需要填入一个**个人访问令牌 (PAT)**。
-   **目的**：这个 Token 用于**身份验证**，使得 **GitHub Actions** 有权限将构建好的静态文件推送到**目标 GitHub Pages 仓库**。没有它，推送会因权限不足而失败。
-   **操作准备**：接下来演示如何生成这个 Token。

(06:49-08:09) ***4.2: 生成 Personal Access Token (Classic)***
-   **操作路径**：在 **GitHub** 页面，点击右上角个人头像 -> `Settings` -> 左侧菜单滚动到底部找到 `Developer settings` -> `Personal access tokens` -> `Tokens (classic)`。
-   **操作**：点击 `Generate new token` 按钮，选择 `Generate new token (classic)`。
-   **配置 Token**：
    *   `Note`：给 Token 起一个**描述性名称**，例如 “用作部署”。
    *   `Expiration`：设置**过期时间**。作者**建议选择 `No expiration`（永不过期）**，理由是避免定期更新 Token 的麻烦。**但也提示** GitHub 出于**安全**考虑**建议**设置过期时间。对于个人私有仓库，作者认为永不过期问题不大。
    ♈*   `Select scopes`：选择 Token 的**权限范围**。**必须勾选**以下两项：
        *   `repo`：授予对仓库的完全控制权（包括推送）。
        *   `workflow`：授予更新 GitHub Actions workflow 的权限。
-   **生成**：点击页面底部的 `Generate token` 按钮。

(08:09-08:41) ***4.3: 保存 Token***
-  ♈ **重要提示**：Token 生成后，**完整的 Token 字符串**只会**显示这一次**。**必须立即复制** (`Copy` 按钮) 并将其**保存**在安全的地方（例如，作者将其粘贴到了本地的记事本文件中）。如果关闭或刷新页面，将**无法再次查看**完整的 Token。

(08:41-10:19) ***4.4: 配置 Repository Secret***
-  ♈ **目的**：将生成的 Token 安全地提供给 **GitHub Actions workflow** 使用。**不应该**直接将 Token 字符串写入 YAML 文件。
-   **操作路径**：回到**存放 Hugo 源代码**的仓库（即 `hugo-main`），进入该仓库的 `Settings` -> 左侧菜单 `Security` 下的 `Secrets and variables` -> `Actions`。
-   ♈**操作**：点击 `New repository secret` 按钮。
-   **配置 Secret**：
    *   `Name`：输入 **Secret 的名称**。这个名称**必须**与 workflow YAML 文件中 `secrets.` 后面引用的名称**完全一致**。作者在这里将其命名为 `TOKEN`。
    *   `Secret`：将刚才**复制并保存**的**完整 Token 字符串**粘贴到这里。
-   **保存**：点击 `Add secret`。
-   ♈**优点**：使用 **Secrets** 可以**隐藏**敏感信息，workflow 运行时会安全地注入该值，比硬编码在代码中**安全**得多。

# part5: 演示自动化部署流程

(10:19-10:31) ***5.1: 准备 workflow 文件***
-   **操作**：回到本地的 Hugo 项目目录（`dev` 目录）。
-   **下一步**：需要将包含 workflow 配置的 YAML 文件添加到项目中。

(10:31-11:29) ***5.2: 创建并编辑 workflow 文件***
-   ♈**操作**：在本地 Hugo 项目根目录下，**创建** `.github` 文件夹，然后在 `.github` 文件夹内**创建** `workflows` 文件夹。
-   ♈**操作**：在 `workflows` 文件夹中，**新建**一个 **YAML 文件**，作者将其命名为 `hugo_deploy.yaml`。
-   ♈**操作**：使用文本编辑器打开 `hugo_deploy.yaml` 文件，将之前从教程文章中**复制**的**简化版 workflow** 内容**粘贴**进去。

(11:29-11:57) ***5.3: 修改 workflow 文件参数***
-   **操作**：在粘贴好的 workflow 内容中，**修改**两个关键的 `with` 参数：
    *   ♈将 `PERSONAL_TOKEN: ${{ secrets.你的token变量名 }}` 修改为 `PERSONAL_TOKEN: ${{ secrets.TOKEN }}`，确保 `TOKEN` 与上一步创建的 **Repository Secret 名称**一致。
    *  ♈ 将 `EXTERNAL_REPOSITORY: 你的github名/你的仓库名` 修改为**实际的 GitHub Pages 目标仓库地址**，作者修改为 `letere-gz/hugo-dev`。
-   **保存**：保存对 `hugo_deploy.yaml` 文件的修改。

(11:57-12:47) ***5.4: 创建新文章验证部署***
-   **目的**：为了验证**推送到 `main` 分支**是否能**自动触发部署**，需要对项目做一些更改，例如**添加一篇新文章**。
-   **现状**：先查看本地 (`localhost:1313`) 运行的博客，确认当前的文章列表（只有一个 "Chinese Test"）。

(12:47-13:22) ***5.5: 本地新增文章***
-   **操作**：在命令行中，执行 `hugo new content post/newnewnew/index.md` 命令，在 `content/post` 目录下创建一个名为 `newnewnew` 的子目录，并在其中生成 `index.md` 文件。
-   **操作**：再次执行 `hugo server -D` 启动本地服务。
-   **验证**：访问 `localhost:1313`，确认名为 `Newnewnew` 的新文章已成功创建并显示在本地博客首页。

(13:22-14:03) ***5.6: 推送更改触发 Action***
-   **操作**：在命令行中按 `Ctrl+C` **停止**本地 `hugo server`。
-   **操作**：执行 **Git 命令**将更改推送到远程仓库：
    ♈*   `git add .`：**添加**所有更改（包括新文章 `.md` 文件和 `.github/workflows/hugo_deploy.yaml` 文件）。
    ♈*   `git commit -m "update"`：**提交**更改，提交信息为 "update"。
    ♈*   `git push`：**推送**本地 `main` 分支的提交到 **GitHub** 上的 `hugo-main` 仓库。

# part6: 验证自动化部署结果与总结

(14:03-14:29) ***6.1: 观察 Action 执行***
-   **操作**：切换回 **GitHub** 网站，进入 `hugo-main` 仓库的 `Actions` 标签页。
-   **观察**：可以看到一个由刚才的 `push` 事件触发的名为 `update`（commit 信息）的 **workflow run** 正在执行（黄色图标，`in progress`）或者已经执行完毕（绿色勾，`Success`）。
-   **检查**：点击该 workflow run，可以查看名为 `deploy` 的 job，再点击进去可以看到详细的**执行步骤和日志**，确认 `Checkout`, `Setup Hugo`, `Build Web`, `Deploy Web` 等步骤均已**成功完成**。

(14:29-14:53) ***6.2: 验证部署结果（首次刷新）***
-   **操作**：打开或刷新用于 **GitHub Pages** 托管的**目标网站**（作者的是 `https://letere-gz.github.io/hugo-dev/`）。
-   **发现**：**第一次刷新**时，页面上可能**仍然没有**显示新添加的文章 `Newnewnew`。
-   **原因**：**GitHub Pages** 的部署和 **CDN 的缓存更新**通常会有一定的**延迟**。

(14:53-15:10) ***6.3: 验证部署结果（成功显示）***
-   **操作**：**再次刷新** GitHub Pages 网站页面。
-   **结果**：新添加的文章 `Newnewnew` **成功出现在了**部署后的网站首页上。
-   **结论**：这表明 **GitHub Actions** 已经成功地**自动拉取**了 `hugo-main` 仓库的最新代码，使用 **Hugo** 进行了**构建**，并将生成的 `public` 目录内容**推送并部署**到了 `hugo-dev` 这个 **GitHub Pages** 仓库。

(15:10-15:40) ***6.4: 总结与结束***
-   **总结**：通过以上设置，现在实现了**自动化部署流程**。开发者只需专注于**编写文章** (`.md` 文件)，完成后执行 `git push` 将代码推送到源仓库 (`hugo-main`)。**GitHub Actions** 就会**自动接管**后续的**构建和部署**工作，**无需再手动**处理 `public` 目录的生成和上传。
-   **评价**：整个过程变得非常**方便和省心**。