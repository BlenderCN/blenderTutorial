## Playground Tutorial

[![Watch the video](https://raw.github.com/GabLeRoux/WebMole/master/ressources/WebMole_Youtube_Video.png)](https://youtu.be/H5ylSfTfNg8)



这个教程将引导你完成Armory的基础知识。我们将创建一个类似游乐场的场景，逐步展示基本功能

*   获取本教程的[.blend](https://github.com/armory3d/armory_tutorials/tree/master/playground)文件。

## Hello World

从我们在[setup tutorial](https://github.com/BlenderCN/blenderTutorial/blob/master/armory_docs/Setup.md)中离开的地方继续。打开Blender，保存项目并在标题中选择Cycles Render或Eevee引擎。按Armory Player-Run（F5）在窗口中播放。

你可以在Armory Player面板中选择运行时：

*   选择Krom可快速播放场景。

*   选择Browser进行HTML5部署。

*   选择C++进行本机部署（需要C++编译器）。

你可以在Armory Player面板中选择相机模式：

*   选择Scene从活动场景摄像机开始游戏。

*   选择Viewport以从视口视图开始游戏。这对于快速场景预览很有用，因为它还可以让你控制相机。试试看！

*   在3D视图中按Shift+F可在场景中导航。

此外，你可以为窗口大小调整Dimensions-Resolution。要以全屏模式运行，请选择Armory Project-Window Mode-Fullscreen

![](https://armory3d.org/manual/getting_started/img/playground/1.jpg)

## Objects

我们将从一些关于如何操作场景对象的Blender基础开始：

*   在3D视图中，单击Space并键入Add Plane以创建平面对象（或从3D视图标题执行Add-Mesh-Plane）。

*   按S键放大平面，按R键旋转，或按G键抓取并平移。

*   right-click选择对象。

*   按X删除对象。

## Modifiers

Blender具有各种编辑器，可对活动对象应用程序效果。选择Cube，导航到Modifiers选型卡并添加Bevel修改器以使立方体边缘看起来很光亮。

![](https://armory3d.org/manual/getting_started/img/playground/1b.jpg)

## Materials

选择立方体并切换到Node Editor。转到Shader Nodes并启用Use Nodes。现在，你可以使用默认的Diffuse BSDF节点调整材质颜色和粗糙度。

![](https://armory3d.org/manual/getting_started/img/playground/2.jpg)

接下来，切换回3D View并选择平面。我们想在上面放一个纹理。按Tab键进入编辑模式，点击Space并键入Unwrap以创建平板的UV坐标。

在材料选项卡中，创建新材料。像我们之前一样切换到节点编辑器。选择Diffuse BSDF节点，然后按X删除它。在标题中，按Add-Group-Armory PBR并放入画布。将Surface套接字连接到Material Output节点。为了获得最佳效果，在使用Armory中的材料时，更偏向于使用Armory PBR节点。

![](https://armory3d.org/manual/getting_started/img/playground/grid_rough.png)

保存上面的图像，只需将文件拖放到Blender中的节点画布上即可。将Image Texture节点连接到Base Color和Roughness套接字。

![](https://armory3d.org/manual/getting_started/img/playground/3.jpg)

按照这些步骤，一个基本的场景已经形成。按F5在Armory播放。

![](https://armory3d.org/manual/getting_started/img/playground/5.jpg)

## Animation



<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/armory_docs/Setup.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/armory_docs/Tanks.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
