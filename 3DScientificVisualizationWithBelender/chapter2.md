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
