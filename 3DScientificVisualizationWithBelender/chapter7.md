# Python 脚本

Blender API可用于Python脚本中，以自动执行任务并读取科学数据。API允许用户创建和操作对象和数据，
编辑属性和分组，以及控制科学可视化的动画序列。

## blender中的脚本

Blender脚本界面如图7-1所示。启动新脚本时，GUI中会出现Run Script按钮——按此按钮将运行脚本。此外，Blender脚本可以通过命令行运行。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3DScientificVisualizationWithBelender/7-1.png?raw=true)

    图7-1。编辑器和命令行允许用户通过Blender Python API控制Blender接口和函数。

    blender --python myscript.py
        
### 7.1.1 Blender和Python库

以下库将与Blender脚本一起用于科学可视化。这些通常会出现在带有import的脚本前导码中。

[bpy]() 允许控制上下文，数据类型，运算符和GUI应用程序的应用程序模块。

[bmesh]() 一个独立模块，可以访问网格对象，顶点和连接段

[csv]() 用于读取格式化数据的有用Python模块。

[glob]() 标准Python模块，用于遍历目录结构并获取要加载的文件列表。

[os]() 一种操作系统接口，可用于加载/查找可能需要所需库的路径。

[math]() 基本的数学运算和函数

### 7.1.2 示例：将格式化的ASCII数据读入blender

有许多不同的方法可以将格式化文本读入Python字典。我们在这里使用词典是为了在定义Python数据结构方面的清晰度和方便性。
此简短示例的数据将采用以下格式：

    # Name      X       Y       Z
    Point001    4.2     2.3     5.6
    Point001    1.2     9.3     6.4
    Point003    4.3     2.3     7.4
    ...
    
然后我们可以使用以下简短的Python脚本读入指定的数据。

    import bpy
    import bmesh
    import csv
    
    “"" Read in the data according to the defined list of fields
    Remove the final header if needed。
    Define the table as an empty list
    ”""
    
    fields = ['Name','X','Y','Z']
    csvin = csv.reader(open(f))
    data = [row for row in csvin]
    header = data.pop(0)
    table = []
    
    """ For each row in the data list,map the fields and
    data elements to a dictionary
    Append the dictionary to the table list.
    """
    
    for row in data:
        datarow = map(float,row[0].split())
        rowdict = dict(zip(fields,datarow))
        table.append(rowdict)

### 7.1.3 示例：将数据点作为顶点添加到对象

对于此示例，我们创建一个立方体对象网格（实际上，任何网格都可以工作）并删除一个顶点之外的所有网格。
然后，该顶点将在导入的ASCII文本文件的所有位置复制。

*    单击Add——Mesh——Cube创建UV立方体网格。或者，可以使用键盘快捷键SHIFT-A。

*    按Tab键进入网格编辑模式。CTRL-right-click所有顶点但只要一个，然后按X删除它们。

*    运行以下脚本，将GUI切换到编辑模式，并复制从ASCII表加载的每个X，Y，Z点的顶点：

    import bpy
    import bmesh
    import csv
    
    # Switch to edit mode
    bpy.ops.object.mode_set(mode = 'EDIT')
    obj = bpy.data.objects['Cube']
    mesh = obj.data
    bm = bmesh.from_edit_mesh(obj.data)
    rowcount = 0
    for i in range(0,len(bm.verts)-1):
        try:
            bm.verts[i].co.x = table[rowcount]['X']
            bm.verts[i].co.y = table[rowcount]['Y']
            bm.verts[i].co.z = table[rowcount]['Z']
            rowcount += 1
            bmesh.update_edit_mesh(obj.data)
        except:
            pass
            
### 7.1.4 示例：动画多个时间步骤

对于数据可能是变化的3D时间序列的模拟，在每个时间步之间插入关键帧可以为数据设置动画。
还可以通过Blender API控制关键帧插入和动画播放工具栏。此示例读入一系列格式化的ASCII文本文件，将前两个代码块放在一起。

    import bpy
    import bmesh
    import csv
    import glob
    
    """ This line will change depending on whether Linux, Mac OS,or Windows is used"""
    
    files = glob.glob(mydirectory + '/*')
    
    for f in files:
        # Switch to edit mode
        bpy.ops.object.mode_set(mode = 'EDIT')
        print(Frame: + str(f))
        bpy.context.scene.frame_set(framecount)
        fields = ['Name','X','Y','Z']
        csvin = csv.reader(open(f))
        data = [row for row in csvin]
        header = data.pop(0)
        table = []
    
    for row in data:
        datarow = map(float,row[0].split())
        rowdict = dict(zip(fields,datarow))
        table.append(rowdict)
    
    obj = bpy.data.objects['Cube']
    mesh = obj.data
    bm = bmesh.from_edit_mesh(obj.data)
    rowcount = 0
    
    for i in range(0,len(bm.verts)-1):
        try:
            bm.verts[i].co.x = table[rowcount]['X']
            bm.verts[i].co.y = table[rowcount]['Y']
            bm.verts[i].co.z = table[rowcount]['Z']
            rowcount += 1
            bmesh.updata_edit_mesh(obj.data)
        except:
            pass
    bpy.ops.anim.keyframe_insert(type = 'Location')            
