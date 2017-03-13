## DESCRIPTION
The task of Chinese text detection is to localize the regions in a 2D image which contain Chinese characters. The task of Chinese text recognition is, given the localized regions including text, to convert each region into machine-encoded text. It is an important technique for understanding text information in 2D images, and many other applications such as text-to-speech, machine translation, text mining, etc.

## PROGRESS
### WEEK 1

#### Overview

- 搭建项目网站

- 阅读相关论文

  ​

#### Details

本周主要看了老师推荐的那篇关于图像中的文本检测与识别的综述文章。

图像中的文本主要分为两类：图形文本（graphic text）和场景文本（scene text）。图形文本是指覆盖在图像上的机印的文本，比如一些图片标注、字幕、注释等；场景文本是指自然场景中一些物体上显示的文本，比如墙壁上的标语、衣服上的文字等。目前大部分的研究都集中在场景文本的检测与识别上，因为场景文本的检测与识别更具挑战性，具体表现在场景的复杂性、光照条件、图像形变、不同字体和多语言文本环境等各方面因素的影响，这些都给场景文本的检测和识别带来困难。

目前在完整的文本检测和识别系统中一般采用两种方法：逐步法（Stepwise）和集成法（Integrated）：

- 逐步法：逐步法将文本检测与识别的过程拆分为四个步骤：
  - 定位（localization）：粗略定位候选的文本区域
  - 验证（verification）：通过文本图像中某种共有不变的模式或特征来进一步验证文本区域
  - 分割（segmentation）：对文本中的字符进行分割，为下一步的识别做准备
  - 识别（recognition）：将分割后的图像块转换成字符
- 集成法：集成法主要以字符分类响应（character classification responses）作为主要特征，被检测和识别模块共享。

下面是两种方法的框架图：

![framework](http://o9zemtn5i.bkt.clouddn.com/framework.JPG)

下面是两种方法的比较：

- 逐步法采用了一个由粗到精（coarse-to-fine）的策略，一方面，无关的背景在粗略定位文本那一步就被去除了，为接下来的步骤降低了计算复杂度；另一方面，由于是模块化的解决方案，所以每一步都可以利用特定的方法来提升最终的检测和识别效果，同时功能可拓展，比如实现多语言识别；缺点主要有两个，一个是系统结构较复杂，另一个是每一步的参数优化较难，可能导致误差的积累；
- 集成法系统相对简单，免去了每一步的参数优化，对复杂背景和低分辨率的文本图像相对来说，不是很敏感，缺点是计算量太大，因为有字符类别太多；另外，随着新的字符类别的增加，系统的表现就会下降，所以集成法通常被用来检测和识别小量的特定的字符。

### WEEK 2

coming soon
### WEEK 3
coming soon 
### WEEK 4
coming soon
### WEEK 5
coming soon

## DEMO
coming soon

## TEAM MEMBERS
| Name |     ID     |       Contact        |
| :--- | :--------: | :------------------: |
| 王硕   | M201671781 | wangshuotzjz@163.com |
| 沈钊   | M201671775 | hustershen@gmail.com |
| 孙保杰  | M201671796 |   176172955@qq.com   |
| 周乘   | M201671807 |   294058634@qq.com   |
