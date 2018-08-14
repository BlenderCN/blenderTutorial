# bgl和blf模块

bgl模块是Blender在3D视窗和Blender游戏引擎中常用的OpenGL函数的包装器。
OpenGL(开放图形库)是一个开源的低级API，用于无数的3D应用程序，以利用硬件加速计算。

对于熟悉OpenGL的读者来说，bgl文档似乎很熟悉。bgl模块本身旨在模仿OpenGL2.1的调用结构和逐帧渲染风格。

在阅读bgl文档时，我们注意到许多高级概念，如缓冲操作，面部剔除和光栅化。幸运的是，对于Blender Python程序员来说，
3D视窗已经在管理这些操作。我们更关注用额外信息标记3D视窗以帮助用户理解他的模型。本章主要关注使用bgl绘图。

blf模块是一小组用于显示文本和绘图字体的函数。它与bgl密切相关，在没有它的例子中很少提及。
Blender Python开发人员通常将bgl和blf模块组合在一起制作测量工具，用bgl绘制线条并用blf显示它们的测量值。
我们在本章就是这么做的。

请注意，这些模块通常在bge(Blender Game Engine)模块的示例中看到。我们不会在Blender游戏引擎中工作，
因此这些脚本将无法运行，并且尝试导入bge将失败。我们将绘图限制为3D视窗。

另请注意，bgl模块设置为在Blender 2.80+中替换或重新构建。本章可能是本文发布后第一次更新。

## 瞬间绘图

bgl和blf模块的教学方式与其他Blender Python模块不同。当通过这些模块中的任何一个在3D视窗上绘制线条或字符时，
它仅对单个帧可见。因此，我们无法像在其他模块中那样在交互式控制台中进行实验。
我们在交互式控制台中执行的功能可以无误地执行，但我们将无法在3D视窗中查看结果。

为了有效地使用bgl和blf模块，我们必须在处理函数中使用它们，该函数设置为在每次帧更改时更新。
因此，我们从使用非OpenGL概念的处理程序示例开始。

## 处理程序概述

本节提供了使用bpy.app.handlers的处理程序示例。这不是我们在处理bgl和blf时最终会使用的子模块，
但它对于在Blender中学习处理程序是有益的。

### 时钟示例

处理程序是设置为每次事件发生时运行的函数。为了实例化一个处理程序，我们声明一个函数，
然后将它添加到Blender中可能的处理程序列表之一。在清单6-1中，我们创建了一个用当前时间修改文本网格文本的函数。
然后我们将函数添加到bpy.app.handlers。scene_update_pre表示我们希望它在3D视窗更新和显示之前运行。

结果似乎是3D视窗中的时钟。实际上，它是一个每秒更新许多次的文本网格。此示例不安全或不完整，
但只要我们将对象保留在场景中命名为MyTextObj，我们就可以在后台运行时钟添加和编辑其他对象。结果见图6-1

清单6-1。Blender时钟处理程序示例

    import bpy
    import datetime
    
    # Clear the scene
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create an object for our clock
    bpy.ops.object.text_add(location=(0,0,0))
    bpy.context.object.name = 'MyTextObj'
    
    # Create a handler function
    def tell_time(dummy):
        current_time = datetime.datetime.now().strftime('%H:%M:%S.%f')[:-3]
        bpy.data.objects['MyTextObj'].data.body = current_time
        
    # Add to the list of handler functions "scene_update_pre"
    bpy.app.handlers.scene_update_pre.append(tell_time)

图6-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/6-1.png?raw=true)    
