# 2022 哈工深计科大一学习指南

<center>李修成 lixiucheng@hit.edu.cn</center>

## 数学基础课

### 微积分

微积分（Calculus）即你们的高等数学课程。Calculus一词源于希腊语，表示计算方法的意思，这是一门研究和处理连续变化量的数学分支，也是后续所有数学课和自然科学的基础。该课程分为微分（Differential Calculus）和积分（Integral Calculus），从学习顺序角度一般会先学习单变量微积分（Single Variable Calculus）---对应你们第一学期的高等数学上，然后是多变量微积分（Multivariable Calculus）---对应第二学期的高等数学下。我校工科采用同济版高等数学教材，这本教材采纳面广，具有目前国内教材的鲜明特色，定理和例子的罗列汇总，缺乏学习动机（motivation）和历史背景的介绍，很难称的上一本优秀教材。对于那些对数学有较高学习热情并且具有一定英语阅读能力的同学（其实以你们的才智，坚持一段时间都没问题，因为所有优秀的外文数学教材写的都简明易懂），我个人推荐Apostol的微积分，国内有影印版[第1卷](https://item.jd.com/13146298.html)和[第2卷](https://item.jd.com/13146362.html#crumb-wrap)，其中第1卷对应单变量微积分，第2卷对应多变量微积分。这本书的一大特色是按照微积分历史的发展顺序来进行讲解，深入浅出的揭示每个定义、定理、方法提出的历史背景和动机，帮你建立数学直觉（intuition），让你身临其境参与数学的发现和创造。与此同时，该书兼具深度和广度，深度体现在不但介绍微积分的知识，同时讲述必要的数学分析（mathematical analysis）基础，因此有人将其称为半微积分半数分教材；广度则体现在，该书将线性代数融入多变量微积分的讲解，使用线性代数作为统一语言，介绍了很多国内教材没有但又很重要的内容，如使用可逆变换和determinant来讲述the change of variables formula，更广义（general）形式的曲线曲面积分等。

![](https://img13.360buyimg.com/n1/jfs/t1/170866/20/12999/113427/6050382dE9d2e219f/053c4374df99627a.jpg)

![](https://img12.360buyimg.com/n1/jfs/t1/156645/6/16142/114045/60503b17E5a0ed177/c44322fea21804b4.jpg)

同时给大家推荐MIT在edx上的开放微积分课程18.01+18.02系列，在这里你可以同MIT的本科生看到一样的教学视频、做一样的作业，MIT的开放课程的一大特点是作业量大质优（requires you to invest large amount of time but very rewarding），大家可以组队学习互相督促，看谁能最后坚持下来。

单变量 MIT 18.01 （**推荐在大一上学期学习**）

[Calculus 1A: Differentiation](https://www.edx.org/course/calculus-1a-differentiation)

[Calculus 1B: Integration](https://www.edx.org/course/calculus-1b-integration)

[Calculus 1C: Coordinate Systems & Infinite Series](https://www.edx.org/course/calculus-1c-coordinate-systems-infinite-series)

多变量 MIT 18.02 （**推荐在大一下学期学习**）

[Multivariable Calculus 1: Vectors and Derivatives](https://www.edx.org/course/multivariable-calculus-1-vectors-and-derivatives?index=product&queryID=ef26f42da47b5b3f5d0b2a5708a4a063&position=1)

[Multivariable Calculus 2: Integrals](https://www.edx.org/course/multivariable-calculus-2-surfaces-and-integrals?index=product&queryID=98ccd2cb20e108e251e7334ff7497403&position=2)



### 线性代数

线性代数（Linear Algebra）这是一门无论将来你是从事科学研究还是工程应用（尤其是人工智能、数据科学），几乎每天都要使用的数学工具，一定要学好！！**推荐在大一完成，不需要微积分基础**。

很遗憾国内的教材试图在这门课上教你计算各种行列式和求解线性方程 :(

但我们是身处算力普及化的时代，解方程可以交给Python, Julia，在这门课上我们真正需要掌握的是深刻理解围绕线性函数所产生的各种基本概念、它们之间的联系以及其背后的几何直觉！！这些基本概念有 basis, span, linear independence, row space, column space, null space, range, rank, linear transformation, injective map, surjective map, invertible map, vector space, orthogonality, projection, diagonalization, eigenvalues, eigenvectors, change of basis, singular values, singular vectors, etc.

好消息是网络上有由Gilbert Strange主讲的MIT 18.06帮你达成上述目标，[b站视频](https://www.bilibili.com/video/BV1ix411f7Yp/?spm_id_from=333.788.recommend_more_video.0&vd_source=08edb25a833d26c16c800b5ea2a5854b)，教材由清华大学引进，[购买链接](https://item.jd.com/12699142.html)，良心价格 :)

![](https://img12.360buyimg.com/n1/jfs/t1/78410/12/9775/92725/5d75e4b0Ee371ff1f/69e182febb830a9b.jpg)

在学习线性代数的过程，非常推荐使用[Julia](https://julialang.org/)编程语言（非常**不**推荐Matlab）编写code来辅助学习，它原生的支持矩阵和向量表示，以及定义在上面的常用运算，通过写code计算来理解问题，可以很好的帮你建立数学直觉。这里有个[小教程](https://en.wikibooks.org/wiki/Introducing_Julia)可以帮你快速的学习使用Julia语言。

## 专业基础课

### UC Berkeley CS 61A 

[CS 61A: Structure and Interpretation of Computer Programs](https://cs61a.org/)，这门课脱胎于威名赫赫的魔法书[SICP](https://github.com/sarabander/sicp)，有人说SICP是截至目前计算机领域内最好的教材，没有之一的那种（读过前4章，一定程度上认可）。SICP最早用Lisp这门函数式语言编写，CS 61A使用Python改写，其[在线教材](http://composingprograms.com/)可以在线交互式阅读，这门课是导论性质的，在Berkeley是大一新生的计算机入门课程，注重计算思维、抽象思维和函数式编程思维的培养，最新一学期的课程刚刚开始，赶紧组队上车！b站上也有往年视频，请自行搜索。**推荐在大一上学期学习，立刻开始**。



### UC Berkeley CS 61B

[CS 61B: Data Structures and Algorithms](https://fa22.datastructur.es/)，数据结构和算法是计算机科学的核心基石之一，数据结构和算法的素养也是区分科班和培训班的重要标准，Linux之父Linus曾说过，“顶尖程序员和一般程序员的区别在于，顶尖程序员主要在思考如何有效使用数据结构，而一般的程序员则关注代码本身”。这门课使用java作为编程语言，不会java的也不要紧，因为这门课有保姆级的java教程，包你学会！同时这门课在b站上也有视频，和CS 61A类似其作业量大、含金量高，但也回报丰厚！这门课**推荐在完成CS 61A后进行**，对于**已经有编程基础**的同学也可以**直接**学习。

### C语言

C语言是系统语言，是学习后面操作系统和计算机系统的基础，推荐一本C语言之父写的书，[C程序设计语言](https://item.jd.com/12580612.html)。

![](https://img12.360buyimg.com/n1/jfs/t1/34588/8/839/146167/5cad9a57Ebe6aea40/9ceff26db3a586c1.jpg)



### CSAPP

CSAPP (Computer System a Programming Perspective)，深入理解计算机系统，从一个hello world C语言程序运行讲起，带你领略整个计算机系统，[b站视频](https://www.bilibili.com/video/BV1iW411d7hd?spm_id_from=333.337.search-card.all.click&vd_source=08edb25a833d26c16c800b5ea2a5854b)，[教材链接](https://item.jd.com/12155718.html)，推荐在有了C语言的基础后学习，这本书可以陪伴你大学四年的学习。

![](https://img13.360buyimg.com/n1/jfs/t4531/220/483445858/300979/584172a2/58d0cf49N6c0daead.jpg)



## 其他常用工具

### Linux Environment


If you use Linux OS and Mac OSX, you can directly launch the terminal.

If you use Windows OS, you can install WSL (Windows Subsystem for Linux) and [mobaxterm](https://mobaxterm.mobatek.net/), [check the tutorial](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-10#1-overview).

You should familarize yourself with the most commonly used linux command lines, they are indispensable for running code remotely on the servers. [Check this tutorial](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview).


### VSCode 

VSCode features tons of useful extensions and supports all mainstream programming languages. You can use it to write code, jupyter notebooks, markdown files, launch terminals and run commands,  and even connect to a remote server to run all aforementioned tasks. [b站教程](https://www.bilibili.com/video/BV1bK411P767?spm_id_from=333.337.search-card.all.click&vd_source=08edb25a833d26c16c800b5ea2a5854b) [c/c++配置](https://www.bilibili.com/video/BV1nt4y1r7Ez/?spm_id_from=333.788.recommend_more_video.1&vd_source=08edb25a833d26c16c800b5ea2a5854b)

### Anaconda3 & Jupyter Notebook

Anaconda3 is an integrated scientific computing package for Python, it includes all useful packages relevant to numerical computing. [Downloading](https://www.anaconda.com/products/distribution#Downloads) the appropriate version depending on your system.

Jupyter Notebook is an interactive computing environment based on web-browser and it will get installed with Anaconda3.

### Markdown and LaTeX

Very useful for making math notes and academic writing, learning the basics from this [tutorial](https://www.bilibili.com/video/BV1Fg411j7CW?share_source=copy_web).