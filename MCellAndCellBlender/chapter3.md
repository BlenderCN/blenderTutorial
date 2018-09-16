# 3.Single Molecule Diffusion

## 3.1.Required software

本教程旨在与CellBlener1.软件包（linux，windows）一起使用。如果使用OSX。请从头开始安装CellBlener（CellBlender安装（面向开发人员和高级用户），注意：首先安装Blender）

## 3.2 Tutorial Overview

本教程将定义单个分子并显示其扩散。

## 3.3 Initial Configuration

如果你尚未安装CellBlender1.1软件包，请安装它。

### 3.3.1 Save the File with a New Name in Your Working Directory

*   启动Blender

选择File>Save As

将untitled.blend更改为single_molecule.blend

单击Save As Blender File按钮

### 3.3.2 Define a Molecule species

*   单击Molecules按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecules.png?raw=true)

*   单击Defined Molecules框右边的加号（+）符号

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/define_molecule.png?raw=true)

*   单击名称字段，键入字母a，然后按Enter建

*   新分子a应在Defined Molecules框中具有绿色复选标记

*   单击Diffusion Constant框，键入1e-6并按Enter键。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/define_molecule2.png?raw=true)

### 3.3.3 Release a Single Molecule into the Simulation

* 单击Molecule Placement按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecule_placement.png?raw=true)

*   单击Release/Placement Sites框右侧的加号（+）符号

*   单击Molecule字段并选择a分子

*   单击Quantity to Release字段并将其设置为1.

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/release_one.png?raw=true)

### 3.3.4 Simulate the Model

*   单击Run Simulation按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim_button.png?raw=true)

*   单击Run按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim.png?raw=true)

*   模拟完成后，MCell进程列表中将出现绿色复选标记

*   按Reload Visualization Data按钮加载模拟结果

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reload_viz_data.png?raw=true)

### 3.3.5 Change Settings to See Results

*   隐藏屏幕底部中间附近的Manipulator。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/manipulater_location.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/hide_manipulator.png?raw=true)

*   单击Molecules按钮。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecules.png?raw=true)

*   打开Display Options子面板  

*   将Sphere_1改为Torus

*   将Scale更改为5

*   将颜色更改为亮黄色

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/display_options.png?raw=true)

    你会注意到实际上有两个圆环物体。其中一个应该在播放模拟时移动，另一个应该在原点静止。原点上的圆环实际上是Blender使用的模板分子。对于你定义的每个分子种类，在原点始终会有一个模板分子。
    
### 3.3.6 Use the Time Line
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/timeline.png?raw=true)

*   按时间线下方的播放![play](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/play.png?raw=true)按钮

*   使用滚轮放大，直到你可以看到移动的圆环

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/single_diffusing_molec.png?raw=true)

*   单击时间线下方的暂停![pause](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/pause.png?raw=true)按钮停止模拟

*   单击时间线上的各个位置以查看当时的分子状态

*   在时间线中单击并拖动以随时间scrub模拟

### 3.3.7.Save Your File

*   File>Save
