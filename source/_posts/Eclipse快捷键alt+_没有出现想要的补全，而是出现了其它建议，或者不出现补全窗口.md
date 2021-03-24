---
title: 'Eclipse快捷键alt+_没有出现想要的补全，而是出现了其它建议，或者不出现补全窗口'
date: 2020-03-18 15:03:27
tags: 
 - eclipse
categories:
 - eclipse
---
这个是因为默认默认提示没有勾选到Java peoposals 这个选项，在路径Window->preferences->Java->Editor->content assis->Advanced
也可直接在preferences中搜索proposal如图所示，在上方点上Java proposals就可以了。这样在alt+/后就可直接出现Java建议。
下面的在 alt+/ 后重复按得话会变成另外建议选项，可根据需要修改。

如果alt+/是没反应的话，则应该查看是否快捷键冲突，或者eclipse的这个快捷键没有打开。在General->keys中查看。
![eclipse设置](https://raw.githubusercontent.com/xfx98/ms/master/img/eclipse-setpeoposals.png)
