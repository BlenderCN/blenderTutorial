# 纹理和渲染

到目前为止，我们已经将代码示例限制为在Blender中创建网格和插件。对于3D艺术家和动画师来说，
3D建模的目标是通过渲染的图像和视频使场景变得生动。在Blender Python中渲染实际上非常简单，
通常只需要一个函数调用。为了让我们达到想要渲染场景的程度，我们将讨论纹理，灯光和相机布局。

到本章结束时，用户将能够创建用于纹理，灯光，相机放置和静态渲染的自动化管道。
虽然可以使用Blender Python渲染动画视频，但我们将在此讨论限制为渲染静止图像。

## 纹理词汇

一般来说有很多类型的纹理，Blender中有许多额外的参数化类型。
我们的第一个示例使用漫反射贴图和法线贴图来说明材质在Blender中的功能。
在我们继续之前，我们将建立一些关于纹理的新词汇。

### Blender中的影响类型

虽然这些效果在Blender中被归类为影响，但它们传统上指的是3D建模的广泛领域中的纹理类型。Blender有自己的纹理类型，
每种纹理都可以采用任何这些影响。有关这些影响在Blender GUI中的位置，请参见图8-1.
它们可以在 Properties>Materials>Influence中找到。

*  漫反射纹理用于着色对象。漫反射纹理可以描述Blender中对象的颜色，强度，alpha级别和半透明度。为了将图像叠加在对象的面上，我们使用漫反射颜色纹理。
  
*  着色纹理描述对象如何与场景中的其他交互。如果我们希望对象镜像另一个，将颜色发射到另一个，或将环境光溢出到场景中，我们在Blender中指定必需的着色属性。
  
*  镜面纹理描述了物体对光的反应。例如，如果我们提供静态模糊图像(如在旧电视屏幕上可能看到的那样)作为镜面纹理，则光会像闪亮的沙粒一样从物体上反射出来。我们可以通过指定颜色对光的反应强度和方向来微调镜面反射贴图。
  
*  几何纹理允许对象影响对象的几何外观。例如，如果我们为几何图提供黑白条纹并指定法线贴图，我们会在模型中看到3D脊。重要的是要注意，这些效果仅在渲染中实现，而不是在网格数据本身中实现。
  
图8-1
  
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-1.png?raw=true)

### Blender中的纹理类型

虽然我们主要使用图像纹理，但Blender有许多我们可以选择的可自定义纹理。这些是从图8-2所示的Properties>Materials>Type菜单中选择的。

图8-2

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-2.png?raw=true)

图像和视频和环境贴图选项可以导入图像和视频文件。剩余的纹理可以在Blender中进行参数化，以获得所需的结果。
我们没有详细说明如何使用这些参数化纹理中的任何一个，就像要讨论的许多参数一样。
清单8-1解释了如何使用Image和Video类型来给对象纹理。从这里开始，
读者应该能够使用Blender的Python工具提示为任何剩余类型复制此过程。

## 添加和配置纹理

在讨论文件交换格式时，我们在第4章中讨论了纹理的定义。纹理通过uv坐标映射到3D空间中的面。
要将方形图像作为纹理映射到网格的正方形面，
我们将uv坐标[(0,0),(1,0),(0,1),(1,1)]分别指定到网格的左下角，，右下角，左上角和右上角。
随着面的形状变得更加复杂，实现期望的纹理所需的过程也变得更加复杂。我们接下来讨论将uv坐标映射到常见形状的方法。

### 加载纹理和生成UV映射

由于Blender处理纹理导入和材质的方式，uv映射不是一个完全简单的任务。我们必须克服一些程序障碍，
以便在我们的脚本中达到我们可以在我们的对象上明确定义uv坐标的点。一旦我们达到这一点，
uv坐标的精确规范就相当简单。我们通过清单8-1中的示例进行解释。

我们在示例中使用了数字1和数字2的示例图像，
可以从http://blender.chrisconlan.com/number_1.png 和 http://blender.chrisconlan.com/number_2.png 下载。
读者可以使用这些图像或清单8-1中的任何其他所需图像。结果见图8-4.我们将在以下部分讨论清单8-1中使用的函数。

