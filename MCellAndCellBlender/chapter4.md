# 4.Unimolecular Reactions

## 4.1.Tutorial Overview

本教程将定义单个单分子反应。

## 4.2Intitial Configuration

本教程建立在[Single Molecule Diffusion](http://mcell.org/tutorials/single_molecule_diffusion.html#single-molecule-diffusion)的基础上。要么自己完成教程，要么使用[single_molecule.blend](http://mcell.org/tutorials/blends/single_molecule.blend)文件开始。

### 4.2.1.Save the File with a New Name in Your Working Directory

*   选择File>Save As...

*   将single_molecule.blend更改为unimol_reactions.blend

*   单击Save As Blender File按钮

### 4.2.2 Define a Reaction

*   单击Reactions按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reactions_button.png?raw=true)

*   单击Defined Reactions框右侧的加号（+）符号

*   在Reactants文本字段中键入a

*   在Products文本字段键入NULL

*   在Forward Rates文本字段键入5e3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/define_reactions1.png?raw=true)

### 4.2.3 Release More Molecules

*   单击Molecule Placement按钮
*   将Quantity to Release更改为1000

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/release_1000.png?raw=true)

### 4.2.4。Create Reaction Data

*   单击Plot Output Settings按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/plot_button.png?raw=true)

*   单击Reaction Data Output框右边的加号（+）符号

*   从Molecule选择器选择a

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/plot1.png?raw=true)

### 4.2.5。Simulate the Model

*   单击Run Simulation按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim_button.png?raw=true)

*   单击Run按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim.png?raw=true)

*   等待仿真完成

*   按Reload Visualization Data按钮加载仿真结果

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reload_viz_data.png?raw=true)

### 4.2.6.Use the Tme Line

*   在时间线下按播放（![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/play.png?raw=true)）按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/150_iters.png?raw=true)
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/750_iters.png?raw=true)

*   通过单击时间线线下的暂停（![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/pause.png?raw=true)）按钮暂停仿真。

### 4.2.7 plot the Reaction Data

*   单击Plot Output settings按钮

*   如果可用，请从绘图仪下拉列表中选择MatPlotLib Plotter

*   点击Plot按钮。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/plot2.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/plot3.png?raw=true)

### 4.2.8.Save Your File

* File>Save

