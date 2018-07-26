# Blender接口

本章讨论并定义了Blender接口的组件。它作为我们用来讨论整个文本接口的词汇参考。我们将专注于Python开发中最常用的接口组件，
以及高效的Python脚本设置自定义接口。

为了避免在整本书中放置大屏幕截图，我们严格定义Blende中各种组件的名称。组件名称在此处以斜体显示，并在整个文本中以首字母显示。

## 默认Blender界面

当我们第一次打开Blender时，我们就会得到熟悉的默认用户界面。我们有一个立方体，一个相机对象和一个灯对象，
它被绘制到3D视窗中显示的场景中。
[图1-1](https://github.com/BlenderCN/blenderTutorial/raw/master/mDrivEngine/1-1.png?raw=true)是默认Blender界面的简单屏幕截图。
[图1-2](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-2.png?raw=true)显示了标有各种主要组件的相同界面。
我们讨论每个接口的功能

图1-1
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-1.png?raw=true)            
                        
图1-2        
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-2.png?raw=true)
                        
            
### 3D视窗

3D视窗，或简称为视窗，为我们提供了工作产品的预览。当我们在Blender中操作数据时，3D视窗会等待所有进程在更新之前完成数据写入。
这在简单的操作中并不明显，如翻译和旋转，似乎是即时和实时发生的，但在插件开发中确认仍然很重要。

3D视窗具有不同的查看选项和交互选项。查看选项包括实体，线框和渲染，而交互选项包括对象模式，编辑模式和雕刻模式。

### 标题菜单

标题菜单是图形用户界面的标准标题。它允许我们在Default，Animation和Scripting等界面布局之间切换，
以及在Blender Render，Cycles Render和Blender Game等渲染引擎之间切换。

### 属性窗口

属性窗口允许我们访问对象，场景，纹理，动画等属性。属性窗口中的大多数界面将提供摘要和基本属性，
而不是显示所有可用的详细信息。它对于跟踪现有对象，对象名称，以应用和未应用的转换以及一些其他重要属性非常有用。
此窗口通常始终以Blender艺术家的布局打开，因此它是放置插件的常用位置。

### 工具架和工具属性

工具架是按类型分组不同类别的operators的地方。如果我们展开窗口，我们可以看到工具架有各种标签，
如Tool，Create和Relations。大多数Blender插件将在工具架中创建一个新选项卡来保存其Operators和参数。

工具属性窗口是一个动态窗口，Blender根据用户具有的活动工具填充不同的参数集。例如，使用旋转工具时，
我们可以在此窗口中微调，而不是导航到属性窗口中指定旋转的确切位置。工具属性是高级功能，通常旨在优化易用性而不是为工具提供不同的功能。
许多Blender插件完全忽略它们，只有少数原生Blender工具使用它们。

### 时间轴

时间轴用于动画。我们可以忽略这一点，因为我们不会在本书中进行动画制作

## 脚本界面

要进入脚本界面，请在页眉菜单中帮助按钮右侧的下拉菜单中选择脚本选项。在整个文本中，我们将使用粗体指令来呈现这样的指令，
例如：标题菜单>屏幕布局>脚本。有关菜单位置的信息。请参见1-3.Blender的布局将更改为如图1-4所示。

图1-3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-3.png?raw=true)

图1-4

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-4.png?raw=true)

Scripting布局或其中的一些变体将是我们在Blender中完成大部分工作的地方。我们将讨论图1-5中介绍的Blender的接口的新组件。

图1-5

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-5.png?raw=true)

### 文本编辑器
我们可以在文本编辑器中编辑Python脚本(以及任何其他文本文件)。我们可以单击新建和打开按钮分别创建和打开脚本。
加载脚本后，文本编辑器底部的菜单栏将更改为允许在文件之间保存和切换。

Blender的文本编辑器具有一些与Python中的导入，系统路径和链接文件有关的特殊属性。
我们将在本章后面以及开发插件的后续章节中详细讨论这个问题。

### 命令日志

命令日志显示在会话期间由Blender接口进行的函数调用。在尝试使用脚本和了解API时，此窗口非常有用。
例如，如果我们使用红色箭头在3D视窗中平移立方体，我们将获得命令日志中清单1-1中所示的输出。

清单1-1。沿x轴平移的命令日志输出。

    bpy.ops.transform.translate(value=(3.05332, 0, 0), constraint_axis=(True, False, False),constraint_orientation='GLOBAL',mirror=False, proportional='DISABLED',proportional_edit_falloff='SMOOTH', proportional_size=1,release_confirm=True)

清单1-1中的输出显示我们从bpy.ops子模块的转换类调用了translate()函数。这些参数相当冗长，并且从界面调用时通常是多余的，
但它们很简单，我们可以破译它们的含义并尝试使用该函数。我们将在下一章节中深入研究这样的代码。
虽然解密行为通常是在Blender Python中学习函数的最佳和最快的方法，但我们也可以参考官方文档以获取更多细节。这也将在下一章中讨论。

### 交互式控制台

交互式控制台是一个Python3环境，类似于vanilla Python控制台和IPython控制台，它们通常出现在IDE(交互式开发环境)的底层。
交互式控制台不与文本编辑器脚本共享本地或模块级数据，但交互式控制台和文本编辑器脚本都可以访问存储在bpy及其子模块中的相同全局Blender数据。
因此，控制台将无法读取或修改脚本本地的变量，但共享对bpy(以及一般的Blender会话)的修改。

更复杂的是，控制台和脚本在Blender会话期间共享链接脚本和系统路径变量。这些组件之间的关系可能看起来不必要复杂，
但我们会发现它们之间的关系u对于开发和实验都是最佳的。

## 自定义界面

Blender界面的组件是模块化的，可拆卸的，可扩展的，全方位可定制的。用户可以在任何窗口的右上角拖动以修改和创建新窗口。

1.将右上角拖动到左侧将创建相同类型的新窗口。

2.向右上角拖动将允许您超越相邻的窗口。

3.按住Shift并向任意方向拖动右上角将在新的分离窗口中复制组件。

在可拆卸窗口中创建3D视窗并复制文本编辑器是使用双屏幕设置的好方法。有两个文本编辑器可用于调试自定义模块。
有关双屏幕设置的屏幕截图，请参考图1-6。

图1-6
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/1-6.png?raw=true)

请注意，如果在界面移动时工具架或工具属性窗口消失，请在3D视窗中按键盘上的T以显示它们。此外，在3D视窗中按键盘上的N以显示新窗口，
即对象属性。这个窗口经常在附加开发中使用，特别是当我们开始将自定义Blender类作为参数分配给对象时。

## 从命令行启动Blender(用于调试)

在Blender中开发Python脚本时，从命令行启动Blender非常重要。当我们在Blender中运行脚本时，如果出现错误，命令日志将显示以下消息：

    Python scripting fail，look in the console for now...

此消息可能非常混乱，因为交互式控制台将不显示任何内容。Blender的意思是：现在查看终端... 不幸的是，
大多数人不通过终端打开Blender，除非我们在后台运行Blender终端，否则错误消息和追溯将被忽视。
通过终端打开Blender是Python开发人员的非官方调试模式。Blender具有核心开发人员使用的官方调试模式，
但这对我们作为API用户通常没有帮助。