图8-3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-3.png?raw=true)

清单8-3。加载纹理和生成uv映射。

    import bpy
    import bmesh
    from mathutils import Color
   
    # Clear scene
    bpy.ops.object.mode_set(mode = 'OBJECT')
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create cube
    bpy.ops.mesh.primitive_cube_add(radius = 1,location = (0,0,0))
    
    bpy.ops.object.mode_set(mode = 'EDIT')
    
    # Create material to hold textures
    material_obj = bpy.data.materials.new('number_q_material')
   
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-4.png?raw=true)   

### Blender中的纹理与材质

纹理是3D建模中的一个广义术语。它可以指漫反射纹理，颜色纹理，渐变纹理，凹凸贴图等。重要的是要注意，
我们可以同时将所有这些纹理形式映射到对象。例如，房屋屋顶上的一组瓦可能需要图像纹理，漫反射贴图和凹凸贴图，
以便在渲染时显得逼真。

另外，通常将真实世界材料的图像，漫反射贴图和凹凸贴图彼此专门构建。在我们的瓦示例中，
凹凸贴图将定义物理挖在图像纹理中出现的脊。漫反射贴图将进一步定义我们通常在屋顶瓦片看到的闪亮粒子。
按照设计，表示图像和地图文件不一定适用于集合外部的其他文件。这是Blender材料的动力。

Blender中的材质是纹理相关数据的集合。它可能包括前面提到的任何图像和贴图，它可能包括其他法线和alpha贴图。
因此，我们必须首先从其构成纹理构建材质，然后将材质指定给对象。无论我们是否有一个或多个包含材质的纹理，
都必须将纹理数据分配给材质。然后，必须将材质指定给对象。

这个讨论揭示了清单8-1中材料管理背后的动机。我们首先声明并操作所有必需的纹理，
然后通过bpy.context.object.data.materials.append()将整个材质添加到对象。从这里，我们可以操纵整个材料的uv坐标。

### UV坐标和循环

清单8-1的后半部分访问了我们以前没有使用过的数据端点。我们要访问的ub坐标数据层包含在循环对象中。
循环可以被认为是跟踪3D对象的一组顶点的3D多边形。循环可以跨越多个面，但必须在同一点开始和结束。
当循环跨越多个面时，它们旨在捕获一组局部的相邻面。

3D艺术家可以使用高级工具来帮助他们创建循环。这些循环然后帮助他们手动分配uv坐标。
虽然我们不会在Blender Python中操作这些循环，但了解它们是如何工作的很重要，因为循环数据对象位于网格本身和uv层之间。

幸运的是，Blender中的循环数据对象与bmesh.faces[].verts[]对象具有1对1的对应关系，我们习惯使用它们。
换句话说，任意两个整数，f和v，bm.faces[f].loops[v][uv_layer].uv访问的(u,v)坐标对应于bm.faces[f].verts[v].co访问的(x,y,z)坐标。

重要的是要注意两个整数f和v可能不指定3D空间中的唯一点。在默认的Blender2.78c立方体中，
因为它出现在启动文件中，f:v对0:2,3:3和4:0都对应于3D空间中的点(-1.0,-1.0,-1.0)。
当立方体被纹理化时，这些uv坐标通常是唯一的，因为它们都将对应于纹理贴图的不同部分。

### 关于索引和交叉兼容性的另一个注释

当动态地处理对象时，我们遇到类似于第3章”关于索引和交叉兼容性的注释“中提到的问题。在该部分中，
我们注意到顶点索引的行为是可复制但不可篡改的，因此可以通过特性作为变通方法(在清单3-13中实现)进行选择。
这里适用相同的概念，除了我们必须使用bm.faces[f].verts[v].co而不是bm.verts[v].co。

