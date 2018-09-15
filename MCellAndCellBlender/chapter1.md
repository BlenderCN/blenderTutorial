# 1.1 Starting Blender

启动Blender时，启动画面出现在窗口的中央。它包含链接下的帮助选项和最近打开的blend文件。可以在下面找到更详细的描述。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface_splash_current.png?raw=true)

    图1：Blender Splash Screen
    
要关闭启动画面并启动新项目，请单击启动画面外的任何位置（但在Blender窗口内）或按Esc键。启动画面将消失，显示默认屏幕。

要重新打开启动，请单击信息编辑器标题中的Blender图标，或选择Infor Editor>Help>Splash Screen.

[title]()

除了Blender图标和文本外，它还显示了Blender版本。例如目前的版本是2.78.

[Image]()

一个图像，你可以在其中识别包和版本。

[Date]()

在右上角，你可以看到编译Blender版本的日期

[Hash]()

Git Hash。在诊断问题时，这对于支持人员是有用的。

[Branch]()

可选的分支ID

[Interaction]()

键配置与User prefrences>Input相同

[Links]()

链接官方网页，可以在信息编辑器的帮助菜单中找到

[Recent]()

你最近打开的blend文件。这样可以快速轻松地访问你最近的项目。

[Recover Last Session]()

Blender将尝试根据临时文件恢复上一个会话

# 1.2 Know What you're looking at

启动Blender并关闭启动画面后，你的Blender窗口应该与下图类似。Blender的用户界面在所有平台上都是一致的。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface_default_startup.png?raw=true)

    图2.The default startup Blender window。
    
## 1.2.1 Interface Elements

    Window â€£ Screen â€£ Areas â€£ Editors â€£ Regions â€£ (Tabs) â€£ Panels â€£ Controls

可以使用屏幕布局自定义界面以匹配特定任务，然后io可以对其进行命名和保存以供以后使用。默认屏幕如下所述。

屏幕被组织到一个或多个区域中，每个区域包含一个编辑器。

## 1.2.2 The Default Screen

默认情况下，Blender会启动显示默认屏幕，该屏幕分为五个区域，其中包含下面列出的编辑器：

*   The Info Editor at the top

*   A large 3D View

*   A Timeline at the bottom

*   An Outliner at the top right

*   A Properties Editor at the bottom right.

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface_introduction_default_screen.png?raw=true)
    
    图3。Blender's default Screen Layout with five Editors。
    
Info（1），3D View（2），Outliner(3),Properties(4)and Timeline(5).

## 1.2.3 Components of an Editor

通常，编辑器提供了一种通过Blender的特定部分查看和修改你的工作的方法。编辑分为几个区域。区域可以具有较小的结构元素，例如标签和面板，其中放置有按钮，控件和小部件。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface_introduction_editor.png?raw=true)

    图4：The 3D View editor
    
Yellow:Main Region,green:Header,blue:Tool Shelf,purple:Operator Panel,red:Properties Region 

## 1.2.4. User Interface Principles

[Non Overlapping]()

用户界面旨在让你一目了然地查看所有相关选项和工具，而无需推或拖动编辑器。

[Non Blocking]()

工具和界面选项不会阻止用户使用Blender的任何其他部分。Blender通常不使用弹出框（要求用户在运行操作之前填写数据）。

[Non Modal Tools]()

无需花时间在不同工具之间进行选择即可高效访问工具。许多工具使用一致且可预测的鼠标和键盘操作进行交互。

## 1.2.5.Customization

Blender还大量使用键盘快捷键来加快工作速度。这些也可以在Keymap Editor中自定义。

[Theme colors]()

Blender允许更改其大多数界面颜色设置以满足用户的需要。如果你发现屏幕上显示的颜色与手册中提到颜色不匹配，则可能是你的默认主题已被更改。可以通过选择用户首选项编辑器并单击主题选项卡来创建新主题或选择/更改预先存在的主题。

# 1.3.Screens

屏幕本质上是预定义的窗口布局。Blender的区域灵活性使你可以为不同的任务（如建模，动画和脚本）创建自定义的工作环境。在同一文件中的不同环境之间快速切换通常很有用。

屏幕数据块菜单允许你选择布局。位于信息编辑器标题中。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface_screen_data-block.png?raw=true)

## 1.3.1.Controls

[Screen Layout]()

可用屏幕布局列表如下所示

[Add]() +

