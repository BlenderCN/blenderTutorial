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