例如，假设我们想要在立方体顶部的y轴上竖立纹理。
一种可能的解决方案是使用ut.py工具包中的ut.act.select_by_loc()根据其位置选择立方体的顶面。
从这里开始，我们可以使用f_ind = [f.index for fm in bm.faces if f.select][0]来返回所选的面索引。使用面索引，
我们可以将面部的顶点存储为vert_vectors = [vm for b in bm.faces[f_ind.verts]]并使用此信息沿立方体来定向我们的纹理

我们的另一个选择是通过假设我们在对纹理进行纹理化之前知道对象的面顶点的位置和方向来违背第3章”关于索引和交叉兼容性的注释“的建议。
我们通常可以提前确定这些信息并将其硬编码到我们的纹理脚本中，如清单8-1所示。这是受控和内部使用的可行选项，
但建议不要使用我们将与社区共享的代码以及针对跨版本兼容性进行测试的代码。

根据我们到目前为止的讨论，读者应该拥有可用的工具和知识来实现他们所需的动态(或非动态)纹理脚本。
第3章的参考部分及其以下部分与读者可能承担的任何动态纹理化任务相似。

我们现在继续讨论Blender中的渲染及其一些用途。

## 删除未使用的纹理和材质

我们已经讨论了许多用于在Blender中删除网格和对象的有用函数。随着我们不断测试脚本，
我们的材料和纹理数据很快就会变得混乱而我们没有意识到。当我们忽略删除它们时，
Blender会将纹理重命名为my_textrue.001,my_texture.002等。

纹理和材料必须没有用户才有资格删除。在这种情况下，用户引用当前分配的对象数。要删除纹理和材质，
我们循环遍历bpy.data.materials和bpy.data.textures数据块，并在未使用的那些上调用.remove()。有关此实现，请参见清单8-2。

清单8-2。加载纹理和生成UV映射。

    import bpy
   
    mats = bpy.data.materials
    for dblock in mats:
        if not dblock.users:
            mats.remove(dblock)
            
    texs = bpy.data.textures
    for dblock in mats:
        if not dblock.users:
            texs.remove(dblock)
            
## 使用Blender渲染渲染

使用Blender的内置渲染功能非常简单。我们介绍并解释如何在场景中定位灯光和相机，然后调用渲染功能来创建图像。
我们的大部分讨论都集中在相机和灯光的语义和辅助功能上。

### 添加灯光

在清单8-1中，我们在多维数据集周围添加六个灯，以便在Blender的3D视窗中的渲染视图中查看它。正确使用此视图和渲染通常需要灯光。
照明在3D建模本身就是一个重要的，大的领域。在本节中，我们将重点放在与照明相关的Blender Python功能上，而不是美学上令人愉悦的照明的一般实践。

在3D视窗标题中，我们可以导航到Add>Lamp以选择任何Blender的内置灯。使用Python工具提示，
我们可以看到它们都依赖于函数bpy.ops.object.lamp_add(),type=parameter确定光的类型。我们有SUN，POINT，SPOT，HEMI和AREA选项。
每种类型都有自己的参数集来配置。

在程序生成的照明方面，我们首要关注的是放置和方向。我们将介绍一些用于管理布局和方向的实用程序。
例如，为了懒洋洋地照亮整个场景，我们可能想要在场景的聚合边界框周围创建点光源。此外，
我们可能希望将聚光灯指向另一个任意放置的对象。有关可能有助于程序添加灯光的实用程序列表，请参见清单8-3。
我们在清单8-3中声明的所有函数都已添加到我们的工具包ut.py中，可以从http://blender.chrisconlan.com/ut.py 下载。

有关每种类型灯的基本说明，请参阅表8-1。

    Table 8-1。灯光类型
    ——————————————————————————————————————————————————————————————————————————————————————
    Type        Description
    ——————————————————————————————————————————————————————————————————————————————————————
    Point       Emits lights equally in all directions;rotation has no effect
    Spot        Emits a cone of light in a particular direction
    Area        Emits light from a rectangular area;follows a Lambert distribution
    Hemisperic  Similar to area,but has spherical curvature
    Sun         Emits orthogonal light in a particular direction;position has no effect
    ——————————————————————————————————————————————————————————————————————————————————————
    
### 添加摄像机