单击添加按钮，将根据你当前的布局创建新的框架布局

[Delete]() X

你可以使用删除按钮删除所选屏幕

    Hint：默认情况下，每个屏幕布局都会记住上次使用的场景。选择不同的屏幕将切换到布局并跳转到该场景
    
### 1.3.1.1 Shortcuts    

要在屏幕之间循环，请使用Ctrl-Right和Ctrl-Left

    Note:在macOS上，你可能需要在计算机的首选项中禁用Mission Control的快捷方式。这些可以在System Preferences>Keyboard>Shortcuts.
    
### 1.3.1.2 Default Screens

3D View Full:全屏3D视图，用于预览场景。

Animation：让演员和其他物体四处移动，改变形状或颜色等。

Compositing:组合场景的不同部分（例如背景，演员，特效）并过滤它们（例如色彩校正）。

Default:Blender用于新文件的默认布局。用于建模新对象。

Game Logic:在Blender中规划和编程游戏。

Motion Tracking:用于使用影片剪辑编辑器进行运动跟踪。

Scripting：记录你的工作和或编写自定义脚本以自动化Blender。

UV Editing:在2D中展平对象网格的投影以控制纹理如何映射到曲面。

Video Editing:剪切和编辑动画序列。

## 1.3.2. Save and Override

屏幕布局保存在blend文件中。打开文件时，在文件浏览器中启用加载UI表示Blender应使用文件的屏幕布局并覆盖当前布局。

一组自定义的屏幕布局可以保存为startup_file的一部分。

## 1.3.3 Additional Layouts

随着你对Blender的熟练程度越来越高，请考虑添加一些其他屏幕布局以适应你的工作流程，因为这有助于提高你的工作效率。一些例子可能包括：

Modeling：四个3D视图（顶部，正面，侧面和透视），用于编辑的属性编辑器。

Lighting：用于移动灯光的3D视图，用于显示渲染结果的UV/Image编辑器，用于渲染的属性编辑器以及灯泡属性和控件。

Materials：材质设置的属性编辑器，用于选择对象的3D视图，大纲视图，库脚本（如果使用），节点编辑器（如果使用基于节点的材质）。

Painting：用于纹理绘制图像的UV/Image编辑器，用于在UV面部选择模式下直接在对象上绘制的3D视图，在背面参考图片设置为全强度的三个迷你3D视图，属性编辑器。

# 1.4 Areas

应用程序窗口始终是桌面上的矩形。它被划分为许多可调整大小的区域。区域包含特定类型编辑器的工作空间，如3D视图编辑器或大纲视图。

## 1.4.1 Arranging

Blender使用新颖的屏幕分割方法来安排区域。我们的想法是将大应用程序窗口拆分为任意数量的较小（但仍为矩形）非重叠区域。这样，每个区域总是完全可见，并且很容易在一个区域中工作并跳转到另一个区域中工作。

### 1.4.1.1 Changing the Size

你可以通过使用LMB拖动边框来调整区域大小。只需将鼠标光标移动到两个区域之间的边框上，直到它变为双头箭头，然后单击并拖动即可。

### 1.4.1.2 Splitting and Joining

#### 1.4.1.2.1 Area Split Widget

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface-window_system-arranging_areas-split_widget.png?raw=true)

在一个区域的右上角和左下角是区域分割小部件，它们看起来像一个小山脊拇指握把。它既分裂又结合了区域。将鼠标悬停在其上时，光标将变为十字

LMB并将其向内拖动分割区域。你可以通过水平或垂直拖动来自定义该边框的方向。

为了连接两个区域，LMB单击并向外拖动区域分割器。它们必须在你想要连接的方向上具有相同的尺寸（宽度或高度）。这使得组合的区域空间产生矩形。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface-window_system-arranging_areas-join_areas.png?raw=true)

    图7。The Properties Editor is being merged over the Outliner
    
将关闭的区域用箭头覆盖黑色。现在，你可以i通过将鼠标移动到它上面来选择要关闭的区域。

释放LMB以完成加入。如果在释放鼠标之前按Esc或RMB，则操作将中止。

#### 1.4.1.2.2 Area Options

边界的RMB打开区域选项。

[Split Area]()

显示一条指示线，可让你选择区域和位置以进行拆分。标签在垂直/水平之间切换。

[Join Areas]()

显示连接方向覆盖。

如上所述确认或取消操作。

### 1.4.1.3 Swapping contents

