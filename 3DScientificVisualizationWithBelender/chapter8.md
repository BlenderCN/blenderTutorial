# 项目和3D示例

最好通过一系列示例扩展本书中提供的信息概述。每个示例将使用Blender界面的不同部分来实现不同风格的科学可视化。
将提供示例blend文件，数据文件和视频以更好地说明这些功能中的一些。

## 8.1 3D散点图

3D散点图可用于显示多个参数中的趋势或3D空间中对象的位置。在这个项目中，来自Hipparcos项目的星星将以3D形式显示。使用的概念将是：

*   使用Blender Python API读取格式化数据。

*   设置笛卡尔网格

*   围绕数据集移动相机。

以下步骤将设置可视化。

*   使用Add——Mesh——Plane添加平面。使用S键缩放平面，然后按TAB键进入网格编辑模式。将平面细分五次并再按一次TAB再次返回对象模式。

*   在属性面板上将材质添加到平面网格物体，并将类型设置为线。选择与背景颜色形成鲜明对比的颜色——深蓝色通常效果很好。

*   将属性面板上的世界选项卡背景水平颜色设置为黑色。

*   使用Add——Mesh——Circle添加一个简单的网格。按Tab键进入网格编辑模式，SHIFT选择除一个顶点之外的所有顶点，然后按X键将其删除。再按一次TAB返回对象模式。

然后，我们使用以下Python脚本导入Hipparcos目录：

    import bpy
    import math
    import bmesh
    import csv
    
    obj = bpy.data.objects['Circle']
    if obj.mode == 'EDIT':
        bm = bmesh.from_edit_mesh(obj.data)
        for v in bm.verts:
            if v.select:
                print(v.co)
            else: 
                print("Object is not in edit mode")
            bm = bmesh.from_edit_mesh(obj.data)
            
    # Read in Hipparcos data
    filename = 'hygxyz.csv'
    fields = 
    
*   数据应该快速加载。我们现在可以向Hipparcos数据点添加材料。 

*   选择数据点，然后单击属性面板的材料选项卡。

*   选择Halo并将尺寸值更改为0.005,硬度值更改为100.可以使用白色或浅黄色对点进行着色。

我们现在可以使用默认的相机对象指向空对象，如6.2.1节所述。

*   使用Add——Empty——Plain Axes添加一个空对象，以便跟踪摄像机（图8.1）。
 
*   右键单击以选择相机对象，并将变换工具栏上的位置和旋转值设置为零。

*   单击右侧属性面板上的约束选项卡。

*   选择追踪到并选择目标为空。

*   选择To作为-Z，Up为Y。这将在查看摄像机视野时正确定向向上和法线方向。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-1.png?raw=true)

    图8-1。设置空跟踪对象。在飞越Hipparcos目录期间，相机将指向此物体。
    
通过对相机进行关键帧设置来为可视化设置动画。

*   这个动画动画将是20秒长，每秒30帧。因此，将帧数设置为600并将当前帧设置为1。

*   右键单击选择摄像机，然后按I键为摄像机的位置和旋转设置关键帧。

*   在动画工具栏上，将当前帧设置为600。将3D视图端口中的摄像机移动到其他位置和方向。

*   使用I键最后一次设置摄像机位置和旋转关键帧。

现在可以使用1080p高清视频导出可视化。

*   在属性面板的渲染选项卡上，选择HDTV 1080p并将帧速率设置为每秒30帧。   

*   将输出设置为AVI JPEG，质量为100%,并指定唯一的文件名。单击选项卡顶部的动画按钮以渲染可视化。最终动画的帧如图8-2所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-2.png?raw=true)
    
    图8-2。Hipparcos目录的3D渲染，利用光环材料，细分网格和相机追踪。

## N体模拟

在这个例子中，我们将为读者提供来自星系碰撞模拟的N体数据。我们的想法是扩展初始化散点图示例，
方法是将N体点放入形态键以提高性能，然后迭代每个快照以完成动画。使用的概念是：

*   使用Blender Python API读取多个格式化数据文件

*   设置笛卡尔网格。

*   将每个时态快照与形态键相关联。

*   将光晕材质添加到数据点。