渲染场景需要相机。要在程序上添加相机，我们必须定位它，调整其方向，并修改其参数。
我们将使用清单8-3中的函数来定位和定向摄像机以及灯光。

在程序生成相机时我们必须解决的最大问题是确定距离和视野，以便捕获整个场景而不会在渲染中显得太小。
我们将使用一些基本的三角学来解决这些问题。

视野(FoV)是从相机向外投射的一对两个角度(θx , θy ),其无限延伸的四棱锥。如果前面没有任何东西，
相机可以看到位于这个四棱锥内的所有东西。为了给出一些观点，在横向模式下，iPhone6相机的FoV大约为(63°,47°)度。
请注意，当摄影师通俗地提及FoV时，它们通常仅指两个角中较大的一个。

我们必须了解FoV,以便我们可以确保摄像机的放置和校准捕获我们想要渲染的场景。

给定具有FoV (θx,θy)的相机并且面向具有高度h和宽度w的边界框的场景，与捕获场景所需的场景d的距离是max(dx , dy )。
对于次讨论，dx和dy分别表示沿水平和垂直维度捕获场景的必要距离。有关可视化表示，请参见图8-5。使用基本的三角学，我们有

                                                     w     θx
                                                dx = — cot(——)
                                                     2      2
                                                     
                                                     h     θy    
                                                dy = — cot(——)
                                                     2      2
                                                     
 图8-5
 
 ![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-5.png?raw=true)                                              

这仅仅说明了相机沿x轴或y轴指向的简单情况，但它足以满足我们的目的。在清单8-4中，
我们使用先前建立的实用程序函数来引导相机，使其可以渲染整个场景。

清单8-3。灯光和相机的实用程序。
   
    # Point a light or camera at a location specified by "target"
    daf point_at(ob,target):
        ob_loc = ob.location
        dir_vec = target - ob.location
        ob.rotation_euler = dir_vec.to_track_quat('-Z','Y').to_duler()
        
    
### 渲染图像

渲染是计算给定3D数据的高分辨率图像和视频的过程。渲染不是即时的。虽然Blender中的3D视窗在我们平移和旋转相机时似乎流畅地移动，
但渲染可能需要相当长的时间。3D视窗是3D数据的即时渲染，但它不代表与传统渲染相同的质量或定义级别。

在代码清单8-4中，我们使用Blender Render和OpenGL渲染渲染清单8-1的输出。该示例假设相机从场景的中间位置沿着x轴指向上方，
从场景的yz中位数开始，使得它将捕获整个场景。我们使用前面讨论的方程来完成此任务。回想一下，这些方程假设我们沿着轴指向相机的简单情况。

结果以帧直接渲染捕获对象。有关在清单8-1中创建的立方体的Blender Render，请参见图8-6/对于Blender Render，
场景的相机用作渲染相机。这就是了解如何在程序上设置摄像机位置的重要原因。如果我们想循环并渲染许多场景，我们需要确信场景将在帧内捕获。

图8-6

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-6.png?raw=true)

我们还可以使用OpenGL渲染渲染3D视窗的快照。这将捕获场景的基本特征，类似于我们在具有实体视图的对象模式下看到3D视窗的方式。
结果见图8-7。请注意，在此视图中，我们可以看到灯光和相机，但不能看到材质。当我们调用bpy.ops.render.opengl()时，
设置view_context=True将导致Blender使用3D视窗相机(用户的视图)而不是场景相机。

清单8-4。使用Blender渲染和OpenGL渲染渲染

    ### Aussumes output of Listing8-1 is in scene at runtime ###
    
    import bpy
    import bmesh
    import ut
    
    from math import pi,tan
    from mathutils import Vector

图8-7

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/8-7.png?raw=true)    

## 结论

本章总结了我们对Blender Python API的讨论。即使有很多例子，本文也不是一本全面的指南。
这证明了Blender的复杂性和模块性比什么都重要。可以使用Python API编辑，调整，自定义和扩展Blender。本书的作者和协助其发展的专业人员希望这些知识有助于鼓励Blender社区的研究和开发。
