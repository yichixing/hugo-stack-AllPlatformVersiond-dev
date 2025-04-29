---
title: github配置：2 github pull 与 github push，pull对分支后才能用push
date: 2023-02-20T20:14:22+08:00
draft: false
categories:
  - github重要认知
collections:
  - 用git来push推送项目前 必定先pull拉取项目，也就是pull拉取了main分支后 就能push推送到main分支了
tags:
  - github配置
series:
  - github
---


- [0 检查当前分支的跟踪关系]
	- <命令：git status；因为如果追踪错误，那么我想要push推送到我想要的分支很可能失败，因为追踪错误的缘故，pull的是其它的分支，因此我想要的分支就没法push了>

- [1 取消本地 main 分支对 所有分支 的跟踪]
	- <命令：git branch --unset-upstream>

- [[[[2 指定pull拉取main的分支（为了push到main，只能强制指定要拉取main分支）]
	- 《命令：git pull origin main》

- [[[[3 pull拉取了main分支后 就能push推送到main分支了]
	- <命令：git push -u origin main>