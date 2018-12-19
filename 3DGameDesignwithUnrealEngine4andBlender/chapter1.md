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

![屏幕截图](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE.png)

右键单击对象，选择其中任何一个。这将以橙色突出显示。尝试右键单击场景中的其他对象。

![数字键](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/%E6%95%B0%E5%AD%97%E9%94%AE.png)

我们可以通过按住鼠标中键（MMB）来围绕中心旋转我们的视角。我们可以通过按住Shift+MMB并使用鼠标滚轮或+和-在数字键盘上进行缩放来滑动视图。最后，数字键盘还可以用于查看对象的特定角度

现在让我们来看一看软件界面菜单：

![界面菜单](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/%E7%95%8C%E9%9D%A2%E8%8F%9C%E5%8D%95.png)

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

![右键选择](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/%E5%8F%B3%E9%94%AE%E9%80%89%E6%8B%A9.png)

要更改此下一个设置，我们将查看Properties窗格。窗口看起来像这样：

窗格顶部有几个小标签。我们正在寻找此图标表示的场景选项卡：

![选项卡](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/%E9%80%89%E9%A1%B9%E5%8D%A1.png)

此窗格允许我们更改与场景相关的多个选项，例如比例。虚幻引擎4使用厘米作为其默认度量，因此我们希望在此处匹配。这将使我们的游戏资产适合我们的水平，而无需在游戏引擎中扩展他们。跟着这些步骤：

1. 将使用的单位从None更改为Metric

2. 将比例更改为0.01

3. 场景中已存在的对象（例如我们的立方体）将无法缩放。如果你要保留任何内容，请将其缩放100.可以通过单击左侧的Scale按钮，键入100，然后Enter键来完成。

4. 这将导致对象开始剪切通过网格的边缘。要解决此问题，我们安N打开Scene Properties栏，找到View部分，然后按剪辑部分，最后将End属性更改为1km

现在我们已经进行了这些更改，请使用File菜单或Ctrl+S保存文件。

## Working with modes

Blender还有一个我们还没有谈过的菜单。这是一个位于3D视图的小菜单栏，如下所示：

![菜单](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/%E8%8F%9C%E5%8D%95.png)

它包含了View，Select，Add和Object菜单。它还包含模式下拉列表。我们将本书中使用许多不同模式，但我们将大部分时间都花在编辑模式上。这种模式允许我们将基本形状推入和拉出到我们的新创作中。Blender允许我们通过Tab键轻松在Object模式（默认模式）和Edit模式之间切换。仅当你有一个可在场景中选择的对象时，此功能才有效。你会注意到更改模式时许多菜单都会发生变化。当我们在第三章创建第一个对象时，我们将更多地探索这个，这是自定义的时间！

## Jumping into our first project

本书分为两个项目，让我们在Blender中创建自定义游戏资源，并将它们添加到虚幻引擎4中构建的level。第一个项目将是在我的10年教学中教过许多学生创建的基本关卡经验：两个房间通过走廊连接，有一些楼梯，有功能的门和电梯。虽然levels本身并不复杂，但我们将贯穿整个设计过程，从构思到原型，再到最终产品。然后，我们将使用相同的过程在本书后面设计一个更复杂的level。这些关卡本身将以科幻恐怖主题为基础创建。有一个主题将联合这两个层次，并为我们提供一种艺术风格，以便在设计中自定义level资产时使用。

那么你如何从头开始设计一个level？我们的流程将遵循几个不同的步骤：

1. 每件好事都从纸上开始。艺术家用草图开始创意。建筑师有蓝图。高级设计师用地图草图开始他们的level。我推荐方格纸。我们也将通过草图开始我们的自定义游戏资产。

2. 在level编辑器中使用基本形状开始布局level。脚本游戏序列。测试level以查看布局是否适用于玩家。这称为白盒或boxing out你的level。它本质上是一个level原型。

3. 一旦你的白盒级别达到你希望的level，请使用blocked out的大小开始创建和添加游戏资产。这允许我们在Blender中创建具有正确大小的资源并填充我们level中的正确空间。

4. 添加游戏资源，调整光照并添加特效。

5. 不要忘记玩游戏，并在每一步都搜集玩家的意见！

现在让我们在虚幻引擎4中启动我们的项目

## Getting things started in the Unreal Engine 4

在我们开始使用Unreal之前，请确保已安装Epic Games Larncher。这可以在https://www.UnrealEngie.com/ 免费下载，可以通过单击页面右上角的Get Unreal按钮来下载。系统会要求你创建一个账户，启动器会在你运行时间询问此信息。

接下来，单击屏幕左侧的Library按钮。在标有引擎版本的部分中，单击版本4.9.2上的启动(撰写本文时的最新版崩）。如果没有引擎可见，请选择添加版本并按照提示操作：

![Unreal 版本](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/Unreal%E7%89%88%E6%9C%AC.png)

一旦引擎加载，你将看到你正在处理的所有项目。对于这个，让我们开始一个新项目。单击顶部的New Project选项卡，如以下屏幕截图所示：

![Unreal 加载](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DGameDesignwithUnrealEngine4andBlender/Unreal%E5%8A%A0%E8%BD%BD.png)

在这一点上，Unreal为你提供了一些选择。该引擎附带了许多免费的入门项目，可以帮助你开始使用多种不同类型的项目。对于我们的项目，我们将使用First Person启动项目。确保Starter Content按钮显示With Starter content并为项目指定一个没有空格的唯一名称。完成所有设置后，单击右下方的Create Project。虚幻引擎将加载，我们将设置为启动我们的level。

## Summary

在本章中，你将了解用于将我们的关卡理论付诸实践的工具。Blender是一个免费提供的开源创建套件，支持整个资产开发过程。使用Python编程语言进行处理，Blender足够灵活，几乎可以在任何机器上运行，并且完全是跨平台的；它可以在Windows,Mac或Linux中运行。接下来，你将了解将要使用的设计过程以及Blender和Unreal Engine 4如何插入其中。最后，你设置了Unreal来开始我们的第一个项目，当你跳转到下一章时，你将构建你的第一个level，该level将主持你的第一个原始游戏资产。你能感受到兴奋玛？？




<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/3DGameDesignwithUnrealEngine4andBlender/README.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/3DGameDesignwithUnrealEngine4andBlender/chapter2.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>



