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



### 时间线