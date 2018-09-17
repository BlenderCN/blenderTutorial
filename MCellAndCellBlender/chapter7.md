# Introduction to Surface Classes

表面类允许将各种属性（例如吸收性，透明性）应用于表面，这可以影响指定的分子。稍后，在[Surface Classes and Reactions](http://mcell.org/tutorials/surface_classes.html#surf-class-rxns)部分中，我们还将看到表面类如何也可用于指定反应中的绝对方向。

# Surface Classes and Volume Molecules

在本节中，我们将创建一个位于Cube对象内的Plane对象。Cube对象将填充Vol1分子。Plane对象将具有对vol分子具有吸收性的表面类。

## Create a New Project

项目目录设置为保存当前blend文件的位置。让我们立即保存文件，方法是Ctrl-s，在目录字段中输入～/mcell_tutorial/sc(或Windows上的C:\mcell_tutorial\sc),将sc.blend输入到文件名字段，然后点击另存为Blender文件按钮。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/save_blend.png?raw=true)

## Add Cube to Model Objects list

*   点击Model Objects按钮

*   点击Cube按钮然后点击+按钮将其添加到Model Object列表。

*   在Cube Object Options面板下，从下拉菜单选择wire

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/add_cube1.png?raw=true)

## Add a Volume Molecule and Release Site

*   添加一个名为vol1的体积分子，其扩展常数为1e-5。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/define_molecule.png?raw=true)

*   使用以下属性创建发布站点  

**      将Site Name设置为vol_rel.

**      将Release Shape设置为Object/Region。

**      将Object/Region设置为Cube。

**      将Quantity to Release设置为2000.

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/molecule_rel.png?raw=true)

## Add a Plane

*   点击Model Objects按钮然后点击Plane按钮。你应该有一个平面穿过立方体的中间。

*   将光标放在3D视图中，按s，然后按1.1指定缩放系数，按Enter键确认。

*   点击+按钮将其实际添加到模型对象列表中。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/add_plane1.png?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/plane_in_cube.png?raw=true)

## Add the Surface Class

*   点击Surface Classes按钮

*   点击+按钮创建一个名为Surface_Class的新表面类

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/default_surface_class.png?raw=true)

*   重命名Surface_Class为absorb_vol1.

*   点击空absorb_vol1属性列表旁边的+按钮。

*   从Molecules字段选择vol1

*   从Molecule Name字段选择vol1

*   设置orientation为Top/Front

*   设置Type为Absorptive

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/absorb_vol1.png?raw=true)

这导致任何接触absorb_vol表面类的表面的FRONT的vol1分子被破坏。

## Assign the Surface Class

现在我们已经创建了表面类，我们需要将它分配给我们的网格。

*   点击Assign Surface classes 按钮

*   点击+按钮开始编辑表面区域

*   在Surface Class Name字段内选择absorb_vol1.

*   在Object Name下，选择新创建Plane对象

*   将Region Selection设置为All Surfaces。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/mod_surf_reg1.png?raw=true)

absorb_vol1现在分配给所有Plane对象。在这个例子中，我们将表面类分配给Plane的每个面，但情况并非总是如此。

## Run the Simulation and Visualize the Results

保存Blender文件（Ctrl-s）。点击Run Simulation按钮，然后点击Export&Run按钮。

模拟运行完毕后，点击Reload Visualization Data按钮。看看你是否能注意到vol1分子被吸收表面破坏了。

## Examine the Surface Class MDL(optional)

下一节不是必需的，但如果你想了解有关MDL语法的更多信息，可以按照它进行操作。打开名为Scene.surface_classes.mdl的文件，你应该看到以下文本：

    DEFINE_SURFACE_CLASSES
    {
        absorb_vo1
        {
            ABSORPTIVE = vol1'
        }   
    }

为了重申之前的说法，上面的命令创建了一个名为absorb_vol1的表面类。由于vol1是设置为ABSORPTIVE命令的值，这意味着任何接触具有absob_vol1表面类的表面的FRONT的vol1分子都将被破坏。

现在打开名为Scene.mod_surf_regions.mdl的文件：

    MODIFY_SURFACE_REGIONS
    {
        Plane[ALL]
        {
            SURFACE_CLASS = absorb_vol1
        }
    }

再一次，重申一下，这会将absorb_vol1分配给所有的Plane。

这里的所有都是它的。另外两个表面类命令是REFLECTIVE（表面的默认状态）和TRANSPARENT（允许分子自由通过）。你可以自己尝试这些。

## Surface Classes and Reactions

在[Surface Classes and Volume Molecules](http://mcell.org/tutorials/surface_classes.html#surf-class-vol-mol)部分中，我们了解到表面类可用于为网格物体提供特殊属性。表面类别也可用于提供反应发生方式的额外特异性。

## Create a New Project

我们将在[Assign the Surface Class](http://mcell.org/tutorials/surface_classes.html#surf-class-mod-surf-reg)结尾处向右移动。事实上，除了一些细微的变化之外，说明将非常相似。

首先，我们将基于现有的sc.blend项目创建一个新项目。从File菜单中选择Save As选项。 

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/save_as.png?raw=true)

将目录字段更改为/home/user/mcell_tutorial/sc_rxn,其中user是你的用户名。单击以确认何时询问你是否要创建新目录。将blend文件名更改为sc_rxn.blend,然后单击Save As Blender File。

## Define a New Molecule

*   点击Molecules按钮

*   点击+按钮。

*   将Name更改为vol2

*   将Molecule Type更改为Volume Molecule

*   将Diffusion Constant更改为1e-6

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/vol2.png?raw=true)

## Modify the Existing Surface Class

*   点击Surface Classes按钮。

*   点击absorb_vol1 properties下的-按钮移除已存在的属性

*   重命名absorb_vol为empty。

这个修改过的表面类empty是你可以拥有表面类最简单的情况。它本身并不是很有用，但我们可以在反应中使用它来指定绝对方向性。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/empty.png?raw=true)

## Modify the Surface Regions

现在我们已经修改了表面类，我们需要将它重新分配给我们的网格。

*   点击Assign Surface Class按钮。

*   在Name字段下，选择empty。

你应该能够保留其他所有内容

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/assign_empty.png?raw=true)

## Define the Reaction

*   点击Reactions按钮

*   点击+按钮

*   将Reactants更改为vol1,@empty'.

*   将Products更改为vol2'.

*   将Forward Rate更改为1e7.

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/MCellAndCellBlender/reaction.png?raw=true)

## Run the Simulation and Visualize the Results

保存Blender文件（Ctrl-s）。点击Run Simulation按钮，然后点击Export&Run按钮。

模拟运行完毕后，点击Reload Visualization Data.按Alt-a开始播放动画。你可能需要更改vol2的颜色，因此你可以将其与vol1区分开来。

一旦你做到了这一点，你应该注意到框内部有vol2分子，但只有在它的上部，尽管vol1分子存在于平面的两侧。这是因为只有empty表面类的BACK上的vol1被认为是可能的反应物。


# Variable Rate Constants

# Release Patterns

# Clamp Concentration

# Scripting

# Dynamic Geometry

# Using GAMer to Refine a Mesh

# Advanced Plotting

# Periodic Boundary Conditions
