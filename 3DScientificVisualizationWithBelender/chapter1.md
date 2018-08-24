# Introduction

## 1.1 科学中的可视化

科学无所不包，能够提供事实的逻辑路径，革新技术和改变我们所知道的世界，并激发下一代技术进步。
通过科学方法，我们形成问题并发展假设，数据可视化在测试和分析中起着关键作用。
我们如何向非科学和科学的所有受众展示科学和工程成果，概念和想法，从而深入了解实验。

现代计算为科学界提供了将数据探索提升到新水平的工具。挖掘大型数据集需要高效的数据组织和可视化。
有时这是由于数据的磁盘大小，受内存或处理能力的限制。图形处理单元（GPU）的发展，
无论是用于驱动高端图形应用程序还是线程处理，都可以创建复杂的高分辨率图像和动画。在其他情况下，
数据可能具有要探索的复杂相空间。数据可以具有N维表或数量之间的相关性，其仅通过3D视觉显示自己。

许多科学学科的许多前沿研究都受益于科学可视化。

[天文](https://github.com/BlenderCN/blenderTutorial/blob/master/3DScientificVisualizationWithBelender/chapter1.md#11-%E7%A7%91%E5%AD%A6%E4%B8%AD%E7%9A%84%E5%8F%AF%E8%A7%86%E5%8C%96) 跨越电磁波谱的望远镜观测现在每小时产生数TB的数据。存储和管理原始数据是一项挑战，
更不用说可视化已通过必要的校准程序推送的已处理数据。
对来自单一波长范围的一百万个星系的调查将具有适当组织和索引以便有效搜索的元数据。 
利用射电望远镜的宽带无线光谱提供有关天空发射的2D信息以及沿第三轴的频率信息。

[物理](https://github.com/BlenderCN/blenderTutorial/blob/master/3DScientificVisualizationWithBelender/chapter1.md#11-%E7%A7%91%E5%AD%A6%E4%B8%AD%E7%9A%84%E5%8F%AF%E8%A7%86%E5%8C%96) 诸如大型强子对撞机等尖端项目需要先进的可视化软件。粒子物理中的理论模型，固态物理中的晶格，
高温等离子体和引力波的物理特性都受益于新的可视化技术。在设计科学仪器之前对其进行可视化可以帮助优化实验的工程和设计。

[化学/生物](https://github.com/BlenderCN/blenderTutorial/blob/master/3DScientificVisualizationWithBelender/chapter1.md#11-%E7%A7%91%E5%AD%A6%E4%B8%AD%E7%9A%84%E5%8F%AF%E8%A7%86%E5%8C%96) 可以使用3D动画检查反应期间的复杂分子和分子动力学。使用来自核磁共振（NMR）研究的数据研究蛋白质可以揭示它们的结构。
GPU加速处理允许可视化复杂病毒的可扩展解决方案。

[地理/行星科学](https://github.com/BlenderCN/blenderTutorial/blob/master/3DScientificVisualizationWithBelender/chapter1.md#11-%E7%A7%91%E5%AD%A6%E4%B8%AD%E7%9A%84%E5%8F%AF%E8%A7%86%E5%8C%96) 存在大量的测绘数据，不仅适用于地球，也适用于我们太阳系中其他被调查的行星。使用特殊数据存储模型和GPU处理，
现在可以实时渲染行星表面的3D地图。

[医学](https://github.com/BlenderCN/blenderTutorial/blob/master/3DScientificVisualizationWithBelender/chapter1.md#11-%E7%A7%91%E5%AD%A6%E4%B8%AD%E7%9A%84%E5%8F%AF%E8%A7%86%E5%8C%96) 可视化计算机轴向断层扫描（CAT扫描）可以显示有机结构的透明视图。
通过执行诊断的分类和特征提取可以极大地从能够查看内部器官的实时3D影视中获益

## 1.2 Blender是什么？

Blender是一种开源软件，允许用户创建高质量的3D模型和数据动画。它在视频游戏和娱乐行业中有广泛的用途。
该软件对于生成高质量的科学可视化非常有用。通过组织良好的Python应用程序接口（API），
可以编写叫本来从数值模拟中加载数据。当用户完全控制摄像机角度，视野和渲染最终动画的各个方面时，
Blender的强大功能变得明显。

Blender的传统用户群是从事建模和动画工作的3D图形专家。然而，
凭借直观的图形用户界面（GUI）和用于读取各种科学数据的Python库的可用性，Blender为现代科学工作台带来了令人兴奋的独特可视化套件。
开发人员/创建者Ton Roosendaal和Blender基金会创建了一个社区，以最大限度地为开发人员和用户提供在线资料。
本书的目的是通过科学实例向读者提供有趣和实用的Blender介绍，每个介绍重要的软件功能和3D图形技术。

图1-1显示了Blender的结构及其主要功能。本书将检查以下每个主题，然后将它们一起用于一系列示例可视化项目：

*   Meshes and models

*   Lighting

*   Animation

*   Camera control

*   Scripting

*   Composites and rendering

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/1-1.png?raw=true)
图1-1。此流程图显示了Blender工作流程图如何用于科学可视化。整本书将分析Blender GUI和API的不同方面，以展示各个部分如何在项目中组合在一起。
