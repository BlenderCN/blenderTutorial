# Biomolecular Reactions

## 6.1.Tutorial Overview

本教程将定义单个双分子反应

## 6.2.Initial Configuration

本教程建立在[Add Simple Mesh Geometrry](http://mcell.org/tutorials/add_meshgeom.html#add-meshgeom)的基础上。要么自己完成该教程，要么使用[add_meshgeom.blend](http://mcell.org/tutorials/blends/add_meshgeom.blend)文件开始。

### 6.2.1.Save the File with a New Name In Your Working Directory

*   选择File>Save As...

*   将add_meshgeom.blend更改为bimol_reactions.blend

*   单击Save As Blender File按钮

### 6.2.2.Define a New Molecule Type

*   单击Molecules按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecules.png?raw=true)

*   单击Defined Molecules右侧的加号（+）符号

*   单击Molecule Name字段，键入字母b并按Enter键

*   单击Diffusion Constant框，键入1e-6并按Enter键

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/define_b.png?raw=true)

### 6.2.3.Release b Molecules into the simulation

*   单击Molecule Placement按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecule_placement.png?raw=true)

*   单击Release/Placement Sites框的加号（+）符号

*   将Site Name更改为Release_Site2

*   单击Molecule字段并且选择b分子

*   将Release Shape更改为Object/Region

*   在Object/Region字段中键入Icosphere

*   单击Quantity to Release字段并且将其设置为1000

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/release_site2.png?raw=true)

### 6.2.4.Define a Biomolecular Reaction

*   单击Reactions按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reactions_button.png?raw=true)

*   将Reactants文本字段从a更改为a+b

*   在Forward Rates文本字段键入5e3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/change_reaction.png?raw=true)

### 6.2.5.Simulate the Model

*   单击Run Simulation按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim_button.png?raw=true)

*   将Tiem Step更改为1e-5

*   单击Run按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim.png?raw=true)

*   等待仿真完成

*   按Reload visualization Data按钮加载按钮的结果

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reload_viz_data.png?raw=true)

### 6.2.6.Change the Shape of b

*   单击Molecules按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecules.png?raw=true)

*   如果尚未打开Display Options子面板，请打开它

*   将Cone更改为Sphere_2

*   将Scale更改为2.5

*   将颜色更改为亮红色

### 6.2.7.View the Results

*   按时间线下面的播放（![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/play.png?raw=true)）按钮

### 6.2.8.Save Your File

*   File>Save
