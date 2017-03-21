## DESCRIPTION
The task of Chinese text detection is to localize the regions in a 2D image which contain Chinese characters. The task of Chinese text recognition is, given the localized regions including text, to convert each region into machine-encoded text. It is an important technique for understanding text information in 2D images, and many other applications such as text-to-speech, machine translation, text mining, etc.

## PROGRESS
### WEEK 1

#### Overview

- 搭建项目网站

- 阅读相关论文


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

#### References

1. Text Detection and Recognition in Imagery: A Survey, TPAMI, 2015

### WEEK 2

#### Overview

* 研究文本检测方面的常用方法
* 学习运行MATLAB中的一个文字检测与识别的DEMO

#### Details

场景文字检测的基本思想是：首先根据文字所具有的特征去确定图像中的文字区域，由于干扰因素的存在错把一些非文字区域判定为文字区域，所以接下来就需要利用一些规则或者候选区域的统计特性等来排除非文本区域。从而能准确定位图片中的文本区域：

* Text Localization
* Text Verification

对于文本定位来说，目前用的比较多的两种方法是：基于连通域（CCA）和滑动窗口分类法。基于连通域的方法首先是通过颜色或者区域极值等属性聚类，得到连通域，如目前很流行的[MSER](https://en.wikipedia.org/wiki/Maximally_stable_extremal_regions)方法，然后再根据人为设置的规则或者机器学习方法学习到的特征来排除非文本区域。该方法提取出来的连通域的数目相对较少，同时具有尺度不变性，对文字大小不敏感等优点。

在文本检测中，用到的特征主要有以下几类：传统的有颜色、边缘、纹理等，近些年研究的特征包括笔划、点和区域等。

为了对整个项目有一个直观的感受，我们实验了MATLAB给出的关于自然场景下的文本检测与识别的一个DEMO，该DEMO是利用的是MSER方法来检测文本区域，之后利用OCR来进行识别。我们选了一张与DEMO中不同的测试图片，原图如下：

![test.jpg](http://o9zemtn5i.bkt.clouddn.com/text_detection.jpg)

在DEMO给的默认参数下，文本检测结果如下：

![test_output1](http://o9zemtn5i.bkt.clouddn.com/test_output1.jpg)

于是我们修改了detectMSERFeatures的一些参数，检测结果就好了很多。

![test_output2](http://o9zemtn5i.bkt.clouddn.com/test_output2.jpg)

这也是逐步法进行文本检测与识别的一个缺点，就是调参比较麻烦，不同图像背景下的检测效果可能会有很大差异。

#### References

1. Qixiang Ye,  Text Detection and Recognition in Imagery: A Survey, TPAMI, 2015
2. 杨飞，自然场景图像中的文字检测综述，电子设计工程，2016
3. MathWorks:  [Automatically Detect and Recognize Text in Natural Images](https://cn.mathworks.com/help/vision/examples/automatically-detect-and-recognize-text-in-natural-images.html?requestedDomain=www.mathworks.com)

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
