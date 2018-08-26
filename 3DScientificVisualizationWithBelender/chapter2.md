# 界面和窗口设置

## 2.1 界面介绍

对于本书中的练习和描述，我们将使用加载Blender的默认GUI。默认情况下，使用Blender加载的界面面向在3D视图端口，
动画栏和对象元数据中操作数据的工作流程，以及材质，纹理和其他对象和网格属性。我们将使用Python API和编写数据加载脚本来扩充它。
界面高度可定制，鼓励读者根据其特定的屏幕设置，首选项和可视化目标来试验GUI组织。主GUI在图2.1显示和描述。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-1.png?raw=true)

    图2-1。用户在启动时将看到的默认Blender GUI。主3D试图端口允许用户以交互方式操作和创建3D对象。左侧显式“对象工具”工具栏，右侧显示“变换”工具栏。屏幕底部显示动画“磁带卡座”，最右侧显示数据大纲和渲染，材质和纹理工具。  

### 2.1.1 3D视图端口

3D视图端口是与实际数据对象进行大部分交互的地方。当Blender首次打开时，默认视图会在原点显示三个箭头（图2.2）。
它们分别用于X轴，Y轴和Z轴的红色，绿色和蓝色。要围绕轴中心的白色圆圈旋转场景，请单击，按住并拖动鼠标中键。
向前和向后旋转鼠标中键将放大和缩小场景。场景的默认视图是透视的，
因为平行线将会汇聚到水平线上消失点（正如在单点或双点透视中创建的图形中的道路将会聚到远点）。
鼠标左键可在观察端口周围移动红色和白色3D光标。任何新的网格或对象都将添加到3D光标的位置。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-2.png?raw=true)

    图2-2.具有原点箭头三元组的网格立方体分别用于X轴，Y轴和Z轴的红色，绿色和蓝色。控件可用于平移，旋转和缩放。用户可以抓住这些手柄来操纵整个网格对象，或单个顶点，线或面。
    
### 2.1.2使用键盘

键盘快捷键对于将3D数据处理到所需位置至关重要。图2-3显示了数字键盘以及每个键可以执行的操作。
1,3,7键将显示顶视图，低视图和侧视图。2,4,8和6将以15° 的增量旋转视图。使用键盘有利于精确控制和定位视图。
中央5键在正交和透视图之间切换。这两种观点在规划和执行科学可视化方面都具有优势。键盘快捷键列表见附录A。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-3.png?raw=true)

    图2-3。标准键盘的示意键盘视图。2,4,6和8键允许用户以15°增量旋转所选对象或数据容器。3,1和7键分别沿X，Y和Z轴的视线移动视图。5键可在正交和透视模式之间切换3D视窗。

通过平移，旋转和缩放（图2-4），可以在GUI中或通过键盘快捷键进行对象移动。GUI的底部有许多下拉菜单，
用于在对象和网格编辑模式之间切换，选择渲染样式（线框，实体，纹理，完整）和更改参考框架（图2-5）。
在网格编辑模式下，可以选择单个或一组顶点，线条或面（图2-6）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-4.png?raw=true)

    图2-4。显示鼠标手柄的三个Blender网格对象（a）平移，（b）旋转和（c）缩放。这些GUI元素允许用户在3D视口中操纵和定位对象。
    
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-5.png?raw=true)  

    图2-5。模式选择的下拉菜单。本书中的练习将主要关注对象和网格编辑模式。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-6.png?raw=true)

    图2-6。左侧面板以对象模式显示UV球体。右侧面板显示在网格编辑模式下选择单个面的相同球体。
    
### 2.1.3 四视图

一个特别有用的GUI设置是四边形视图。这可以通过CTRL-ALT-Q完成，然后显示沿X,Y和Z视线的等距视图，以及来自活动摄像机的视图（图2-7）

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-7.png?raw=true)

    图2-7。示例Blender GUI以四边形视图配置，显示具有顶视图，侧视图和前视图的碰撞星系的N体模拟，以及来自当前所选摄像机的视图。四边形视图对于从多个方向进行可视化分析以及最终渲染的视野将显示的内容非常有用。

### 2.1.4 UV视图

可以通过Blender屏幕顶部的下拉菜单访问UV编辑GUI。这将主窗口界面拆分为3D视图端口和UV编辑界面，可在数据映射到网格曲面时使用。
这通常通过使用TAB键进入网格编辑模式来完成，右键单击以选择所需的顶点，然后按U键以使用其中一种投影方法将图像映射到曲面。

### 2.1.5 对象工具工具栏

