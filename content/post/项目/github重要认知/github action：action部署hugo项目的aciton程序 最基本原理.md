---
title: github action：0 action部署hugo项目的aciton程序 最基本原理
date: 2024-02-20T20:14:22+08:00
draft: false
categories:
  - github重要认知
collections:
  - action部署hugo的原理 就是在linux系统使用hugo命令 所以linux环境要有hugo包
tags:
  - github action的作用
series:
  - github
---



		- [action部署hugo项目的aciton程序]
			- <action部署hugo项目的aciton程序的核心：让action使用hugo命令，action环境需要hugo命令执行hugo博客>
			- <1 有参考作用的使用uses参数的action程序的写法（参考如何让AI写action程序）>
			- <2 有参考作用的使用run字段的action程序的写法 https://aistudio.google.com/prompts/1dv1SBgRsYgFBSgYe1Dseg0dyYouTZ3_P（参考如何让AI写action程序）>

		- [[[[在action环境安装项目包（hugo包）的两种做法（参考如何让AI写action程序）]
			- 《1 uses 关键字并指定一个像 别大大佬些好的aciton 比如peaceiris/actions-hugo@v3 这样的“动作 (Action)”时，你是在委托这个预先打包好的工具去完成安装和配置 Hugo 的任务》
			- 《2 直接用run字段亲自告诉那个临时的 Linux 系统具体怎么做才能把 Hugo 装好》


		- [aciton想部署hugo的核心就是用别人预先写好的东西]
			- 《hugo官方有提供官方action动作把仓库代码下载（检出） 到当前的虚拟运行环境中，它就是actions/checkout@v4，通过uses: actions/checkout@v4: 就能使用一个预先写好的、官方提供的“动作” (Action)》
		