*   移动摄像机跟踪到星系的质心，直到星系碰撞的特定时间。

这个星系模拟的例子是用GADGET-2是一种非常有用的N体和平滑例子流体动力学包，用于天体物理学。其功能范围从大规模结构宇宙学模拟到星系碰撞。该示例中的模拟运行大约1100个时间步，总运行时间为20亿年。每个大型螺旋盘星系都有10000个圆盘粒子和2万个具有银河系尺度长度和质量（2.1千克和10^9太阳质量）的光晕粒子。粒子位置数据显示为具有晕圈材质的单个顶点。当相机对象沿Bézier曲线飞行时，每个快照都是关键帧。动画中的帧如8.3所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-3.png?raw=true)

    图8-3。显示两个星系之间碰撞的N体模拟的3D渲染。

模拟生成的ASCII快照文件采用以下简单的X，Y，Z逗号分隔值格式：

    -44.4367,-44.1049,-0.539139
    -94.6014,-74.8028,-0.5839
    -70.9394,-77.0528,-0.357556
    -58.1572,-57.4876,-0.917166
    -63,3899,-61.3828,-0.793917
    ...
    
首先，需要在脚本中创建一个形态键（对于此模拟），需要20000个顶点：

    import bpy
    import bmesh
    # Switch to object mode
    bpy.ops.object.mode_set(mode = 'OBJECT')
    # Create initial Basis Shape Key
    bpy.ops.object.shape_key_add()
    # Create initial Shape Key
    
然后使用csv和glob Python模块读入快照数据文件，顶点将添加到单个顶点圆对象，就像在3D散点图示例中创建的网格一样。    

    import glob
    import csv
    import bpy
    import bmesh
    file = glob.glob('*.txt')
    file.sort()
    
添加每个形态键的关键帧：

    # key a single frame per data file
    for j in range(1,frames+1):
        if keyname != os.path.basename(files[framecount-1\):
            bpy.data.shapt_key[0].key_blocks[keyname].value = 0
            
我们现在将顶点更改为具有小高斯球体的材质。

1.选择圆形对象后，单击属性面板中的材料选项卡以使用光晕选项创建新材质。

2。将光晕大小更改为0.05。

3.将硬度更改为100.

4.根据你的喜好修改所需的光晕颜色。

由于两个星系都在移动，我们将为一个空物体设定关键帧，以便在碰撞前跟踪一个星系的质心。随着碰撞的发展，
空物体将减速到静止位置（图8-4）。在整个动画过程中，相机也将移动并跟踪空对象。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-4.png?raw=true)

    图8-4.一个空物体被放置在其中一个星系的质心处，并在相机跟踪过程中移动关键帧。
    
1.使用Add——Empty——Plain Axes添加一个空对象，使摄像机指向。

2.将当前帧设置为零，将空对象放置在galaxy3的中心附近。并使用I键设置关键帧位置。

3.通过碰撞的第一部分播放动画，然后停止。将空对象移动到碰撞质心附近的新位置，然后添加另一个关键帧。

4.右键单击以选择相机对象，并将变换工具栏上的位置和旋转值设置为零

5.单击右侧属性面板上的约束选项卡。

6.选择Track To并选择目标为Empty。

7.选择To作为-Z，Up作为Y。浙江在查看摄像机视野时正确定向向上和法线方向。

最终动画中的快照和不同摄像机视图如图8.5所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-5.png?raw=true)

    图8-5.来自N体星系碰撞模拟的帧
    
## 磁场

在这个项目中，我们将为电流回路产生磁势的三维可视化。使用的概念将是：

*   在Python中评估适当的磁势方程。

*   使用拉伸为当前循环添加模型。

*   使用顶点编辑模式绘制轮廓。

*   由于等式的方位对称性，旋转轮廓。

*   为潜在的轮廓添加彩色线材。

从经典动力学中我们知道，电流密度J在半径为a的环中具有方位角分量。回顾这些基本方程式，我们有：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/81.png?raw=true)

矢量势的解决方案写为：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/82.png?raw=true)

由于配置的对称性，我们可以将方位角分量写为：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/83.png?raw=true)

