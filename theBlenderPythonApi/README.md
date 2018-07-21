# The Blender Python API: Precision 3D Modeling and Add-on Development

![](https://chrisconlan.com/wp-content/uploads/2017/06/blender_python_api-717x1024.png)

by[Chris Conlan](https://github.com/chrisconlan)


# Introduction

本文详细介绍了Blender Python API中3D建模工具的开发和使用。我们挑战通过构建精确的数据驱动模型，将Blender视为纯粹的艺术家工具。
同时，我们通过在熟悉的Blender环境中部署自定义工具，教你如何帮助和启用艺术家。本文中提供的知识是对Blender’s的深刻理解的结果，文档和
源代码，以及Blender核心编写的附加组件的源代码开发人员。作者发现了许多又用的功能，截至撰写本文时，无证。值得庆幸的是，我们用户可以通过
倾听和学习来保持最前沿开发人员。本文统一了文档齐全的介绍性材料和未记录的高级材料创建一个强大的参考。本书包含代码示例和强大脚本和附加组件的屏幕截图。我们包括用于自动执行精确任务的脚本，否则这些任务很难手动实现。此外，我们使用新工具，对象和自定义来构建附加组件，以增强Blender现有的功能选项。

# Definitions

3D建模是操纵数据以创建对象和环境的3D表示的艺术。3D艺术家使用以下工具和技术来构建3D模型。

  1 手动建模涉及艺术家与软件界面交互，这可以是
  
    1 使用3D建模套件（blender，Mayahuo3d Max）进行创建和编辑手工制作的物品
    2 使用3D建筑元素（Minecraft，Fallout4或Sims）玩视频游戏
    4 手动将数据输入到3D对象文件（.obj,.stl或glTF）
 
  2 自动建模涉及算法生成3D模型。这可以是：

    1 程序生成视频游戏中的环境和角色
    2 根据建筑规范生成详细的建筑模型
    3 从分形算法生成3D打印艺术
    
  3  基元是3D模型的基本构建块。虽然没有严格的关于什么构成原语的规则，这些可以是：

    1 简单的封闭形状，如平面，立方体和金字塔
    2 简单的曲线形状，如球形，圆柱形和锥形
    3 复杂的形状，如tori（复数圆环），Bezier曲线，Nurbs曲面
    
3D模型是对象和环境的数据表示。3D模型具有以下特征组件

  1 数据格式允许模型通过应用程序进行区分和专门化。每种类型的3D模型都具有指定它的格式。这些包括：

    1 特定于套件的格式，例如.blender for Blender,.3ds for 3ds Max 和 .ma for Maya
    2 特定于渲染器的格式，如.babylon for BabylonJS,.json几何描述符（3JS）和.glsl(OpenGL着色器）
    3 简单的交换格式，如.obj和.stl
    
  2 顶点和面定义了在3D空间中连接这些点的点和曲面。
  
    1 顶点是实数3D空间的三元组，或对象的每个点的传统（x,y,z)坐标
    2 面是整数的三元组，其中（i，j，k）表示由第i，第j和第k个顶点形成的3D空间中的三角形。
    
# 本书的必备知识

本书涵盖了运行Python3.5.2的Blender版本2.78c。大多数例子都在Blender2.70和更大，这些概念一般适用于Blender。
尽管如此，建议读者使用Blender2.78c最好跟随。在我们讨论Blender和Python语言的历史和发展时，
我们将指出不太可能用于过去和未来版本的编程实践。

  我们假设Blender和Python3的基本工作知识。熟悉任何版本的Blender2.60或更高版本就足够了。同样，纯Python2程序员也没有问题。
  
# 资料概述

本文介绍知识并依据它构建，以创建越来越完整和复杂的软件解决方案。我们介绍和讨论以下主要议题

## 第一章 Blender 接口

Blender有许多独立的接口。核心接口具有容易交互因为几乎所有可能的用户都直接与Python函数相关联。
我们对Python编程特别重要的部分建立了相似性。

Blender接口将充当你的软件的部署和开发环境。我们讨论了在Blender接口中保留编程和测试Python的特别注意事项。

为了尽量减少整篇文章中截图的使用，我们引入了重要的词汇来讨论Blender接口。使用这个词汇表，我们可以专注于Python代码，
同时允许用户在他们自己喜欢的Blender接口的首选布局中工作。

## 第二章 bpy模块

bpy模块是Blender Python API的核心。学习浏览此模块将大大提高你对Blender和API的理解。在本书的早期，
我们专注于构造对象和操纵其关联元数据bpy中的类。在本书的后面，我们访问bpy模块中的新类，将脚本转换为插件。

模块本身非常冗长。早期的脚本既复杂又重复。在完成对象创建和操作后，我们将开始为我们将在本书中构建的工具包添加有用的函数。
我们将在工具包中存储复杂且常用的算法，但鼓励读者将bpy模块的核心元素提交到内存中。通过这种方式，我们创建了易于编写且易于共享的代码。

## 第三章 bmesh模块

bmesh模块是一个相对较新的模块，它试图简化对象数据的复杂顶点级操作。对于那些熟悉Blender的读者来说，
bmesh中的大多数操作只能在编辑模式而不是对象模式下运行。这有助于强制bmesh中的函数用于粒度变化而不是网格数据的全局变换。

作者认为，该模块将Blender Python API与其他自动3D建模软件区分开来。bmesh模块为我们提供了对Blender的大型编辑模式工具的算法访问，
用于顶点级，边级和面级对象操作。它允许我们为数百而不是数千代码的非常复杂的对象编写程序生成算法。

## 第四章 建模和渲染主题

对于从事3D建模工作的人来说，必须对我们依赖的机制有一个基本的了解，以便呈现和和可视化我们的工作产品。
我们将讨论渲染管道的基础知识以及Blender Python开发的重要渲染主题。
Blender和我们导出的可视化器中的许多感知错误和奇怪行为实际上是渲染器的预期行为。
我们学习如何检测和编程这些行为，以确保我们创建高度可移植的模型。

我们
