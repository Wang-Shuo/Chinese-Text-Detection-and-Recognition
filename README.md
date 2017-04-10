## DESCRIPTION
The task of Chinese text detection is to localize the regions in a 2D image which contain Chinese characters. The task of Chinese text recognition is, given the localized regions including text, to convert each region into machine-encoded text. It is an important technique for understanding text information in 2D images, and many other applications such as text-to-speech, machine translation, text mining, etc.

## PROGRESS
### WEEK 1

#### Overview

* 搭建项目网站
* 阅读相关论文

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

#### Overview

* 确定了人员的分工
* 尝试了一种文本检测的方案
* 在Windows下配置opencv开发环境

#### Details

经过前两周的探讨，我们基本确定了人员的分工：王硕和沈钊主要负责文本检测模块，周乘和孙保杰主要负责文本识别模块。

在查找文本检测方面的文章时，我们看到一篇文章叫《Real-Time Scene Text Localization and Recognition》，这是2012年CVPR上的一篇文章，进一步的搜索发现，作者在这篇文章中提出的自然场景下的文本检测算法*The "Neumann-Matas algorithm" *已经有人在opencv_contrib库中实现了。于是我们的想法是先使用这个方法跑一个demo看看检测效果，再决定要不要基于这种检测方法做后续的文本识别。

于是我们开始在Windows平台上安装编译opencv_contrib库和配置Python环境。Python环境的配置相对简单，opencv_contrib库只能通过源码编译安装，所以过程就相对麻烦一点。整个环境的配置主要分为以下几步：

1. Python 3.5的安装
2. Visual Studio 2015的安装
3. Cmake的安装
4. Opencv 3.2和opencv_contrib的源码编译

安装和配置细节在这里就不详细说明，折腾了好几次才最终配置成功。当cv2和cv2.text模块都能正常导入不报错时，证明环境配置基本完成。

