---
title: github page部署hugo博客
date: 2025-02-20T20:14:22+08:00
draft: false
categories:
  - 项目部署
collections:
  - hugo博客项目
tags:
  - 项目部署
---

# part1: 创建新的 GitHub 仓库

(00:04-00:23) ***1.1: 推荐仓库命名***
对于**首次部署博客**的用户，视频**推荐**将 GitHub **仓库名称**设置为 `**用户名.github.io**` 的格式。视频中演示了输入 `**letere-gg.github.io**` 作为示例。

(00:23-00:30) ***1.2: 处理命名冲突***
视频指出 `**letere-gg.github.io**` 这个特定名称在其账户下**已被占用**，因为它已被用于**部署**另一个项目（提及了 **Hexo**，但后续操作基于 **Hugo**）。

# part2: 理解 GitHub Pages 的 URL 结构

(00:30-00:47) ***2.1: `用户名.github.io` 仓库的 URL***
视频展示了名为 `**letere-gg.github.io**` 的仓库，在其 **Settings -> Pages** 中，生成的 **GitHub Pages** 网站 URL 是**根域名** `**https://letere-gg.github.io**`。

(00:48-01:02) ***2.2: 普通仓库的 URL***
视频接着展示了另一个名为 `**hugo-stack**` 的仓库，在其 **Settings -> Pages** 中，生成的 **GitHub Pages** 网站 URL 带有**仓库名称作为子路径**，即 `**https://letere-gg.github.io/hugo-stack**`。

(01:02-01:18) ***2.3: 部署建议***
基于以上对比，视频**建议首次部署时使用 `用户名.github.io` 格式命名仓库**，这样生成的网站 URL **不带子路径**，可以**避免一些潜在的 bug**。

# part3: 设置新仓库与本地项目配置

(01:21-01:35) ***3.1: 创建示例仓库***
由于推荐的仓库名已被占用，视频演示创建了一个**新的示例仓库**，命名为 `**hugo-dev**`，并确保其设置为 **Public**（公开），因为**只有公开仓库才能部署 GitHub Pages 静态页面**。

(01:35-01:46) ***3.2: 准备上传代码***
仓库创建成功后，页面显示了用于**上传代码的 Git 命令**。视频提到下一步是上传代码，但在此之前需要**修改一个配置文件**。

(01:46-02:16) ***3.3: 修改 `hugo.yaml` 文件***
视频打开本地项目中的 `**hugo.yaml**` 文件，指出需要修改 `**baseURL**` 配置项。将其值**修改为**与 GitHub Pages 部署后**期望的 URL 一致**。因为仓库名是 `hugo-dev`，所以 `baseURL` 被设置为 `**https://letere-gg.github.io/hugo-dev/**`，**必须包含**末尾的仓库名子路径。

# part4: 生成静态网站并上传至 GitHub

(02:18-02:46) ***4.1: 使用 Hugo 生成站点***
视频演示在本地项目文件夹中♈**删除旧的 `public` 文件夹**（如果存在）。然后在项目**根目录**打开**命令提示符 (cmd)**，♈运行 `**hugo -D**` 命令。此命令使用 **Hugo** **编译**项目，并♈将生成的**静态网站文件**输出到 `public` 文件夹中。

(02:46-03:18) ***4.2: 初始化 Git 仓库***
♈进入新生成的 `public` 文件夹，确认里面是**静态网站内容**。在此 `public` 文件夹内打开**命令提示符 (cmd)**，并♈执行 `**git init**` 命令，将其**初始化为一个 Git 仓库**。

(03:18-03:49) ***4.3: 添加文件、提交并关联远程仓库***
♈按照 GitHub 页面提供的指南（略作修改）：
*   使用 `**git add .**` 命令将 `public` 文件夹内**所有生成的静态文件**添加到暂存区（视频强调是 `.` 而不是 `README.md`）。
*   使用 `**git commit -m "first commit"**` 提交更改。
*   使用 `**git branch -M main**` 将当前分支重命名为 `main`。
*   使用 `**git remote add origin https://github.com/letere-gg/hugo-dev.git**` 关联到之前创建的**远程 GitHub 仓库**。

(03:49-04:08) ***4.4: 推送代码到 GitHub***
♈执行 `**git push -u origin main**` 命令，将本地 `main` 分支的**代码推送到 GitHub 远程仓库**。视频提到在国内环境可能需要**开启代理**才能成功推送。推送完成后，**刷新 GitHub 仓库页面**，可以看到**静态文件已成功上传**。

# part5: 启用 GitHub Pages 并验证部署

(04:09-04:23) ***5.1: 配置 Pages 源***
返回 GitHub 上的 `hugo-dev` 仓库页面，进入 **Settings -> Pages** 选项卡。在 "Build and deployment" 部分下的 "Source" 设置中，选择 **Deploy from a branch**，然后从 "Branch" 下拉菜单中选择 `**main**` 分支，点击 **Save**。

(04:23-05:24) ***5.2: 等待部署完成***
保存设置后，**GitHub Pages 开始部署**过程。视频指出这**需要一些时间**。等待页面刷新，直到显示 "Your site is live at ..." 并给出**最终的网站 URL**：`**https://letere-gg.github.io/hugo-dev/**`。

(05:24-05:35) ***5.3: 访问并验证站点***
点击页面上提供的**网站链接**。浏览器成功加载了部署好的**博客网站**，显示内容正常，证明**部署已成功**。视频总结这个过程**很简单**。