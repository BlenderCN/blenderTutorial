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

这个子模块

