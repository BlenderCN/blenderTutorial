# bpy模块

本章介绍和详细介绍了bpy模块的主要组件。在这样做时，我们解释了Blender的许多重要行为。我们涵盖选择和激活，创建和删除，场景管理和代码抽象。

可以通过在http://www.blender.org/api/上选择Blender版本来找到Blender Python API的官方文档。我们在本文中使用了Blender2.78c，
因此我们的文档可以在http://www.blender.org/api/blender_python_api_2_78c_release/找到。

## 模块概述

我们首先给出bpy每个子模块的一些背景知识。

### bpy.ops

如上所述，该子模块包含运算符。这些主要是用于操作对象的函数，类似于Blender艺术家在默认界面中操纵对象的方式。
子模块还可以操纵3D视窗，渲染，文本等等。

对于操作3D对象，两个最重要的类是bpy.ops.object和bpy.ops.mesh。对象类包含用于同时操作多个所选对象的函数以及许多常规实用程序。
网格类包含用于一次一个地操纵对象的顶点，边和面的函数，通常在编辑模式下。

bpy.ops子模块中目前有71个类，所有类都名称相当且组织良好

### bpy.context

bpy.context子模块用于通过各种状态标准访问Blender的对象和区域。
该子模块的主要功能是为Python开发人员提供一种访问用户正在使用的当前数据的方法。如果我们创建一个按钮来置换所有选定的对象，
我们可以允许用户选择他选择的对象，然后在bpy.context.select_objects中置换所有对象。

我们在构建插件时经常使用bpy.context.scene,因为它是某些Blender对象的必需输入。我们还可以使用bpy.context访问活动对象，
在对象模式和编辑模式之间切换，并从油脂笔接受数据。

### bpy.data

该子模块用于访问Blender的内部数据。解释这个特定模块(*/bpy.data.html页面直接指向一个单独的类)的文档可能很困难，
但在本文中我们将非常依赖它。bpy.data.objects类包含确定对象形状和位置的所有数据。
当我们说前面的子模块bpy.context非常适合将我们指向对象组时，我们的意思是bpy.context类将生成对bpy.data类的数据块的引用。

### bpy.app

这个子模块并没有完整记录，但是我们对目前所信的信息可以在脚本和插件开发中发挥很大的作用。
子模块bpy.app.handlers是我们在本文中唯一关注的问题。处理程序子模块包含用于触发自定义函数以响应Blender中的事件的特殊函数。
最常用的是帧更改句柄，每次更新3D视窗时(即，在帧更改后)执行某些函数。

### bpy.types,bpy.utils和bpy.props

这些模块将在后面的插件开发章节中详细讨论。读者目前可能会发现*/bpy.types.html中的文档对于描述我们在其他地方使用的对象类非常有用。

### bpy.path

此子模块与本机随Python一起提供的os.path子模块基本相同。它对于核心开发团队之外的Blender Python开发人员来说很少有用。

## 选择，激活和规范

Blender界面设计直观，同时还提供复杂功能。某些操作在逻辑上适用于单个对象，而其他操作可以在逻辑上同时用于一个或多个对象。
为了处理这些场景，Blender开发人员创建了三种访问对象及其数据的方法

1.选择：可以一次选择一个，多个或零个对象。使用所选对象的操作可以在单个对象或多个对象上同行执行该操作。

2.激活：在任何给定时间只能激活一个对象。在活动对象上工作的操作通常更具体和更激烈，因此不能一次性地在许多事物上直观地执行。

3.规范：(仅限Python)Python脚本可以通过名称访问对象并直接写入其数据块。虽然操作选定对象的操作通常是平移，旋转或缩放等差异操作，
但将数据写入特定对象通常是声明性操作，如位置，方向或大小。

### 选择对象

在继续之前，建议读者在3D视窗中创建一些不同的对象作为示例。转到3D Viewport>Add 以查看对象的创建菜单。

