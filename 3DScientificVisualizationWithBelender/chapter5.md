# 动画

使用Blender创建的科学可视化的输出可以包括3D散点图，3D场景的静态图像，或者有或没有支持相机移动和跟踪的3D动画。

## 5.1 关键帧

为了便于网格对象的动画，必须在关键帧中记录X，Y，Z位置，旋转和缩放。关键帧在GUI底部的动画播放栏中通过与所选对象相关的黄线进行注释。
通过I键插入关键帧——可能的选择在图5-1中显示和描述。通过组合键ALT-I删除关键帧。
使用关键帧的一个优点是Blender将在帧之间自动插值——不需要为网格对象的动画中的每个帧设置关键帧。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/5-1.png?raw=true)

    图5-1.按下I键允许用户键入对象，该对象在可视化期间插入记录对象的位置方面的关键帧。
    
### 5.1.1 示例：如何旋转和动画对象

下面的示例显示如何插入旋转关键帧以围绕X轴旋转圆环（在默认3D视图空间中为红色），并使用曲线图编辑器编辑它们，以便创建平滑的运动。
动画将是300帧，每秒30帧，总持续时间为10秒。在那些10秒内，圆环网将旋转360度（图5-2）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/5-2.png?raw=true)

    图5-2。在圆环网格对象的动画期间，旋转是关键帧（黄色）。
    
*   在屏幕底部的动画工具栏上将结束帧数设置为300.将当前帧设置为1。

*   单击Add——>Mesh——>Torus创建圆环网格。或者，可以使用键盘快捷键SHIFT-A。

*   打开变换工具栏（小加号+）

*   将环面关键帧设置为0°旋转（I键，然后是弹出菜单上的旋转）。或者，用户可以右键单击变换工具栏中的旋转坐标，然后选择插入关键帧。黄色垂直线将放置在窗口底部的动画工具栏上。

*   将绿色动画滑块移动到第150帧。

*   在变换工具栏中将X旋转值更改为180度。右键单击该值并插入另一个旋转关键帧。

*   最后，将绿色动画滑块移动到第300帧，将X旋转值更改为360度，然后右键单击并插入另一个关键帧。

*   使用动画工具栏上的右箭头播放按钮播放动画。

### 5.1.2 示例：使用图形编辑器

请注意，圆环的旋转在早期帧中上升，然后在结束时减慢。如果我们想要使这个动画循环平滑移动，则需要恒定的速度。图表编辑器可用于调整旋转。

*   在主窗口中打开Graph Editor（图5-3）

*   在Graph Editor窗口的底部，选择View——>View All。

*   按V键并将Keyframe Handle Type设置为矢量。

*   返回默认视图中的3D视口并重放动画，动画应在动画时间线上指定的帧数内以恒定速度运行。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/5-3.png?raw=true)

    图5-3。Graph Editor允许用户在Blender中可视化地控制动画。通过更改这些曲线的形状，可以创建可视化期间的平滑动画。

### 5.1.3 示例：添加循环修改器

在前面的示例的基础上，我们可以让Blender通过向图表编辑器添加循环修改器来自动重复可视化。

*   单击屏幕右侧的Add Modifier

*   选择Cycles。锯齿波形将出现在图形编辑器中。

*   图5-4显示了如何添加循环修改器以及最终图形应该是什么样的进展。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/5-4.png?raw=true)

    图5-4。这三个面板显示了围绕X轴（红色曲线）的旋转矢量化，添加了一个循环修改器以重复动画
    
圆环现在将在视频渲染期间重复关键帧运动，与长度无关。

## 5.2帧速率和渲染输出

渲染视频取决于硬件资源和可视化目标。对于广播质量的高清输出，应使用最高分辨率和视频编解码器。对于可视化开发期间的实验渲染，
可以使用不具有所有渲染元素的较低分辨率。

渲染选项卡中的标记功能对于记录有关可视化——帧速率，渲染时间，作者以及最终合成中使用的场景组件的元数据非常有用。

### 5.2.1输出格式

单帧图像可以以多种格式导出。对于高质量视频输出，可以使用AVI JPEG或AVI RAW作为初始步骤。如果需要，可以转换视频。
FFMPEG是适用于任何操作系统的有用视频转换实用程序。下面的示例将AVI文件转换为Quicktime Prores：

    ffmpeg -i file.avi -vcodec prores -profile 3 -an file.mov
    
## 节点合成

节点编辑器是一个功能强大的Blender功能，允许用户以图形方式访问API的元素，
以及在将不同的可视化图层提供给渲染引擎之前将其组合成一个最终合成。可以通过GUI左下方的编辑器下拉选择器访问节点编辑器。

节点表示具有输入和输出的后处理中的特定特征或动作以及可以修改的关键字参数。节点本质上是Python函数的图形表示。
示例节点如图5-5所示。一旦它们到位，它们就会从一个输出端口连接到另一个节点的输入端口（图5-6）

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/5-5.png?raw=true)

    图5-5。用于合成的示例节点。每个节点都有成像输入和输出，可以通向连接节点。这里的示例节点给出了开始渲染层，RGB颜色校正，混合节点到符合层以及连接到渲染引擎的输出节点。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/5-6.png?raw=true)

    图5-6。此窗口显示三个渲染层被组合并通过颜色平衡和RGB节点，然后传递给合成器。可以通过使用鼠标左键从一个输出到另一个图像输入绘制来连接节点。可以使用CTRL键销毁线，并使用X键删除节点。