对象工具工具栏与TAB键一起允许用户修改网格对象的属性。工具栏将根据所选对象而改变;所选摄像机或照明元素的属性将与网格对象不同（图2.8（a））。
在后面的部分中，我们将利用方位角对称性并演示旋转工具（和其他工具）在建筑模型中节省时间的效用

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-8.png?raw=true)

    图2-8。对象工具和变换工具栏，用于在3D视图端口中精确定位对象和光标。每个对象属性都可以从变换工具栏进行关键帧设置。通过按TAB键并进入网格编辑模式，可以通过对象工具工具栏操作各个元素。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-9.png?raw=true)

    图2-9。Blender中的数据大纲提供它们之间所有对象和关联的全局视图。子/父关系，渲染状态和3D视图端口状态都反映在这个有用的GUI元素中。

### 2.1.6 变换工具栏

变换工具栏是GUI的重要组成部分，它允许用户精确地操纵和细化给定对象的位置，旋转和缩放。这通常与键盘控件一起使用。
可以锁定网格位置并移动3D光标。观察距离称为剪辑，可以控制用户可以看到的当前3D视点的距离（图2.8（b））。这在拥挤的可视化场景中特别有用。

### 2.1.7 数据大纲

数据大纲（图2-9）显示了场景中存在的所有对象。它以分层格式描绘了哪个摄像机处于活动状态以及哪些对象当前正在为场景做出贡献。
可以在场景和/或最终渲染中打开或关闭对象。这对于在GUI中具有最终渲染中不存在的对齐指南非常有用。
具有共同属性的对象可以组合在一起以便于组织。使用可视化场景时，用户可以选择哪些元素在3D视口中可见或在最终渲染中是必需的。
构建场景时，某些对象可能用于对齐或引用，但不会在动画或渲染中使用。在其他时候，
特定的参考数据对象可能计算成本太高而无法放入测试渲染并暂时从场景中排除，直到创建最终渲染。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-10.png?raw=true)

    图2-10。属性面板，可以操作材质，纹理，渲染和场景控件。从左到右，面板包括渲染，图层，场景，世界控件，对象控件，约束，修改器，顶点控件，材质，纹理，粒子生成器和物理模拟。
    
### 2.1.8 属性面板    

属性面板是GUI右侧的多标签面板。其中包括渲染选项卡，图层，场景，世界控件，对象控件，约束，修改器，顶点控件，
材质，纹理，粒子生成器和物理模拟（图2-10）。此处列出的某些选项卡将更详细地介绍，因为后续部分需要它们的功能。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/2-11.png?raw=true)

    图2-11。Blender动画时间线，允许缩放动画的帧以及查看关键帧标记。从左到右，按钮指示开始和结束帧，当前帧，开始帧，前一个关键帧，反向播放，前向播放，下一个关键帧和动画结束。
    
[Render]() 此选项卡设置可视化大小和分辨率，文件动画类型和元数据。预设适用于常见的视频类型和格式。

[Scene layers]() 此选项卡用于在可视化场景中屏蔽和消隐图层。

[Scene properties]() 可以在此处设置基本场景属性，包括缩放单位。这也是控制刚体物理模拟参数的选项卡。

[World]() 可以在此处控制设置环境光和背景颜色，具体取决于可视化是作为日志中的图形发布还是更适合高清动画。

[Object]() 可以在此处控制GUI配置以及与其他对象和层的关系。

[Constraints]() 网格物体可以相对于其他物体或定义的标准限制或锁定其运动或位置。这在构建摄像机轨道时很有用。

[Modifiers]() 修饰符可用于扩展数据对象，而不会增加顶点或多边形的数量。在制作需要快速渲染的网格时，这非常有用。

[Vertex groups and shape keys]() 形态键对于具有大量粒子的关键帧动画非常有用，包括平滑粒子流体动力学和N体模拟。

[Materials]() 科学可视化的一个重要部分涉及在表面，网格和点材料之间进行选择，以传达各种可视化元素。这些可能包括一系列现象，例如行星表面，场线或通过3D散点图最佳传达的数据

[Textures]() 纹理对于加载地形图和3D数据立方体是有用的，其可以分别被带入分层纹理或体素数据结构中。

[Particles]() 内置的Blender粒子发生器可用于N体和平滑粒子流体动力学（SPH)模拟的现象学模型和示踪剂。

[Physics]() 该引擎允许用户设置力，重力和磁场，并使用刚体和软体动力学特征。

### 2.1.9 动画时间线

为了便于移动模拟或动画，Blender GUI提供动画控件（图2-11）。动画时间线类似于一组经典的磁带卡座控件。
快退，播放，快进，单击并拖动以查看3D视口中发生的所有动画和摄像机移动。此外，一旦设置了关键帧并选择了一个对象，
用户就可以在这些关键帧之间跳转，并在时间线上将它们视为黄线。
