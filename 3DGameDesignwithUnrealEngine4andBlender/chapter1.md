作为游戏开发者，我们所有人都拥有我们打梦想游戏——无论多少年过去，这一款游戏都让我们兴奋不已。对于某些人来说，这意味着要等到另一个游戏开发者构建类似的东西，但他们的版本永远不会与我们自己的版本匹配。对于我们大多数人来说，希望看到这个游戏并且能够发挥它成为开始我们在独立游戏开发中的职业生涯催化剂。当我们建立我们的梦想并将我们的心灵和灵魂倾注到游戏开发中时，我们仍然希望在今天打游戏市场中与大男孩竞争，但我们没有钱购买“AAA"游戏引擎的商业许可证。高端3D软件包。几年前，当像虚拟引擎这样大型3D游戏引擎为独立开发者免费提供时，这一切都发生了变化。现在，较小打开发人员可以访问大型开发人员喜欢的高端工具。这些新的游戏引擎让我们有能力构建我们梦想的游戏。然而，3D艺术节目从未效仿过。许多行业标准创建套件（例如Autodesk 3ds Max）仍然需要花费数千美元。2002年，随着Blener Foundation的成立，这是一个致力于支持Blender的非营利组织。Blender是一款开源3D创作软件，允许像我们这样的小型开发人员在我们的商业项目中使用我们的艺术，而无需预先花费大量资金。我们终于可以创造出梦想中的3D游戏，而不必担心我们如何能够为我们所需的工具买单。

这就是你在这里的原因。也许你已经是使用最新版本的虚幻引擎4的独立开发人员，但仍然只使用其他人创建的游戏资产。也许你是一个完全的新手，你的思想充满了需要创造的惊人的数字远景。无论哪种方式，这本书都适合你。在这些页面中，我们将了解如何一起使用Blender和虚幻引擎4为你的游戏创建自定义级别的游戏内容。

在这一章，我们将包含以下主题：

*   Installing blender
*   Exploring the interface
*   Customizing your settings
*   Working with modes
*   Jumping into our first project
*   Getting things started in Unreal Engine 4

## Installing Blender

我们的发展历程的第一步始于http://www.blender.org ,这是Blender Foundation的在线主页。在这里，你可以了解Blender的历史，与他们的社区联系，访问培训视屏等。我鼓励你在有时间的情况下查看网站，因为它有很多可供选择。例如，每次对软件进行重大更新时，都会发布动画短片。这些电影往往非常有趣，并展示了工具集的功能。

这是你下载Blender的方法：

1. Go to http://www.blender.org/ 

2. Click on the Button on the labeled [Download Blender2.8b](https://www.blender.org/download/) 

3. Blender is a cross-platform software.Select a 64-bit or 32-bit mirror for your operating system. Most likely,your computer will be 64 bit

4. Click on the Installer once it has finished downloading.

5. Follow the installation prompts.They are pretty straightforward and do not need additional configuration

一旦安装好所有内容，请继续运行程序。你会看到闪屏。现在让我们来看看界面。

## Exploring the interface

当你运行Blender时，你会看到启动画面。你最近处理过的文件列表将在左侧列出，以及一些指向各种内容的快速链接，例如文档和网站。单击闪屏两侧的空格以将其删除。现在让我们来看看默认场景。

Blender已经开始使用场景中的三个基本对象：立方体，灯光和相机，如下面的屏幕截图所示

![屏幕截图]()

右键单击对象，选择其中任何一个。这将以橙色突出显示。尝试右键单击场景中的其他对象。

![数字键]()

我们可以通过按住鼠标中键（MMB）来围绕中心旋转我们的视角。我们可以通过按住Shift+MMB并使用鼠标滚轮或+和-在数字键盘上进行缩放来滑动视图。最后，数字键盘还可以用于查看对象的特定角度

现在让我们来看一看软件界面菜单：

![界面菜单]()

1. Mnu bar

2. Tools Panel

3. Animation Timeline

4. Properties

5. Scene Properties(press N if you dont's see this menu)

6. Scene Outliner

所有前面的选项都有几个功能。在我们继续该项目时，我们将在此简要讨论它们：

*   Menu Bar:这个包含File,Render,Window,Help,Scene Layout,Browse Scene和Render下拉选项。在大多数情况下，我们将使用File菜单来保存，加载，更改首选项，导出文件以及退出程序。

*   Tools Panel：这包含了我们将用于编辑形状和塑造创作的大部分功能

*   Animation Timeline: 稍后我们将使用它来管理我们的动画游戏资产。

*   Properties: 我们经常使用面板来编辑场景的属性，添加修改器等。

*   Scene Properties: 这包含一些与场景中的项目相关的特定功能，例如比例。

*   Scene Outliner： 这是我们场景中每个对象的方便列表，如果你无法直观地找到特定对象，则会很方便。虚幻引擎也有其中之一。

很多信息，对吧？相信我，练习会变得更容易。为了使事情变得更容易，让我们自定义一些设置。

## Customizing your settings

如果没有自定义某些设置，使用Blender可能会有点麻烦，特别是如果你对其他3D软件有任何使用经验。当我们开始在界面上移动时，你可能已经立即注意到，如果你左键单击，它移动小靶心光标。这被称为3D光标，它实际上用于Blender中的很多东西，例如放置新3D形状的位置。现在，当你左键单击时移动它不太理想，有时在你尝试选择3D场景中的内容时很容易被遗忘，但有一种方法可以改变它

在屏幕的左上角，单击File，然后选择User Preferences。选择Input选项卡，向左看，知道看到Select With选项。你可以看到它当前设置为右键单击以选择对象。让我们将其更改为Left以使其更符合虚幻引擎4.当你在两个程序之间进行操作时，它会停止很多混乱。完成此更改后，请务必单击左下角的Save User Settings按钮。需要更改的第二个设置涉及规模。

下面截屏显示了Blender User Preferences窗口：

![右键选择]()

要更改此下一个设置，我们将查看Properties窗格。窗口看起来像这样：

窗格顶部有几个小标签。我们正在寻找此图标表示的场景选项卡：

![选项卡]()

此窗格允许我们更改与场景相关的多个选项，例如比例。虚幻引擎4使用厘米作为其默认度量，因此我们希望在此处匹配。这将使我们的游戏资产适合我们的水平，而无需在游戏引擎中扩展他们。跟着这些步骤：

1. 将使用的单位从None更改为Metric

2. 将比例更改为0.01

3. 场景中已存在的对象（例如我们的立方体）将无法缩放。如果你要保留任何内容，请将其缩放100.可以通过单击左侧的Scale按钮，键入100，然后Enter键来完成。

4. 这将导致对象开始剪切通过网格的边缘。要解决此问题，我们安N打开Scene Properties栏，找到View部分，然后按剪辑部分，最后将End属性更改为1km