其中K(k)和E(k)是完全椭圆积分。可以通过以下近似扩展解决方案：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/84.png?raw=true)

我们将评估此表达式，计算轮廓并使用顶点编辑模式绘制轮廓。由于场景是方位角对程的，我们可以使用Blender旋转工具围绕Z轴旋转磁势曲线。
最后，我们将添加当前循环并为可视化设置动画。生成8.4中的数据i并通过以下Blender Python脚本读入：

    import pickle
    import numpy
    import bpy
    import bmesh
    import csv
    obj = bpy.data.objects['MagneticField']
    
使用拉伸工具创建电流环。

*   使用Add——>Mesh——>Circle添加网格。

*   使用E键挤出顶点，然后单击鼠标。

*   使用S键缩放新的顶点集，然后使用TAB键返回到对象模式

*   在变换工具栏上，将尺寸设置为1.5个单位，以匹配模型中使用的值。

*   使用蓝色材料为当前循环着色。

由于方位角对称性，可以通过旋转工具扩展轮廓（图8-6）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-6.png?raw=true)

    图8-6.沿每个轴的四视图以及磁势的摄像机视图。
    
*   选择磁势轮廓网格。    

*   使用TAB键进入网格编辑模式。

*   从左侧的网格工具栏中选择旋转

*   将步数设置为6,角度设置为360,中心设置为零，Z轴设置为1.0。检查复制框，这将创建一组方位角对称的轮廓。

*   使用TAB键退出网格编辑模式，并使用红色材料为潜在轮廓着色（图8-7）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-7.png?raw=true)

    电流环的磁势的3D渲染
    
## 拉格朗日平衡和零速度曲线

我们可以计算和绘制两个质量系统的拉格朗日平衡点和零速度曲线。该项目将使用以下概念：

*   将曲线渲染为3D旋转曲面。

*   介绍Blender材质的透明。

*   将网格模型复制到计算位置。

这里考虑的公式在太阳系动力学的第三章中概述。我们考虑质量为m1和m2的圆形轨道中的两个质量关于原点O处的共同质心。
质量之和和它们之间的恒定间隔都是一致的。q是质量的比例，我们定义：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/85.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/86.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/87.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/88.png?raw=true)

三角拉格朗日点L4和L5的位置可以求解：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/89.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/810.png?raw=true)

共线拉格朗日平衡点推导为：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/811.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/812.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/813.png?raw=true)

我们可以通过使用Newton-Rhapson方法来求解向量r1和r2,进一步细化这些共线拉格朗日点。
这些例子包含在图书馆网站上。接下来，我们提供解决这些问题的代码

    import math
    import os
    import csv
    import numpy as np 
    from scipy.optimize import fsolve
    import numpy as np
    from scipy.optimize import newton
    import matplotlib.pyplot as plt
    
然后我们可以将X和Y对读入Blender网格中以绘制轮廓。首先，我们在GUI中创建一个名为RocheOutline的顶点网格。  

    import csv
    import glob
    import os 
    import bpy
    import bmesh
    
创建轮廓后，我们可以使用镜面修改器添加对称轮廓的第二部分，然后围绕X轴旋转轮廓。

*   选择罗氏轮廓并在X轴上添加镜面修改器。

*   使用TAB键进入网格编辑模式

*   选择左侧网格物体工具面板上的旋转工具，将度数设置为360,将步数设置为40.将X，Y和Z轴分别设置为1.0,0.0和0.0,使X成为旋转轴。取消选中Dupli框，以便创建曲面。

最后，表面可以制成透明。这在需要在另一个网格中看到对象的可视化场景中非常有用

*   在属性面板上添加红色表面材质。

*   将漫反射强度更改为1.0

*   将阴影发射和半透明设置为0.0

*   选中透明度框，单击Z透明度（以便更快地渲染）并将Alpha值设置为0.35（图8.8）。

*   在对象工具部分中，选择平滑着色。这样可以在没有硬边的情况下渲染曲面。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-8.png?raw=true)

    图8-8.可以使用网格编辑模式中的旋转工具对零速度曲线进行映射，然后在3D中旋转。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-9.png?raw=true)

    图8-9.初级和次级质量可以在各自的位置添加简单的彩色UV-spheres，拉格朗日点也可以指定为小四面体（图8-9）。
    
