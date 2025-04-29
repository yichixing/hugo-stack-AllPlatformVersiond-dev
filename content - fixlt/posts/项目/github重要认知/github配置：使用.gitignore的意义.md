---
title: github配置：1 使用.gitignore的意义
date: 2023-02-20T20:14:22+08:00
draft: false
categories:
  - github重要认知
collections:
  - blog
tags:
  - github项目配置
---

(01:24-02:04) ***2.4: 配置 .gitignore 忽略生成文件***
-   **说明**：**Hugo** 项目中的某些文件/目录是**自动生成的**，**不应该**提交到源代码仓库，包括：
    *   `public/`：**Hugo** 生成的静态网站文件目录。
    *   `resources/`：**Hugo** 处理后的资源缓存目录。
    *   `hugo.build.lock`：构建锁文件。
    *   `hugo.exe`：（如果在 Windows 下）**Hugo** 可执行文件本身。
-   ♈ **操作**：在本地 Hugo 项目根目录下，**创建**一个名为 `.gitignore` 的文件。
-   ♈**操作**：编辑 `.gitignore` 文件，将上述需要忽略的文件名或目录名（如 `public`, `resources`, `hugo.build.lock`, `hugo.exe`）添加进去，每行一个。
-   ♈**目的**：**保证**上传到 **GitHub** 的是**最干净、最原始**的 Hugo 项目文件。
	《♓目的就是不要让静态资源污染源码，因为hugo-main是源代码仓库，源代码仓库不负责展示github page，只负责运行action得到静态资源给其它部署仓库hugo-dev，所以没必要把多余的静态资源上传到hugo-main，反正在hugo-main展示这些静态资源》
	<git add .：添加所有文件到暂存区（♎.gitignore 会生效，忽略指定文件）。>