当我们右键单击3D视窗中的对象时，对象会高亮显示并取消高亮显示。当我们按住Shift键并单击时，我们可以一次突出显示多个对象。
3D视窗中的这些高亮显示所选对象。要列出所选对象，请在清单2-1中键入交互控制台中的代码。

清单2-1。获取所选对象列表

    # Outputs bpy.data.objects datablocks
    bpy.context.selected_objects

正如我们我们前面提到的，bpy.context子模块非常适合根据Blender中的状态获取对象列表。在这种情况下，我们获取了所有选定的对象。

    # Example output of Listing 2-1,list of bpy.data.objects datablocks
    [bpy.data.objects['Sphere'],bpy.data.objects['Circle'],bpy.data.objects['Cube']]

在这种情况下，在3D视窗中选择了一个名为Sphere的球体，一个名为Circle的圆圈和一个名为Cube的立方体。
我们返回了一个bpy.data.objects数据块的Python列表。鉴于所有这种类型的数据块都具有名称值的知识，
我们可以遍历清单2-1的结果来访问所选对象的名称。请参阅清单2-2,我们在其中获取所选对象的名称和位置。

清单2-2。获取所选对象列表

    # Return the names of selected objects
    [k.name for k in bpy.context.selected_objects]
    
    # Return the locations of selected objects
    # (location of origin assuming no pending transformations)
    [k.location for k in bpy.context.selected_objects]
   
现在我们知道如何手动选择对象 ，我们需要根据某些条件自动选择对象。必需的函数在bpy.ops中。清单2-3创建了一个函数，
该函数将对象名称作为参数并选择它，默认情况下清除所有其他选择。如果用户指定additive=True，则该函数不会事先清除其他选择。

清单2-3。以编程方式选择对象
    
    import bpy
    
    def mySelector(objName,additive=False):
        
        # By default,clear other selections
        if not additive:
            bpy.ops.object.select_all(action='DESELECT')
            
        # set the 'select' property of the datablock ot True
        bpy.data.objects[objName].select = True
    
    # select only 'Cube'
    mySelector('Cube')
    
    # Select 'Sphere',keeping other selections
    mySelector('Sphere,additive=True)
    
    # Translate selected objects 1 unit along the x-axis
    bpy.ops.transform.translate(value=(1,0,0))

_____
Note: 要在没有Python脚本的情况下轻松查看对象的名称，请导航到“属性"窗口并选择橙色立方体图标。现在，
活动对象将在此子窗口顶部附近显示其名称，如图2-1中的情况。此外3D视窗的左下角将显示活动对象的名称。
我们将在本章的下一小节中讨论激活。
_____

图2-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/2-1.png?raw=true)

### 激活对象

激活，如选择是Blender中的对象状态。与选择不同，在任何给定时间只能有一个对象处于活动状态。此状态通常用于单个对象的顶点，边和面操作。
此状态还与编辑模式有密切关系，我们将在本章节后面详细讨论。

当我们在3D视窗周围单击鼠标左键时，我们单击的任何对象都将高亮显示。当我们以这种方式高亮显示单个对象时，
Blender会选择并激活该对象。如果我们按住Shift并在3D视窗周围单击鼠标左键，则只有我们单击的第一个对象才会处于活动状态。

请注意图2-1中所示的属性窗口区域，其中显示活动对象的名称。也可以通过图2-1底部的菜单激活对象。

要在Python中访问活动对象，请在交互式控制台中键入清单2-4.请注意，有两个等效的bpy.context类用于访问活动对象。
就像选择的对象一样，我们返回一个bpy.data.objects数据块，我们可以直接操作。

清单2-4。访问活动对象

    # Returns bpy.data.objects datablock
    bpy.context.object
    
    # Longers synonym for above line
    bpy.context.active_objects
    
    # Accessing the 'name' and 'loation' values of the datablock
    bpy.context.object.name
    bpy.context.object.location
    
    
