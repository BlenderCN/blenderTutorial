![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree1.jpeg?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree2.jpeg?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree3.jpeg?raw=true)

打开Blender界面，

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree4.jpeg?raw=true)

点击文件>用户设置（快捷键ctrl+alt+u）

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree5.jpeg?raw=true)

打开用户设置后，点击插件

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree6.jpeg?raw=true)

点击添加曲线。接着我们找到Sapling Tree Gen。点击前面方块的勾选。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree7.jpeg?raw=true)

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree8.jpeg?raw=true)

接着我们关闭用户设置面板，在Blender界面中使用快捷键shift+a，调出添加项，选择添加曲线中的Sapling Tree Gen

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree9.jpeg?raw=true)

此时我们的界面中就会出现一个树木的结构。在我们的右侧也会出现Sapling:AddTree操作板

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree10.jpeg?raw=true)

在设置中选择几何数据。勾选倒角。此时你的树干就出现了。如下图所示：

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree11.jpeg?raw=true)

接下来我们开始设置树枝的高度。在设置中选择：Branch Splitting，在面板中修改Trunk Height的数值和Branches中第二项值。
（Trunk Height的数值控制的是的是树枝到树干底部的高度值，Branches控制的是树枝从树干顶端开始散开的树枝数。
这些数据不是唯一的固定值，建议大家进行不同尝试。）

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree12.jpeg?raw=true)

我们再次在设置中选择：Branch Growth，在下面的面板中进行树枝的微调。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree13.jpeg?raw=true)

然后在Branch Radius中分别修改半径缩放，Radius Scale Variation,Root Flare, Tweak Radius。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree14.jpeg?raw=true)

接着我们在设置中选择Leaves。我们来制作树叶，首先我们都选Show Leaves,然后修改Leaves数值，并对下面的其他数值进行调整，调整出满意的效果。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/tree15.jpeg?raw=true)

[link](https://mp.weixin.qq.com/s/di-X6OOB6UrXRzTMI-605g)

  
    bpy.ops.curve.tree_add(
        do_update=True, 
        chooseSet='5', 
        bevel=True, 
        prune=False, 
        showLeaves=True, 
        useArm=False, 
        seed=0, 
        handleType='0', 
        bevelRes=1, 
        resU=4, 
        levels=2, 
        length=(1, 0.5, 0.6, 0.45), 
        lengthV=(0.15, 0.1, 0.15, 0.25), 
        taperCrown=0, 
        branches=(0, 150, 30, 10), 
        curveRes=(8, 5, 3, 1), 
        curve=(0, 45, -40, 0), 
        curveV=(20, 50, 75, 75), 
        curveBack=(0, 0, 0, 0), 
        baseSplits=0, 
        segSplits=(0, 0, 0, 0), 
        splitByLen=True, 
        rMode='rotate', s
        plitAngle=(8, 20, 25, 30), 
        splitAngleV=(2, 5, 5, 5), 
        scale=13, scaleV=3, 
        attractUp=(0, -1, 0.5, 0.5), 
        attractOut=(0, 0, 0, 0), 
        shape='7', 
        shapeS='4', 
        customShape=(0.5, 1, 0.3, 0.5), 
        branchDist=1.35, 
        nrings=0, 
        baseSize=0.76, 
        baseSize_s=0.25, 
        leafBaseSize=0.2, 
        splitHeight=0.2, 
        splitBias=0, 
        ratio=0.015, 
        minRadius=0.0015, 
        closeTip=False, 
        rootFlare=1.25, 
        splitRadiusRatio=0.75, 
        autoTaper=True, 
        taper=(1, 1, 1, 1), 
        noTip=False, 
        radiusTweak=(1, 1, 1, 1), 
        ratioPower=1.2, 
        downAngle=(90, 45, 45, 45), 
        downAngleV=(0, 75, 10, 10), 
        useOldDownAngle=False, 
        useParentAngle=True, 
        rotate=(99.5, 137.5, 137.5, 137.5), 
        rotateV=(15, 0, 0, 0), 
        scale0=1, 
        scaleV0=0.05, 
        pruneWidth=0.4, 
        pruneBase=0.3, 
        pruneWidthPeak=0.6, 
        prunePowerHigh=0.5, 
        prunePowerLow=0.001, 
        pruneRatio=1, 
        leaves=200, 
        leafType='0', 
        leafDownAngle=60.84, 
        leafDownAngleV=30, 
        leafRotate=90, 
        leafRotateV=10, 
        leafObjZ='+2', 
        leafObjY='+1', 
        leafScale=2.5, 
        leafScaleX=0.05, 
        leafScaleT=0.45, 
        leafScaleV=0.1, 
        leafShape='hex', 
        leafangle=-30, 
        horzLeaves=True, 
        leafDist='6', 
        armAnim=False, 
        previewArm=False, 
        leafAnim=False, 
        frameRate=1, 
        loopFrames=0, 
        wind=1, 
        gust=1, 
        gustF=0.075, 
        af1=1, 
        af2=1, 
        af3=4, 
        makeMesh=False, 
        armLevels=2, 
        boneStep=(1, 1, 1, 1), 
        matIndex=(0, 0, 0, 0))  