![import_success](http://o9zemtn5i.bkt.clouddn.com/import_success.JPG)

配置完环境后，我们就用Python写了文本检测的Demo脚本，使用别人训练好的数据，看看检测效果。我们试了几张图片，检测效果如下：

![dres_1](http://o9zemtn5i.bkt.clouddn.com/dres_1.JPG)



![dres_2](http://o9zemtn5i.bkt.clouddn.com/dres_2.JPG)

可以看出，该方法的检测效果并不理想，把很多不是文本区域给框出来了，虚警率太高，并且有很多参数需要调，对中文的检测效果也不理想，所以我们决定再寻找其他文本检测方法。

#### References

1. Neumann L., Matas J.: Real-Time Scene Text Localization and Recognition, CVPR 2012
2. Opencv Documentation: [Class-specific Extremal Regions for Scene Text Detection](http://docs.opencv.org/3.0-beta/modules/text/doc/erfilter.html)
3. Github opencv_contrib: [modules/text/samples](https://github.com/opencv/opencv_contrib/blob/master/modules/text/samples/textdetection.py)
4. [OpenCV install opencv_contrib on Windows](http://stackoverflow.com/questions/37517983/opencv-install-opencv-contrib-on-windows)
5. [Building and Installing OpenCV with Extra Modules on Windows 7 64-bit](https://putuyuwono.wordpress.com/2015/04/23/building-and-installing-opencv-3-0-on-windows-7-64-bit/)





### WEEK 4

#### Overview

* 尝试了CTPN文本检测模型
* 寻找中文识别解决方案

#### Details

由于上周尝试的一种文本检测效果不太理想，所以这周我们继续寻找文本检测的解决方案。上周我们尝试的方案是一种bottom-up的思路，通常是利用底层的字符或笔画特征来找到可能区域，再做非文本区域的过滤，文本线的构建和确认等，事实证明这种方法不够鲁棒，这周我们决定换种思路，于是我们尝试了这篇论文中提到的CTPN模型：《Detecting Text in Natural Image with Connectionist Text Proposal Network》。该方法利用了VGG16模型的最后一层卷积层的特征，借鉴了经典的目标检测模型Faster R-CNN的方法，将整个文本区域的检测划分成一个个等宽的小块，最后再做融合。大致的pipeline分为以下三个部分：

1. Detecting Text in Fine-scale Proposals
2. Recurrent Connectionist Text Proposals
3. Side-refinement

下图是文章中提出的模型的检测框架：

![architecture](http://o9zemtn5i.bkt.clouddn.com/artectiure.JPG)

我们尝试在作者给出的[demo网站](http://textdet.com/ )上试了几张图片，发现检测效果确实不错。于是我们决定采用CTPN模型作为我们项目的文本检测方案。下面是检测效果：

![11](http://o9zemtn5i.bkt.clouddn.com/11.JPG)

![12](http://o9zemtn5i.bkt.clouddn.com/12.JPG)

可以看到检测效果比上周的方法好很多，并且模型的鲁棒性很好。对于我们的项目来说，一个好的文本检测结果对后续的文本识别至关重要。

确定了文本检测方案，接下来就是确定文本识别方案。结合中文的结构、类别众多等特点，我们如果自己建字库来训练的话，工作量可能会很大。所以我们在网上寻找中文识别的解决方案，最终确定了目前由谷歌维护的开源的文字识别引擎Tesseract，并且采用了其最新版本，即Tesseract 4.0，识别准确率相对于前面的版本有一定的提升。

我们从源码编译安装了Tesseract 4.0，做了一些文本识别尝试，发现对英文的识别效果不错，对中文的识别效果要看图片背景，当背景太杂或者文本太倾斜的话，识别效果就不太理想，这也是我们后期需要在调用Tesseract进行识别时需要进行优化的地方。

#### References

1. Zhi Tian,Weilin Huang, Tong He: detecting text in natural image with connectionist text proposal network, ECCV, 2016
2. Shaoqing Ren, Kaiming He, Ross Girshick: Faster R-CNN: Towards Real Time Object Detection with Region Proposal Networks, TPAMI, 2015
3. Github : [Tesseract-ocr](https://github.com/tesseract-ocr/tesseract)
4. Github: [CTPN](https://github.com/tianzhi0549/CTPN)





### WEEK 5

#### Overview

* 系统的集成与调试
* 系统优化
* 报告撰写与制作PPT

#### Details

这周我们开始讲文本检测模块和文本识别模块进行集成，系统主要采用的是Python语言，所以Tesseract也是调用的Python接口，使用到了Github上别人写的开源封装库[PyOCR](https://github.com/jflesch/pyocr)，整个系统是运行在Linux上，文本检测用到了CTPN模型作者已经训练好的训练数据。跑了几张图片后，发现英文的识别准确率还可以，但是对中文的识别准确率还是不太好，从网上查阅资料后，认为对于中文识别准确率不高这一问题，一方面是Tesseract识别引擎本身对中文的识别准确率就不太好，另一方面可能是因为自然背景和文本的倾斜等因素造成的，所以我们决定在对文本进行识别之前，对检测到的文本先进行一些预处理，比如先使用大津法进行二值化处理，再进行倾斜校正，可能会对识别准确率有所提高。

#### References

1. [Improving the Quality of the output](https://github.com/tesseract-ocr/tesseract/wiki/ImproveQuality)
2. [PyOCR](https://github.com/jflesch/pyocr)

## DEMO
demo video: <iframe height=498 width=510 src='http://player.youku.com/embed/XMjcwMDEyNTQ3Mg==' frameborder=0 'allowfullscreen'></iframe>

## TEAM MEMBERS
| Name |     ID     |       Contact        |
| :--- | :--------: | :------------------: |
| 王硕   | M201671781 | wangshuotzjz@163.com |
| 沈钊   | M201671775 | hustershen@gmail.com |
| 孙保杰  | M201671796 |   176172955@qq.com   |
| 周乘   | M201671807 |   294058634@qq.com   |
