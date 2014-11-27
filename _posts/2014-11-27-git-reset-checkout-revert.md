---
layout: post
category: Revision Control
title: Git撤销操作
tagline: by BluePandaLi
tags: [Git]
---
Git中的撤销操作分以下两种情况未提交和已提交。

<!--more-->

###未提交撤销操作

* 重置整个工作目录状态
 
		git reset --hard HEAD
* 重置某个文件  

		git checkout -- fileName
		
###已提交撤销操作  
*   如果已经进行Push操作，理论上可以进行reset操作后强行提交。**但是最好不要这么做**，git不会处理项目的历史会改变的情况，如果一个分支历史被改变了以后不能正常的合并。最好的做法是创建一个新的commit去修正提交的错误。  

		git reset --hard SHA
		git push -u master origin -f

*   如果没有进行Push操作可以进行revert操作  

		git revert HEAD  #撤销最近一次的操作
		git revert HEAD^ #撤销上上次的操作
		  
		  
		  
参考资料: [http://gitbook.liuhui998.com/4_9.html](http://gitbook.liuhui998.com/4_9.html)
	