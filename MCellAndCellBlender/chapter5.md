# 5.Add Simple Mesh Geometry

## 5.1.Tutorial Overview

在本教程中，一个球体将被添加到模拟周围的扩散分子。

## 5.2.Initial Configuration

本教程建立在[单分子反应](http://mcell.org/tutorials/unimol_reactions.html#unimol-reactions)中。要么自己完成自己的教程，要么使用[unimol_reactions.blend](http://mcell.org/tutorials/blends/unimol_reactions.blend)文件开始。

### 5.2.1.Save the File with a New Name in Your Working Directory

*   选择File>Save As...

*   将unimol_reactions.blend更改为add_meshgeom.blend

*   单击Save As Blender File按钮

### 5.2.2.Add Sphere

*   单击Create标签

*   点击Ico Sphere按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/add_icosphere1.png?raw=true)

*   Add Ico Sphere选项显示在左下角

*   将Subdivisions更改为3

*   将Size更改为0.5

*   如果尚未设置，则将每个位置（X,Y和Z)更改为0.0.

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/add_icosphere2.png?raw=true)

### 5.2.3.Add Sphere to Model Objects

*   单击CellBlender标签

*   单击Model Objects按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/model_objects_button.png?raw=true)

*   单击模型对象框右侧的加号（+）符号

*   展开Icosphere显示选项面板

*   从下拉菜单中选择Wire

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/model_objects.png?raw=true)

### 5.2.4.Release Molecules inside Sphere

*   单击Molecule Placement菜单

*   选择Release_Site

*   将Release Shape更改为Object/Region

*   在Object/Region字段中键入Icosphere

*   将Quantity to Release更改为1000

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/sphere_release.png?raw=true)

#   5.2.5.Simulate the Model

*   单击Run Simulation按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim_button.png?raw=true)

*   单击Run按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/run_sim.png?raw=true)

*   等待仿真完成

*   按Reload Visualization Data按钮加载仿真结果。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reload_viz_data.png?raw=true)

### View the Results

*   按时间线下面的播放（![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/play.png?raw=true)）按钮

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/diffuse_in_sphere.png?raw=true)

### 5.2.7。Save Your File

* File>Save
