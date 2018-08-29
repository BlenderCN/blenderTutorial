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
    
    图8-2。
