# 视点：相机控制

我们现在将讨论如何通过摄像机的移动和跟踪来记录我们的可视化。在Blender中，当前选定的相机对象呈现在最终渲染中的视野。
摄像机对象有许多关键参数：摄像机的X，Y，Z位置和旋转，垂直于摄像机胶片平面的矢量，视野，观察投影和纵横比。例如，
在图6-1中，摄像机位于（1,2,3）位置，沿X轴旋转45°，透视投影中的视野角为35°纵横比为16：9（1080p高清画质）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/6-1.png?raw=true)

    图6-1。3D视图端口显示指向球形网格的相机，变换工具栏详细说明了相机的精确位置和旋转配置。

## 6.1 投影差异：视角问题

考虑图6-2中投影的差异。透视投影显示沿轴线的所有平行线会聚到水平线上的单个点。这就是人眼会看到的。
正交投影显示沿轴的所有线是平行的——通常在科学图中使用的线。两者的使用取决于用户希望如何在可视化中呈现他们的数据。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/6-2.png?raw=true)

    图6-2。两个例子显示了（a）立方体网格的透视图和（b）具有正投影的相同网格。
    
## 6.2 相机关键帧    

相机对象的位置和旋转可以像任何Blender对象一样设置关键帧。此外，无论摄像机的位置如何，
摄像机都可以连接到移动轨道并被约束为指向特定物体。这些属性对于跟踪可以为非常简单的可视化添加非常动态视图的镜头非常有用。

### 6.2.1 示例：跟踪对象

在这一点上，我们介绍了约束的概念。可以约束相机对象以跟踪另一个对象——相机的胶片平面的法线将直接指向另一各队相当定义中心。
对于许多场景，向可视化场景添加空对象很有用。空对象不会在可视化中呈现，而是由GUI中的一组正交轴表示（图6-3）。
实质上，相机对象将指向空对象所在的任何位置。可以将空对象附加到另一个对象以帮助在可视化期间跟踪移动对象。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/6-3.png?raw=true)

    图6-3。此视图显示空轴对象的位置。此类对象不会渲染，可以附加到其他网格，以便摄像机可以在可视化动画期间跟随和追踪它。
    
*   在启动Blender时，从默认场景开始——一个立方体，相机和灯。

*   使用Add——>Empty——>Plain Axes添加空对象。

*   右键单击以首先选择立方体对象，然后再次SHIFT——right——click以选择空对象。

*   按CTRL-P将立方体设置为父空对象。

*   右键单击并选择相机对象。

*   单击右侧属性面板上的约束选项卡。

*   选择追踪到并选择目标为空

*   选择To作为-Z，Up作为Y。这将在在查看摄像机视野视野时正确定向向上和法线方向。

*   现在，蓝色虚线将直接从相机对象指向空对象，表明无论相机 移动到何处，在可视化过程中两个对象之间的跟踪始终保持（图6-4）。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/6-4.png?raw=true)

    图6-4。应用了跟踪到约束摄像机对象，使得摄像机始终将其视野置于作为渲染立方体的父对象的空对象上。垂直于相机平面的蓝色虚线指向从相机对象跟踪到其跟随的对象。
 
### 示例：跟随路径的对象

相机对象可以设置关键帧以在两点之间移动（如6.2节所述）。但是，它可以设置为遵循连续平滑的预定路径。Bézier曲线对象（图6-5）可用于构造此路径。
然后可以锁定摄像机对象以跟随路径。当希望在同时跟踪的同时将相机移动通过场景时，这非常有用。以下场景从上一个摄像机跟踪示例继续。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/6-5.png?raw=true)    

    图6-5。此面板显示附有摄像头的Bézier曲线。摄像机锁定在曲线上，并在动画中跟踪它，同时指向立方体网格物体。
    
*   右键单击以选择场景摄像机对象——它将突出显示橙色。

*   在变换面板中，将位置和旋转的任何偏移归零，分别为X，Y，Z的0.0和0.0度。

*   单击Add——>Curve——>Circle。

*   在属性面板中选择对象数据选项卡（图6-6）。单击路径动画复选框，将帧更改为300.

*   在动画工具栏上，将帧设置为零，然后在路径动画下，右键单击Evaluation Time和Insert Keyframe。

*   在动画工具栏上，将帧设置为另，然后在路径动画下，将评估时间设置为300.0，右键单击Evaluation和Insert Keyframe。在该示例中，评估时间段参数化单位是帧编号。

*   右键单击以再次选择相机对象。单击Constraints图标并添加名为Follow Path的对象约束。将前向轴更改为-Z，将Up向量更改为Y。

*   播放动画并观察摄像机对象是否遵循路径，摄像机法线向量与圆相切。

*   为摄像机添加另一个对象约束——这次是Track To约束。将To向量更改为-Z，Up向量更改为Y。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/6-6.png?raw=true)

    图6-6。此窗口位于属性面板，用于Bézier曲线的路径动画设定关键帧。一旦路径为关键帧，可以将诸如相机的对象锁定到路径上。
    
现在，相机将在动画过程中跟随Bézier圆形路径，同时指向立方体。如果移动或缩放路径，摄像机将跟随。    