## 8.5地球物理学：行星表面测绘

Blender非常适合从映射数据创建3D表面。在此项目中，我们将使用Mars MOLA数字地形地形图中生成3D地表图。该项目介绍了以下概念：

*   将图像导入为平面。

*   使用位移修改器。

*   将图像映射到UV平面。

*   使用地形图和颜色底图成像来分层纹理

数据从USGS PDS成像节点获得。这两个图像的尺寸和分辨率均为1000x1000像素，每像素1.32千米。
地形图将用于Blender网格对象的曲面上点之间的相对高度。Mars basecolor贴图将用作纹理图层。

首先，我们将在File——User Preferences——AddOns下激活新功能。单击Import Images as Planes复选框，
然后单击Save User Settings复选框。这将允许我们将地形图直接成像为网格平面对象。

图像需要是灰度地形图而不是阴影浮雕图。

*   选择File——Import——Images作为平面，然后选择地形图文件。

*   按TAB键进入边级模式。

*   按W键并选择细分。如果网格划分小于图像中的像素数，则将对得到的网格进行平均。这对于快速查看可视化是可接受的，但是可以在最终渲染中以全分辨率执行。

*   在属性面板中，选择Modifiers——>Displace。选择纹理作为地形图文件。将强度（便利比例因子）设置为0.05.将纹理坐标设置为UV（图8-10）

*   在属性面板中，选择材质选项卡。将漫反射和光谱强度设置为0.0,将阴影发射设置为1.0.

*   在属性面板中，选择纹理选项卡。添加两个纹理——一个用于从地形图中移位，另一个用于表面成像。他们的类型需要设置为图像或电影，坐标设置为UV。对于表面成像，漫反射影响设置为颜色：1.0,对于地形图强度：1.0.这些设置如图8-11所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-10.png?raw=true)

    图8-10.设置位移修改器以将火星地形图加载到网格对象中。
    
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-10.png?raw=true)

    图8-11.火星地图的地形纹理设置

垂直于平面观察的最终复合材料如图8-12所示

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-10.png?raw=true)

    图8-12。火星地形合成由地图位移修改器和纹理图层生成。

## 8.6 体积渲染和数据立方体

数据立方体是由给定相空间中的横截面组成的数据集，最好将其视为渲染透明体积。这些数据在生物学，医学和天体物理学中有应用。
CAT扫描数据提供扫描对象的切片，每个元素是体积像素（体素）。来自射电望远镜的数据包括正确的提升和赤纬（天球上的位置）和频率，
可用于显示星系或化学物种在毫米波长处的旋转。

在此应用程序中，我们使用以下Blender函数：

*   将横截面切片加载入Blender

*   将数据添加为体素纹理。

*   设置材质和纹理属性以渲染体积。

对于此示例，我们将使用易于下载的人头的CAT扫描数据，并说明体积渲染的简单示例。
射电天文学和NRAO射电望远镜设施的其他优秀例子也可以在网上找到。

*   从Blender启动时加载的默认文件开始。

*   右键单击以选择默认的多维数据集对象。这将充当数据容器。

*   缩放数据容器以匹配右侧变换对话框中的数据立方体尺寸。

*   单击属性面板最右侧的材料选项卡。

*   单击+按钮添加新材料。选择体积作为材料类型。

*   图形密度已在随附的混合文件中设置。对于不同的数据，这些必须由用户适当地缩放。

*   将图形密度更改为0.0,将密度范围更改为2.0.

*   在着色下，将发射设置为0.0并散布到1.4。

以设置材质类型，现在可以将图像切片应用于网格的纹理。

*   单击属性面板上的纹理选项卡。

*   单击+新建按钮以创建新纹理。

*   从类型下拉框中选择Voxel Data。

*   选中渐变框并将最右边的渐变颜色设置为您选择的颜色。我们将在此示例中使用灰度颜色。

*   在Voxel Data部分下，打开图像序列中的第一个文件。

*   将文件格式和源更改为图像序列。

