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
