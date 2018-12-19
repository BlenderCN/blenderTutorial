你的level怎么样了？

它感觉像一艘货船吗？

你的游戏测试者在玩游戏时感觉如何？

请记住，对一个级别进行白盒化的目的之一是了解我们需要的道具及其相对大小。如果我们要在整个开发过程中采用这个level（并且在本章之后可以自由地完成level），我们将用详细的自定义游戏资产替换每个基本形状。但是，这有点超出了本书的范围。我们要做的是建立一个基本道具，让你开始使用Blender，让你熟悉将自定义艺术带入Unreal的过程。通过一些练习，你的水平构建能力的唯一限制将是你的想象力！在本章中，我们将介绍以下主题：

*   Getting started making game assets

*   Using the basic tools of polygon modeling

*   How to use UV mapping and why it's important

*   UV unwrapping our game asset

*   Basic texturing techniques

## Getting started making game assets

那么，在我们开始设计资产之前，我们将建设哪种资产？在我们的白盒中，我们使用了几种基本形状来表示潜在的游戏资产，这使我们有很多可供选择，例如全息投影仪，计算机控制台，管道，柱子或板条箱。由于这可能是你第一次尝试使用Blender，我们将首先为我们的货船创建一个详细的箱子来运输。一个箱子允许我们构建一些相当基本的东西，但仍然可以支持很多细节。所以继续拿自己的另一张方格纸打开你的网页浏览器；是时候草绘了！

![箱子设计草图]()

我的设计基于我在网上找到的几个不同科幻版条箱的碎片，我记得我最喜欢的一些游戏和电影（科幻碰巧是我最喜欢的类型之一）。箱子本身也设计有一个内部，所以我们可以在我的水平显示它打开和关闭盖子。这为资产提供了灵活性，并允许我在许多不同情况下使用它，这是非常重要的设计考虑因素。

你可能已经注意到我对我的箱子做了一些不同的看法。这允许我们从正面，侧面和背面看到我们的对象，并为每个部分设计一些具有多样性的对象。通过这样做，我们还可以将设计传达给团队中的人另一位艺术家，如果我们是创建设计的人而不是那些将在3D中构建资产的人。






















我们将开始使用最接近我们想要的完成形状的原始对象来构建我们的箱子。在这种情况下，我们开始使用默认多维数据集效果很好。那么什么是原始的，为什么我们从一个开始？原始形状是你的基本形状，在Blender的情况下：平面，立方体，圆形，球形，圆柱形，圆锥形和圆环形。我们从这些形状开始，因为它们是我们所看到的基本构建块。传统艺术家在绘制时也从这些开始。如果你看看外面，你可以看到这些形状：杆是圆柱体，建筑物是立方体，等等。这也是为什么Unreal为我们提供了一个充满基元的Shapes文件夹来阻挡我们的level的原因。

要学习的最简单的3D建模技术之一是多边形建模，或简单地说，多边形建模。它涉及操纵构成基本形状的顶点，边和多边形以创建更大的东西。多边形建模使用以下工具：

![Blender工具面板]()

<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/3DGameDesignwithUnrealEngine4andBlender/chapter2.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/3DGameDesignwithUnrealEngine4andBlender/chapter4.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