*   在Mapping下将投影更改为Cube。

*   在Influence下，选择Density，Emission和Emission Color，并将其值设置为1.0（图8-13）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-13.png?raw=true)

    图8-13。用于渲染数据立方体的设置配置。
    
数据立方体的几个视图如图8-14所示。此处概述的过程可应用于需要体积渲染以供查看的任何类型的3D数据集。射电望远镜观测的数据立方体如图8-15所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-14.png?raw=true)

    图8-14。旋转数据立方体的体积渲染。
    
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-15.png?raw=true)

    图8-15。3D渲染的星系数据立方体与美国新墨西哥州国家射电天文观测台非常大阵列的观测获得。
    
## 8-7。物体模块和刚体动力学

Blender物理模块允许用户使用内置的微分方程求解器将场添加到可视化场景并创建物理演示。该模块可以实现简单的抛射运动，
带电粒子与磁场相互作用以及简谐运动。用户可用的字段如图8-16所示。在这个项目中，我们将使用以下概念。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-16.png?raw=true)

    图8-16。使用Blender模块的用户可用的字段。可以设置网格对象以响应由于给定字段而发生的物理约束。
    
*   激活物理属性选项卡。

*   将平面网格设置为被动刚体。

*   将模拟单位缩放为SI。

*   在运行动画之前设置时间步长i并计算网格位置（称为烘焙）以提高效率。

此示例将结合许多Blender物理模块项并演示其属性。

*   通过Add——Mesh——Plane开始，然后使用S键缩放对象。

*   在属性面板上选择物理模块（图8-17）

*   选择刚体并将类型设置为被动。

*   应在平面上方Z=1.0处添加一个球（Add——>Mesh——>UV Sphere）

*   选择物理模块，选择刚体，并将类型设置为Active。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-17.png?raw=true)

    图8-17.物理菜单允许向可视化场景添加字段。这些特殊菜单显示了具有刚性和弹性值的固定被动表面的配置。地球上的重力引起的加速度设置为9.8ms-2的SI值，并且场景菜单下的微分方程求解器设置为每秒60步。
    
点击动画时间线上的播放按钮将计算球的位置，其中a=9.8ms-2且无阻力。平面是被动和固定的，因此球只是停止。
球和表面的表面响应可以从零到一的范围进行修改。对于两个物体，最大值为1.0时，碰撞将是弹性的，
没有能量耗散——球将在整个动画中反腐地保持弹跳。我们现在通过给球的初始速度提供X分量来创建弹射运动。

*   将Bounciness级别设置为球体的0.5和固定平面的1.0.

*   添加第二个平面并使用键R-Y-90旋转。将物理模块设置为刚体和被动，就像第一个平面一样。另外，选中动画框。

*   设置第一帧和第十帧的X位置的关键帧。由于我们现在设置的刚体动力学，飞机将给球提供X速度。

*   将固定地平面的摩擦力值设置为0.0.

从头开始再次按下播放按钮现在将显示球呈现射弹运动。

我们将使用网格螺钉工具和多个球体来完成刚体动力学示例。

*   Add——>Mesh——>Circle以创建刚体管的起点。

*   使用TAB键进入网格编辑模式并添加旋转矢量，如图8-18所示。

*   激活GUI左侧的螺钉工具，并将步数设置为48.

*   将创建一个开瓶器管。再次按TAB键进入对象模式并选择物理模块选项卡。将刚体对象设置为被动，使用网格碰撞形状而不是默认的凸形外壳。这将允许球在管内滚动

*   相对于平面网格物体移动螺旋形螺钉，使得下部开口将允许球滚出到平面上。

*   将球体对象的碰撞形状更改为球体并将其放置在螺旋形管的顶部。

*   使用SHIFT-D复制球体并将新对象放在平面上（图8-19）。
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-18.png?raw=true)

    图8-18的初始设置。网格编辑窗口中的此示例显示圆和旋转矢量的设置。螺钉工具将用于围绕斜面旋转顶点以形成螺旋形管形状。这个形状在物理模块中将具有网格碰撞形状，使得另一个球体可以在管内滚动。
    
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/8-19.png?raw=true)    

    图8-19.