你可以在初始区域的一个分割器上使用Ctrl-LMB在两个区域之间交换内容，向目标区域拖动，然后在那里释放鼠标。这两个区域不需要并排，但它们必须位于同一个窗口内。

## 1.4.2 Duplicate Area into new Window

菜单：View>Duplicate Area into new Window

新窗口是一个功能齐全的窗口，它是Blender的同一个实例的一部分。这可能是有用的，例如.如果你有多个显示器。

可以从View>Duplicate Area创建一个新窗口到新窗口。

你还可以通过区域拆分器小部件上的Shift-LMB从现有区域创建新窗口，然后稍微拖动。

可以使用OS Close Window按钮关闭窗口。

## 1.4.3 Toggle Maximize Area

菜单：View>Toggle Masimize Area

热键：Ctrl-Up，Shift-Spacebar

最大化区域填充整个应用程序窗口。它包含信息编辑器和选择区域。

你可以使用View>ToggleMaximize Area菜单项条目最大化区域。要返回正常大小，请再次使用菜单项或编辑器标题上的RMB，然后选择最大化区域和平铺区域以返回。在Info Editor标题中，菜单右侧的Back to Previous按钮也返回到平铺区域。

更快捷的方法是使用快捷方式：Shift-Spacebar，Ctrl-Down或Ctrl-Up在最大区域和普通区域之间切换。

    Note：鼠标当前悬停的区域是使用键盘快捷键最大化的区域。
    
## 1.4.4 Toggle Fullscreen Area 

菜单：View>Toggle Full Screen

热键：Alt-F10

全屏区域仅包含主区域。仍可使用快捷键方式切换方式切换标题可见性。要退出，请将鼠标移动到区域的右上角以显示返回或使用快捷键Alt-F10。

# 1.5。Regions

编辑器细分为区域

## 1.5.1 Main Region

至少有一个区域始终可见。它被称为主要区域，是编辑器中最突出的部分。

每个编辑器都有一个特定的目的，因此主要区域和其他区域的可用性在编辑器之间是不同的。请参阅编辑器章节中有关每个编辑器的特定文档。

## 1.5.2 Header

标题是一个较小的水平条带，背景较浅，位于该区域的顶部或底部。所有编辑器都有一个标题作为菜单和常用工具的容器。菜单和按钮将随编辑器类型和所选对象和模式变化而变化。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/modeling_meshes_introduction_3d-view-header-object-mode.png?raw=true)

    图8.The Header of the 3D View editor
    
如果如果将鼠标移动到某个区域上，则其编辑器的标题会变为略浅的灰色阴影。这意味着它是聚焦的。你按下的所有热键将影响此编辑器的内容。可以使用Alt-F9隐藏标题。 

## 1.5.3 Tool Shelf

默认情况下，左侧的工具架包含工具设置。T切换工具架区域的可见性。

### 1.5.3.1。Operator Panel

操作器面板是仅包含一个面板的工具架的一部分。在3D试图中，它显示最后执行的操作器的属性，在文件浏览器中显示文件导入/导出选项。

## 1.5.4.Properties Region

默认情况下，属性区域位于右侧。它包含面板，其中包含编辑器中的对象设置和编辑器本身。N切换属性区域的可见性。

## 1.5.5.Arranging

### 1.5.5.1 Scrolling

通过使用MMB拖动区域，可以垂直和或水平滚动区域。如果该区域没有缩放级别，则可以使用滚轮滚动它，同时鼠标悬停在它上方。

### 1.5.5.2 Changing the Size and Hiding

通过拖动边框，调整区域的大小与区域的工作方式相同。

隐藏区域可将其缩小至零。隐藏区域留下一点点加号（见图）。通过LMB对此，该区域将重新出现。

工具架和属性区域具有指定用于在隐藏和显示之间切换的快捷方式。

    Hiding and showing the Header
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface-window_system-headers-hide.png?raw=true)
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/interface-window_system-headers-show_02.png?raw=true)

### 1.5.5.3 Position

要将区域从一侧翻转到另一侧，请按F5，同时区域位于鼠标指针下方。

标题也可以通过RMB翻转并从弹出菜单中选择适当的项目。如果标题位于顶部，则项目文本将显示为Flip to Bottom，如果标题位于底部，则项目文本将显示为Flip to Top。

# 1.6 common Shortcuts

许多按钮类型之间共享快捷方式。

## 1.6.1.Mouse
