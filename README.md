本开源系统已经迁移到  https://git.openi.org.cn/OpenIOSSG/PLabel  进行开发，这里可以存储超大文件，包括docker镜像，可以直接下载。

# PLabel V3.0
预计将在2021年12月底发布，将彻底修改架构，将各个自动标注算法全docker化，按照指定的流程发布标注算法的docker，就可以接入标注系统。另外将增加文本标注功能。

# PLabel V2.0
PLabel发布, 主要是升级mmdetection平台到V2.3.0,重训人车检测模型，提升了检测精度，同时增加自动分割，集成人工标注的主动分类学习功能，标注工具栏中增加一些功能及一些Bug的解决,同时一并发布系统的详细设计文档及详细的系统帮助说明文档，请参见doc目录下文档。

# PLabel
半自动标注系统是基于BS架构，纯Web页面操作，由鹏城实验室叶齐祥、曾炜、田永鸿教授团队自主研发，由工程师邹安平维护，集成视频抽帧，目标检测、视频跟踪、ReID分类、人脸检测等算法，实现了对图像，视频的自动标注，并可以对自动算法的结果进行人工标注，最终得到标注结果，同时也可以对视频、图片、医疗（包括dicom文件及病理图像）相关的数据进行人工标注，标注结果支持COCO及VOC格式。支持多人协同标注。 半自动标注系统主要功能有：用户管理，数据集管理，自动标注，人工标注，ReID标注，车流统计，视频标注，医疗CT标注，超大图像标注，模型管理与重训，报表管理。数据标注过程一个非常重要的因素是数据安全，在标注使用中防止数据泄露，采用基于web标注工具是有效避免数据泄露的措施之一。 半自动标注系统以保证性能的情况下最小化人工标注代价为目标，不断提升自动标注效率，减少人工标注和人工参与过程。

# Install
新增最简单的方式安装：
下载此docker镜像：
https://git.openi.org.cn/attachments/a2628b9b-fcff-4e8b-9b84-696f1d8c88ad

加载镜像
docker load --input pcl_label_hand_v1.tar

运行容器：
docker run  --name PLabelHand -p 8008:8008 --shm-size 4G -i -t   -v /data1/PLabelHand:/data 8b0262004872 /bin/bash

注意：必需使用8008端口映射，如果要修改，则需要进入docker容器修改nginx下plabel.conf文件中的端口。

进入容器：
docker exec -it PLabelHand /bin/bash

运行系统：
./server.sh
再进行回车，一直到出现命令界面。

在web浏览器中输入：
http://ip:8008/
访问标注系统。 初始用户名：LabelSystem01  /  pcl123456



其它安装方式：
参见doc目录下环境搭建word文档，本标注系统将会一直维护下去，有疑问、使用过程发现Bug、或者想要新增功能，都可以直接联系zouap@pcl.ac.cn，工作日一般会及时回复。

# Usage
1、用户管理，只有管理员LabelSystem01帐号可以创建用户，分管理员、标注人员、审核人员三类。 在用户管理中可以对每个标注人员进行分表存储标注数据（适合大批量标注，100W级以上），也可以将所有用户的操作的标注数据都存储在同一张表中。

2、数据集管理
   支持视频(需要ffmpeg能支持的格式)、CT影像(.ima,.dcm)、超大图像(.svs，.tiff)、通用图片（常见的图片格式都能支持）  4种类型的数据。视频支持抽帧，抽帧可以指定文件名方式，也支持视频合并，4种类型数据都可以支持预览。视频目前只有mp4格式可以在线预览，其它的视频需要抽帧之后才能预览。

（注：视频及超大图像需要docker镜像的支持）

3、自动标注
   自动标注目前集成了人、车的几种开源算法，基于mmdetection平台，搜集所有开源数据集训练得到的目标检测模型，新建自动标注时，需要选择数据集，然后等待自动标注完成后，可以查看标注结果。
   自动标注也可以集成开发者的自己的模型，需要开发者在数据库表中插入对应的模型位置及运行脚本，当然模型的输出结果需要与系统读取的数据格式一致。

4、人工标注
  人工标注可以基于自动标注的结果，也可以对数据集中的数据进行直接标注，包括点、多边形、矩形三种标注。人工标注以任务的方式存在，可以看到当前标注的进度。
  在人工标注页面的按钮中，还可以进行单目标跟踪、多目标跟踪、自动单个图像目标检测。
  包括复制、粘贴、上下左右移动等功能按钮。
  对于人工标注任务在标注人员标注完成后，还可以转给审核人员进行审核。

5、ReID标注
   可以对多个镜头进行ReID标注，在标注页面可以看到多个镜头的ReID标注，大大提升效率，同时查看ReID结果页面会列出所有的ReID标注结果，还可以直接对结果进行修改，删除。也可以对结果进行导出，包括抠图导出，原图导出，标注导出。

6、车流统计
   操作MP4格式的视频，可以在视频上对车辆行驶方向进行标注。

7、视频标注
   操作MP4格式的视频，可以在视频上对目标进行标注。

8、超大图像标注
   对tiff及svs格式的图像进行标注，集成了openseadragon组件，并进行了二次开发，以便可以进行标注。

9、报表管理
   对标注人员的工作进行度量。

10、操作日志管理
  用户登录、登出日志查看。
  
11、模型重训
  可以基于mmdetection使用用户标注的数据重训人、车的检测模型。

标注请使用chrome浏览器，相关截图可参见doc目录下《半自动标注系统介绍_V1.4》，有疑问可以直接联系zouap@pcl.ac.cn，工作日一般会及时回复。

