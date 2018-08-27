# 网格，模型和纹理

## 3.1 3D网格对象的结构

Blender中的3D对象通常称为网格。每个网格基本上由顶点，线和面组成。
顶点是奇点构造——它没有与之关联的体积——并且由全局3D视图端口中的X-Y-Z坐标定义。
这些坐标可以转换为另一个坐标系或参考系。顶点可以通过线连接，顶点和直线的闭环可以形成一个称为面的实心平面的边界。

网格具有几何属性，包括对象可以围绕其旋转和/或旋转的原点。可以在网格编辑模式下编辑各个顶点，
线和面，可通过模式选择下拉菜单或TAB键访问（图2-5）。每个网格都有一个通过这些顶点，线和面的选择模式。

### 示例：建立一个简单模型

可以通过屏幕顶部菜单中的Add——>Mesh创建基础网格。基本网格如图3-1所示。这些可以变形，改变和扩展，以适应所需的科学可视化。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-1.png?raw=true)

    图3-1。Blender网格示例，构成科学可视化中数据容器和网格构造的基础。这些基本形状——立方体，圆锥体，UV球体，环面，圆柱体和球体——可以在Blender 3D视口中进行操作
    
*   单击Add——>Mesh——>Cube创建新的立方体网格。或者，使用键盘快捷键SHIFT-A。

*   使用R键在垂直于视线的平面中旋转立方体。

*   使用S键缩放立方体。

*   使用G键在垂直于视线的平面中平移立方体。

*   GUI右侧的变换工具栏允许精确定位网格对象。

我们可以通过多种方式进一步操纵网格的各个元素。这可以帮助用户精确定位可视化的场景元素和数据对象

*   在3D视图端口的底部，单击下拉菜单并选择编辑模式。或者，按键盘上的TAB键  

*   网格元素的面，线和顶点将以橙色高亮显示。顶点，线或面选择的模式如图3-2所示。可以使用辅助鼠标按钮（通常是鼠标右键）选择网格元素。按住SHIFT键可以选择多个元素。

*   可以通过按键盘上的B键（用于框选择），然后在所选点上单击并拖动一个框来选择元素组。

*   通过CTRL左键单击需要放置新顶点的位置添加由线连接的顶点。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-2.png?raw=true)

    图3-2。选择Blender Mesh Edit模式，并显示顶点，线和面模式按钮。这允许用户操纵网格对象的各个部分。
    
## 3.2 2D材料和纹理

纹理可以在各种场景中应用。2D纹理可用于为面部提供更逼真的表面外观，应用贴图数据以及更改数据的可见性和颜色。
凹凸贴图也可以应用于网格以模拟3D表面。这对于通过较低的多边形来提高渲染时间的速度具有重要的应用。
2D材料和纹理可以应用于UV平面中的各种投影中的单个面或整个网格对象。地球的正投影如图3-3和3-4所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-3.png?raw=true)

    图3-3。此视图显示用于将地球的2D地图投影到UV球体上的设置。
    
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-4.png?raw=true)   

    图3-4。使用来自于http://earthobservatory.nasa.gov 的图像投射地球投影。这里以最终的复合材料呈现几个层，包括地球的日夜侧图和大气层。

### 示例：向球体添加纹理

我们可以使用以下过程将地图投影到球体上。这需要在球体上标记将要分离对象的接缝。然后可以将网格的投影与图像或地图匹配。

*   单击Add——>Mesh——>UV Sphere创建UV球体网格。或者，可以使用键盘快捷键SHIFT-A。   

*   增加球体的多边形面数。

*   按TAB键选择网格编辑模式。

*   右键单击沿着从球体的北极到南极的子午线的顶点。

*   按键盘上的CTRL-E并选择Mark Seam。接缝将标记为红色，并显示3D对象将被分离的位置（图3.5）。

*   选择UV编辑菜单并打开新图像文件（ALT-O）。

*   选择所有顶点（A键），按U键进行UV Mapping并选择Sphere Projection（图3-6）。

*   通过选择“视口着色”下拉菜单下的“纹理模式”可以看到结果。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-5.png?raw=true)

    图3-5。在网格编辑模式（TAB键）中，可以使用CTRL-E添加接缝。在这个特殊例子中，我们沿着子午线标记接缝。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-6.png?raw=true)

    图3-6。通过正确标记子午线上的接缝，现在可以使用UV Mapping应用地图投影。
    
## 3.3 3D材料和贴图  

对于3D材质和纹理，Blender可以将晕圈应用于数据点或将数据立方体渲染为透明卷。光晕纹理可应用于顶点，可用于创建3D散点图。

### 3.3.1 示例：为3D散点图创建点

我们将使用3D网格的顶点作为样本X，Y，Z数据，并使用光晕对这些点进行纹理处理。

*   单击Add——>Mesh——>Icosphere创建UV-icosphere网格。或者，可以使用键盘快捷键SHIFT-A。

*   转到属性面板上的材料选项卡，然后单击New以添加新材料。

*   选择Halo选项（图3-7）

*   更改光晕参数：尺寸为0.050,硬度为20。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-7.png?raw=true)

    图3-7。可以应用光晕材料来照射散点图或目录中的各个顶点。这些小高斯球体具有可调节的尺寸，照明和颜色特性。

### 示例：创建线框网格

使用相同的icosphere网格对象（或任何网格），可以创建3D线框对象。这些对于在3D散点图中创建背景网格非常有用。只需将材料更改为Wire，将阴影发射更改为1.0（图3-8）

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/3-8.png?raw=true)

    图3-8。线材可以应用于网格。这在创建可视化的网格和边界框时很有用。
    
*   单击Add——Mesh——Plane创建平面网格。或者，可以使用键盘快捷键SHIFT-A。

*   按键盘上的TAB键进入网格编辑模式。

*   在网格工具面板（GUI的左侧）上，单击细分五次以增加平面中的网格点数。

*   再次按TAB键重新进入对象模式。使用S键将对象缩放到更大的大小。

*   转到属性面板上的材料选项卡，然后单击New以添加新材料。

*   选择Wire选项（图3-8）。如果需要，可以更改颜色。

*   更改线材参数：漫反射强度：1.0,镜面强度：0.0和着色发射：1.0。
