+++
author = "taojl"
title = "用gemini测网络质量"



collections = ["cursor经常运行gemini2.5pro失败就是网络的原因",
"google ai studio进场跑模型失败就是网络的原因"
]
date = "2025-01-28"
#description = "Sample article showcasing basic Markdown syntax and formatting for HTML elements."

categories = [

"就是网络不行导致的问题",
"gemini跑不了就是网络的问题",
 "网络行不行 gemini一测就知道",

]
tags = [
    "节点行不行 gemini一测就知道",
    "CDN ip行不行 gmmini一测就知道",
    "",
    "",
]

series = ["测网络（全）","网络问题（全）"]
#aliases = ["migrate-from-jekyl"]
#image = "pawel-czerwinski-8uZPynIu-rQ-unsplash.jpg"

+++
༼ ༽

#测试CDN的速度或节点速度的行不行
- 用gemin2.5pro就能轻松测出来，无论是在cursor还是在google ai studio
	- ♐如果节点速度不行，那么用cursor跑gemini2.5pro就经常跑失败，cursor连续两次跑gemin2.5pro失败那一定是网络问题
	- ♒google ai studio同理，google ai studio跑gemini模型对网络要求非常高，连续两次跑模型失败那就是网络问题
	- ♒google ai stuido对视频提取的网络要求变高了