+++
author = "taojl"
title = "编程能力对比gemini2.5pro vsqwen2.5max和Claude-3.7-Sonnet "
date = "2025-03-28"
#description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."
tags = [
"通过能力对比来测试模型" ,
 "模型综合能力测试比对 gemini2.5pro vs claude 3.7",
"环境变量赋值给配置文件的参数"
]
categories = ["模型测试","项目部署"]
collections = [
"模型编程能力对比gemini2.5pro vs  qwen2.5max和Claude-3.7-Sonnet ",
"模型综合能力（UI设计、文件索引、推理能力）的测试比对 gemini2.5pro vs claude 3.7",
"配置文件的参数baseURL"
]
series = ["AI","项目部署","AI模型测试"]
+++

- 《《《编程能力 gemini2.5pro > qwen2.5max vs Claude-3.7-Sonnet》
- 【测试题目：想同时在GitHub Pages和Cloudflare Pages上部署Hugo博客，因此动态设置 baseURL】     
 	- 《《能行的方案：分离构建，各自为政；既然问题根源在于“一次构建，两个不同 baseURL 的目标”，最清晰的解决方案就是让两个平台各自独立构建；简单来说就是github page和cloudflare page单独分别用main仓库源码部署》		
		- 《该方案可行的原因在于【cloudflare page真的有执行hugo源码的能力   cloudflare page能独立构建hugo项目得到hugo静态资源】，因此两个平台各自独立构建是成立的，让cloudflare page独立构建hugo项目得到hugo静态资源即可，然后github page也独立用action构建hugo项目得到hugo静态资源》
	- <用该编程题一测就知道模型编程能力的优劣>	
		- <<<【除了gemini2.5pro以外 ，其它模型都认为应该创建dev仓库的action代码 而且这个action还在run hugo -D】，这种说法是错误的，dev仓库都是从main仓库转换来的静态资源，因此dev仓库哪里来的源码让你执行hugo命令啊，能说出在dev仓库的action程序中执行run hugo -D 的全都是代码分析能力不行的，我把action源码给到这些模型，这些模型早就该分析出dev仓库都是静态资源，不可能在dev仓库执行aciton程序了>
		- 《《《另外有一个重要的认知就是【配置文件的参数的参数值仅在项目构建过程正被读取】，这意味着项目构建好后不会调用参数，比如 配置文件的baseURL必须在项目构建过程中把变量设置好，项目构建完成后再设置变量就没用了》
		- 《编程能力和推理能力一般的模型只会给你dev仓库的action代码，明明dev仓库的任何action的没意义》
	- 《《《测试结论：gemini2.5pro是唯一能成功解决我问题的模型，因此gemini2.5pro成为我能免费用到的最强模型，不但可以帮助我用视频学习，还有免费使用的平台Trae或无限邮箱cursor，编程能力还是免费用到中的最强》
- 《♒《逻辑推理是gemini2.5pro比claude强，gemini2.5pro逻辑推理能力很强 文件索引能力在有明确提示词要求的情况下也非常强，所以单纯的编程的使用一定是gemini2.5pro>claude》
	- 《♒《《因此gemini2.5pro适合开发 前后端 代码，而claude适合开发样式（UI）；♒claude用来开发UI的效果实在太好，本来我要求加个高光，claude还帮我加了个动画过渡效果》
	- 《图片识别能力也是gemini2.5pro强，如果有图片想让AI按图片要求来，也是用gemini2.5pro；claude我把分格图片给它看，它居然看成了是卡片；♒而gemini2.5pro是真的能照着图片来写代码》