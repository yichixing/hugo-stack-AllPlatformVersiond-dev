---
title: github action：2 github action部署项目原理：github action是将项目部署到其它仓库，不是部署到自己的仓库
date: 2023-02-20T20:14:22+08:00
draft: false
categories:
  - github重要认知
collections:
  - blog
tags:
  - github action的作用
---
(08:41-10:19) ***4.4: 配置 Repository Secret***
-  ♈ **目的**：将生成的 Token 安全地提供给 **GitHub Actions workflow** 使用。**不应该**直接将 Token 字符串写入 YAML 文件。
-   **操作路径**：回到♎♐**存放 Hugo 源代码**的仓库（即 `hugo-main`），进入该仓库的 `Settings` -> 左侧菜单 `Security` 下的 `Secrets and variables` -> `Actions`。
-   ♈**操作**：点击 `New repository secret` 按钮。
-   **配置 Secret**：
    *   `Name`：输入 **Secret 的名称**。这个名称**必须**与 workflow YAML 文件中 `secrets.` 后面引用的名称**完全一致**。作者在这里将其命名为 `TOKEN`。
    *   `Secret`：将刚才**复制并保存**的**完整 Token 字符串**粘贴到这里。
-   **保存**：点击 `Add secret`。
-   ♈**优点**：使用 **Secrets** 可以**隐藏**敏感信息，workflow 运行时会安全地注入该值，比硬编码在代码中**安全**得多


[[[[[使用aciton后将分源代码仓库和静态页面仓库 ]https://aistudio.google.com/prompts/1dv1SBgRsYgFBSgYe1Dseg0dyYouTZ3_P
	《《《hugo-main 仓库 (源代码仓库)，专门用来提交项目源码的仓库，以及用来运行action的仓库，不需要开启github page的功能，所以hugo-main仓库不需要公开，也不需要把静态资源上传到这仓库上》
	《《《hugo-dev 仓库 (部署目标/静态页面仓库)，hugo-main运行action后生成的静态资源文件所推送到的仓库，是存放最终网站文件并对外提供访问的地方，它是自动化流程的终点/发布目标，因此我们在仓库开启github page功能》
		《《《aciton中的参数 EXTERNAL_REPOSITORY: letere-gz/hugo-dev 就是用来 明确指定了main仓库部署的目标是这个dev仓库。》