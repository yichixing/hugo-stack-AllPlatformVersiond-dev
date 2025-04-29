---
title: hugo博客action代码部署：hugo博客的action代码本体
date: 2025-02-20T20:14:22+08:00
draft: false
categories:
  - 项目部署
collections:
  - hugo博客的action代码本体  也可以是action模板用来扔给AI修改
tags:
  - action代码本体
series:
  - 项目部署
---


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

