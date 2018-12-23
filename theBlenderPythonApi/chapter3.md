# bmesh 模块

到目前为止，我们已经讨论了创建，管理和装换整个对象的方法。Blender的默认模式是对象模式，
它允许我们选择和操作一个或多个对象，通常使用可以适当应用于不同对象组的转换，例如旋转和平移。

当我们进入编辑模式时，Blender开始成为3D艺术套件。此模式允许我们选择单个对象的一个或多个顶点来执行高级和详细的转换。
正如人们所预料的那样，大多数用于编辑模式的操作无法在对象模式下执行，反之亦然。

bmesh模块几乎专门处理编辑模式操作。因此，在深入研究bmesh的功能之前，我们将对对象模式和编辑模式之间的差异进行适当的处理。

## 编辑模式

要像传统的Blender 3D艺术家一样手动进入编辑模式，请转到3D Viewport>Interaction Mode Menu>Edit Mode,如图3-1所示。
使用相同的菜单切换回对象模式。

图3-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-1.png?raw=true)

切换到编辑模式时，此时激活的对象将是用户可以编辑该编辑模式会话的唯一对象。如果用户想要在编辑模式操作不同的对象，
则必须先切换回对象模式以激活所需的对象。只有这样，在切换回编辑模式并激活所需对象后，他才能操作它。如果此时不清楚有关选择和激活的措辞，
请参阅第二章中的选择，激活和规范部分。请记住，我们始终可以在交互控制台中运行bpy.context.object来检查激活对象的名称。

要以编程方式在对象模式和编辑模式之间切换，请使用清单3-1中的两个命令。

清单3-1。在对象和编辑模式之间切换。

    # Set mode to Edit Mode
    bpy.ops.object.mode_set(mode="EDIT")
    
    # Set mode to Object Mode
    bpy.ops.object.mode_set(mode="OBJECT")
    
## 选择顶点，边和平面

要开始选择特定的操作单个对象的细节，我们必须能够选择特定的部分。我们将在ut.py模块中包含模式设置函数，
然后讨论如何使用bmesh来选择对象的特定部分。在这样做的过程中，我们将在Blender中解决bmesh和顶点索引协议的一些怪癖和版本兼容性缺陷。

### 在编辑和对象模式之间切换一致

清单3-2实现了一个包装函数，用于在对象模式和编辑模式之间切换。我们将在第2章开始构建的ut.py工具包中插入它。
我们对vanilla bpy.ops方法进行的唯一修改是在进入编辑模式时取消选择活动对象的所有顶点，边和平面。
目前，Blender用于确定在编辑模式下进入时选择对象的哪些部分的协议是不透明和笨重的。
每当我们进入编辑模式时，我们将采取最安全和最一致的方法，取消选择对象的每个部分。

当我们从编辑模式进入对象模式时，Blender只需从我们第一次进入编辑模式时恢复活动和选择的对象。这种行为是可靠且易懂的，
因此我们不会修改bpy.ops.object.mode_set(mod="OBJECT")的标准行为。

清单3-2。用于在对象和编辑模式之间切换的包装函数

    # Place in ut.py
    
    # Function for entering Edit Mode with no vertices selected,
    # or entering Object Mode with no additional processes
    
    def mode(mode_name):
        bpy.ops.objectt.mode_set(mode=mode_name)
        if mode_name == "EDIT":
            bpy.ops.mesh.select_all(action="DESELECT")
            
-----
Note 如果您在同一个Blender会话中多次编辑自定义模块如ut.py，请确保在模块上调用import.reload(ut)以查看将未缓存的版本导入Blender。
有关示例，请参见清单3-3。

-----

清单3-3.编辑自定义模块，在Blender会话中存在。

    # Will use the cached version of ut.py from 
    # your first import of the Blender session
    import ut 
    ut.create.cube('myCube')
    
    # Will reload the module from  the live script of ut.py
    # and create  a new cached version for the session 
    import importlib
    importlib.reload(ut)
    ut.create.cube('myCube')
    
    # This is what the header of your main script 
    # should look like when editing custom modules
    import ut
    import importlib
    importlib.reload(ut)
    
    # Code using ut.py ...
    
### 实例化bmesh对象

在Blender中，与其他核心数据结构相比，bmesh对象相当笨重，计算成本也很高。为了保持效率，
Blender为用户提供了大量数据和实例管理工作，以便通过API进行管理。在我们探索bmesh时，我们将继续看到这方面的示例。
有关实例化bmesh对象的示例，请参见3-4。通常，实例化bmesh对象需要我们在编辑模式下将bpy.meshes数据块传递给bmesh.from_edit_mesh()。

清单3-4。实例化bmesh对象

    import bpy
    import bmesh
    
    # Must start in object mode
    # Script will fail if scene is empty
    bpy.ops.object.mode_set(mode="OBJECT")
    bpy.ops.object.select_all(action="SELECT")
    bpy.ops.object.delete()
    
    # Create a cube and enter Edit Mode
    bpy.ops.mesh.primitive_cube_add(radius=1,location=(0,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    
    # Store a reference to the mesh datablock
    mesh_datablock = bpy.context.object.data
    
    # Create the bmesh object(named bm) to operate on 
    bm = bmesh.from_edit_mesh(mesh_datablock)
    
    # Print the bmesh object
    print(bm)
    
如果我们尝试在 交互式控制台中运行这些命令，我们可能会得到不同的结果。bmesh对象的实例不是持久的。
除非Blender检测到它正在被主动使用，否则bmesh对象将取消引用网格数据块，垃圾收集内部数据，
并返回<BMesh dead at some_memory_address>。考虑到维护bmesh对象所需的空间和计算能力，这是一种理想的行为，
但它确实需要程序员执行额外的命令以使其保持活动状态。在构建用于选择3D对象的特定部分的函数时，我们将遇到这些命令。
    
### 选择3D对象的部分

要选择部分bmesh对象，我们操纵每个BMesh.verts,BMesh.edges和BMesh.faces对象的选择布尔值。清单3-5给出了选择立方体部分的示例。

请注意清单3-5中对ensure_lookup_table()的大量调用。我们使用这些函数来提醒Blender在操作直接保持BMesh对象的某些部分不被垃圾收集。
这些功能占用的处理能力极低，因此我们可以轻松地调用它们而不会产生太大后果。因为调试此错误，最好是over-call它们而不是under-call它们。

ReferenceError:已删除BMesh类型的BMesh数据。

可以在大型代码库中使用keep_lookup_table()没有协议的噩梦。

清单3-5。选择3D对象的部分。

    import bpy
    import bmesh
    
    # Must start in object mode
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create a cube and enter Edit Mode
    bpy.ops.mesh.primitive_cube_add(radius=1,location=(0,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    
    # Set to "Face Mode" for easier visualization
    bpy.ops.mesh.select_mode(type="FACE")
    
    # Register bmesh object and select various parts
    bm = bmesh.from_edit_mesh(bpy.context.object.data)
    
    # Deselect all verts,edges,faces
    bpy.ops.mesh.select_all(action='DESELECT")
    
    # Select a face
    bm.faces.ensure_lookup_table()
    bm.faces[0].select = True
    
    # Select an edge
    bm.edges.ensure_lookup_table()
    bm.edges[7].select = True
    
    # Select a vertex
    bm.verts.ensure_lookup_table()
    bm.verts[5].select = True
    
读者会注意到我们运行了bpy.ops.mesh.select_mode(type="FACE")。到目前为止还没有涵盖这个概念，但要正确使用高级编辑模式功能，
这一点很重要 ，通常，Blender艺术家会单击3D视窗标题中的三个选项之一，如图3-2所示。
图3-2中的按钮对应于bpy.ops.mesh.select_mode()中的VERT,EDGE和FACE参数。现在，这只会影响我们在编辑模式下可选择的方式。
我们为此示例选择FACE，因为它是同时可视化所有三种类型的最佳模式。在本章的后面，我们将讨论编辑模式中的一些功能，
其行为将根据此选择而改变。

图3-2

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-2.png?raw=true)

## 编辑模式转换

本节讨论简单转换，如编辑模式下的转换和旋转，以及随机化，拉伸和细分等高级转换。

### 基础转换

方便的是，我们可以使用第2章中用于对象模式转换的相同功能来操作3D对象的各个部分。
我们将使用清单2-9中介绍的bpy.ops子模块给出一些示例清单3-6。有关轻微变形立方体的输出，请参见图3-3。

清单3-6。编辑模式的基础转换。

    import bpy
    import bmesh
    
    # Must start in object mode
    bpy.ops.object.mode_set(mode='OBJECT‘)
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create a cube and rotate a face around the y-axis
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(-3,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    # Set to face mode for transformations
    bpy.ops.mesh.select_mode(type='FACE')
    
    bm = bmesh.from_edit_mesh(bpy.context.object.data)
    bm.faces.ensure_lookup_table()
    bm.face[1].select = True
    bpy.ops.transform.rotate(value=0.3,axis=(0,1,0))
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # Create a cube and pull an edge along the y-axis
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(0,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    bm = bmesh.from_edit_mesh(bpy.context.object.data)
    bm.edges.ensure_lookup_table()
    bm.edges[4].select = True
    bpy.ops.transform.translate(value=(0,0.5,0))
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # Create a cube and pull a vertex 1 unit
    # along the y and z axes
    # Create a cube and pull an edge along the y-axis
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=3,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    bm = bmesh.from_edit_mesh(bpy.context.object.data)
    bm.verts.ensure_lookup_table()
    bm.verts[3].select = True
    bpy.ops.transform.translate(value=(0,1,1))
    
    bpy.ops.object.mode_set(mode='OBJECT')

图3-3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-3.png?raw=true)    

### 高级转换

我们不能希望覆盖Blender中所包含的所有工具来编辑网格，因此我们将在本节中介绍一些工具，并在本章末尾使用示例清除更多内容。
清单3-7实现了挤出，细分和随机化运算符。有关预期输出，请参见图3-4。

清单3-7。挤出，细分和随机化运算符

    import bpy
    import bmesh
    
    # Will fail if scene is empty
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create a cube and extrude the top face away from it
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(-3,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    # Set to face mode for transformations
    bpy.ops.mesh.select_mode(type='FACE')
    
    bm = bmesh.from_edit_mesh(bpy.context.object.data)
    bm.faces.ensure_lookup_table()
    bm.faces[5].select = True
    bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate = {"vlaue":(0.3,0.3,0.3),
                                                                "constraint_axis":(True,True,True),
                                                                "constraint_orientation":'NORMAL'})
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # Create a cube and subdivide the top face
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(0,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    bm = bmesh.from_edit_mesh(bpy.context.object.data)
    bm.faces.ensure_lookup_table()
    bm.faces[5].select = True
    bpy.ops.mesh.subdivide(number_cuts=1)
    
    bpy.ops.mesh.select_all(action='DESELECT')
    bm.faces.ensure_lookup_table()
    bm.faces[5].select = True
    bm.faces[7].select = True
    bpy.ops.ops.transform.translate(value = (0,0,0.5))
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
    # Create a cube and add a random offset to each vertex
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(3,0,0))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='SELECT')
    bpy.ops.transform.vertex_random(offset=0.5)
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
图3-4

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-4.png?raw=true)

## 关于索引和交叉兼容性的注意事项

读者可能已经注意到3D对象中的顶点，边和面的索引没有按特定顺序排列。到目前为止示例脚本中，
作者已经提前手动定位索引而不是以编程方式发现它们。例如，在操作清单3-7中多维数据集的顶部时，
作者事先确定ut.act.select_face(bm,5)将选择多维数据集顶部的面。这是通过试错法测试确定。

使用试错法测试来发现对象的一部分的索引号是一种可接受的做法，但是存在许多缺点。
在任何给定版本的Blender中，索引语义应该被认为是可复制的但是不可篡改的。
    
*   对象的默认索引在不同版本的Blender中变化很大。作者已经注意到依赖于不同版本的Blender的硬编码索引的插件中的主要兼容性问题。
在依赖于硬编码索引的插件中，版本2.77和版本2.78之间存在重大差异。 

*   某些转换后索引的行为非常难以处理。有关默认平面的顶点索引，三个插入后的平面以及两个细分后的平面的示例，请参见图3-5。
这些平面中的指数不符合特定的逻辑模式。转换之间的差异是跨版本不兼容的另一个原因。

*   使用硬编码索引的插件在用户交互的可能性方面非常有限。使用硬编码索引的插件可以连续运行，但是如果能够与用户进行来回交互则很少。

此问题的解决方法是按特征选择。要按特征选择顶点，我们遍历对象中的每个顶点，并在符合条件的顶点上运行bm.verts[i].select = True。
边缘和面也一样。从理论上讲，这种方法在计算上看起来非常昂贵并且在算法上非常复杂，但您会发现它的速度非常快且模块化。
使用按特性进行纯选择的插件通常可以在多个版本的Blender上同时成功运行。不幸的是，
实现这一点在Blender中开辟了一个关于局部和全局坐标系的蠕虫概念。我们将在下一节中详细说明。

图3-5

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-5.png?raw=true)

## 全局和局部坐标

Blender为每个对象的每个对象的每个部分存储许多坐标数据集。在大多数情况下，我们只关注两组坐标：全局坐标G和局部坐标L。
当我们对对象执行变换时，Blender将这些变换存储为变换矩阵T的一部分。Blender在某些时候，将变换矩阵应用于局部坐标。
在Blender应用变换矩阵之后，局部坐标将等于全局坐标，并且变换矩阵将是单位矩阵。

在3D视窗中，我们始终查看全局坐标G=T*L。

我们可以控制Blender何时使用bpy.ops.object.transform_apply()应用转换。这不会改变对象的外观，
而是将L设置为等于G并将T设置为等于标识。

我们可以利用它来轻松选择对象的特定部分。
如果我们通过不运行bpy.ops.object.transform_apply()并且不退出编辑模式来延迟执行bpy.ops.object.transform_apply(),
我们可以维护两个数据集G和L。实际上，G对于相对于其他人定位对象非常有用，并且L很容易循环来获取索引。

有关访问对象的全局和局部坐标的函数，请参见清单3-8。给定bpy.data.meshes[].topics数据块为v，
v.co给出局部坐标和bpy.data.objec[].matrix_world*v.co给出全局坐标。值得庆幸的是，
可以在对象模式和编辑模式下访问此数据块。我们将构建与模式无关的函数来访问这些坐标。
有关独立于模式获取每组坐标的函数，请参见清单3-8。

这些功能牺牲了一些清晰度以换取简洁和效率。在此代码中，v是表示矩阵L的元组列表，
obj.matrix_world是表示我们的变换矩阵T的Python矩阵。

# 清单3-8。获取全局和本地坐标

    def coords(objName,space='GLOBAL'):
        
        # Store reference to the bpy.data.objects datablock
        obj = bpy.data.objects[objName]
        
        # Store referencce  to bpy.data.objects[].meshes datablock
        if obj.mode = 'EDIT':
            v = bmesh.from_edit_mesh(obj.data).verts
        elif obj.mode == 'OBJECT':
            v = obj.data.vertices
        
        if space == 'GLOBAL':
            # Return T*L as list of tuples
            return [(obj.matrix_world*v.co).to_tuple() for v in v]
        elif space == 'LOCAL':
            # Return L as list of tuples
            return [v.co.to_tuple() for v in v]
            
    class sel:
        
        # Add this to the ut.sel class,for use in object mode
        def transform_apply():
            bpy.ops.object.transform_apply(location=True,rotation=True,scale=True)
            
有关本地和全局坐标行为的示例，请参阅清单3-9。我们在转换之前，
转换之后立即以及在transform_apply()之后打印立方体的前两个坐标三元组。这在纸上和代码编辑器中都很有意义。
逐行在交互式控制台中运行清单3-9突出显示了transfrom_apply()的有趣行为。转换立方体后，
读者将看到立方体移动，但局部坐标将保持不变。运行transform_apply()后，立方体将不会移动，但本地坐标将更新以匹配全局坐标。

清单3-9。全局和局部坐标的行为及变换应用

    import ut
    import importlib
    importlib.reload(ut)
    
    import bpy
    
    # Will fail if scene is empty
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(0,0,0))
    bpy.context.object.name = 'Cube-1'
    
    # Check global and local coordinates
    print('\nBefore transform:')
    print('Global:',ut.coords('Cube-1','GLOBAL')[0:2])
    print('Local:',ut.coords('Cube-1','LOCAL')[0:2])
    
    # Translate it along x=y=z
    # See the cube move in the 3D viewport
    bpy.ops.transform.translate(value=(3,3,3))
    
    # Check global and local coordinates
    print('\nAfter transform,unapplied:')
    print('Global:',ut.coords('Cube-1','GLOBAL')[0:2])
    print('Local:',ut.coords('Cube-1','LOCAL')[0:2]
    
    # Apply transformation
    # Nothing changes in 3D viewport
    ut.sel.tansform_apply()
    
    # Check global and local coordinates
    print('\nAfter transform,applied:')
    print('Global:',ut.coords('Cube-a','GLOBAL')[0:2])
    print('Local:',ut.coords('Cube-1','LOCAL')[0:2])
    
    #############################Output###################
    # Before transform:
    # Global:[(-0.5,-0.5,-0.5),(-0.5,-0.5,0.5)]
    # Local:[(-0.5,-0.5,-0.5),(-0.5,-0.5,0.5)]
    #
    # After transform,applied:
    # Global: [(2.5,2.5,2.5),(2.5,2.5,3.5)]
    # Local: [(-0.5,-0.5,-0.5),(-0.5,-0.5,0.5)]
    #
    # After transform applied:
    # Global : [(2.5,2.5,2.5),(2.5,2.5,3.5)]
    # Local:[(2.5,2.5,2.5),(2.5,2.5,3.5)]
    #####################################################
    
在下一节中，我们使用此概念来解决图3-5中显示的问题，并在Blender中解锁编辑模式的全部功能。    

## 按位置选择顶点，边和面

有关两个函数的共同作用，有助于在全局和局部坐标系中按位置选择顶点，边和面，请参见清单3-10。
我们指定为ut.act.select_by_loc()的函数看起来非常复杂，但不使用我们迄今尚未介绍的任何Blender概念。
作者认为此功能应作为bmesh模块的一部分包括在内，因为它广泛适用。

清单3-10。按位置选择对象的功能

    # Add in body of script,outside any class declarations
    def in_bbox(lbound,ubound,v,buffer=0.0001):
        return lbound[0] - buffer <= v[0] <= ubound[0] + buffer and \
            lbound[1] - buffer <= v[1] <= ubound[1] + buffer and \
            lbound[2] - buffer <= v[2] <= ubound[2] + buffer
            
    class act:
    
        # Add to ut.act class
        def select_by_loc(lbound=(0,0,0),ubound=(0,0,0),
                            select_mode='VERT',coords='GLOBAL'):
            # Set selection mode,VERT,EDGE,or FACE
            selection_mode(select_mode)
            
            # Grab the transformation matrix
            world = bpy.context.object.matrix_world
            
            # Instantiate a bmesh object and ensure lookup table
            # Running bm.faces.ensure_lookup_table() works for all parts
            bm = bmesh.from_edit_mesh(bpy.context.object.data)
            bm.faces.ensure_lookup_table()
            
            # Initialize list of vertices and list of parts to be selected
            verts = []
            to_select = []
            
            # For VERT,EDGE,or FACE ...
            # 1.Grab list of global or local coordinates
            # 2.Test if the piece is entirely within the rectangular
            #   prism defined by  lbound and ubound
            # 3.Select each piece that returned True and deselect
            #   each piece that returned False in Step 2
            
            if select_mode == 'VERT':
                if coords == 'GLOBAL':
                    [verts.append((world*v.co).to_tuple()) for v in bm.verts]
                elif coords ==  'LOCAL':
                    [verts.append(v.co.to_tuple()) for v in bm.verts]
                
                [to_select.append(in_bbox(lbound,ubound,v)) for v in verts]
                for vertObj,select in zip(bm.verts,to_select):
                    vertObj.select = select
            
            if select_mode == 'EDGE':
                if coords ==  'GLOBAL':
                    [verts.append([world*v.co).to_tuple() for v in e.verts]) for e in bm.edges]
                elif coords == 'LOCAL':
                    [verts.append([v.co.to_tuple() for v in e.verts]) for e in bm.edges]
                
            [to_select.append(all(in_bbox(lbound,ubound,v) for v in e)) for e in verts]
            
            for edgeObj,select in zip(bm.edges,to_select):
                edgeObj.select = select

        if select_mode == 'FACE':
            if coords == 'GLOBAL':
                [verts.append([world*v.co).to_tuple() for v in f.verts]) for f in bm.faces]
            elif coords == 'LOCAL':
                [verts.append([v.co.to_tuple() for v in verts]) for f in bm.faces]
                  
            [to_select.append(all(in_bbox(lbound,ubound,v) for v in f)) for f in verts]
            for faceObj,select in zip(bm.faces,to_select):
                faceObj.select = select
                
清单3-11给出了一个使用ut.act.select_by_loc()来选择球体片段并对其进行转换的示例。请记住，
此函数的前两个参数是3D中矩形棱镜的最低角和最高角。如果整个部件（顶点，边缘，面）落入矩形棱镜内，则将选择它。

清单3-11。选择和变换球体的碎片

    import ut
    import importlib
    importlib.reload(ut)
    
    import bpy
    
    # Will fail if scene is empty
    bpy.ops.object.mode_set(mode='OBJECT')
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    bpy.ops.mesh.primitive_uv_sphere_add(size=0.5,location=(0,0,0))
    bpy.ops.transform.resize(value=(5,5,5))
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    # Selects upper right quadrant of sphere
    ut.act.select_by_loc((0,0,0),(1,1,1),'VERT','LOCAL')
    
    # Selects nothing
    ut.act.select_by_loc((0,0,0),(1,1,1),'VERT','GLOBAL')
    
    # Select upper right quadrant of sphere
    ut.act.select_by_loc((0,0,0),(5,5,5),'VERT','LOCAL')
    
    # Mess with it
    bpy.ops.transform.translate(value=(1,1,1))
    bpy.ops.transform.resize(value=(2,2,2))
    
    # Selects lower half of sphere
    ut.act.select_by_loc((-5,-5,-5),(5,5,-0.5),'EDGE','GLOBAL')
    
    # Mess with it
    bpy.ops.transform.translate(value=(0,0,3))
    bpy.ops.transform.resize(value=(0.1,0.1,0.1))
    
    bpy.ops.object.mode_set(mode='OBJECT')
    
## 检查点和例子

到目前为止，我们已经为ut.py做了很多补充。有关我们迄今为止在本书中所做的所有新增内容的最新版本，
请访问blender.chrisconlan.com/ut_ch03.py。
    
鉴于此版本的ut.py,我们将尝试一些有趣的示例。有关随机形状增长算法，请参见清单3-12。
随机(和粗略地）的简短算法选择对象所在的空间块，然后沿着所选表面的垂直法线挤出所选部分。
要沿着曲面的垂直法线进行挤出，我们只需运行ut.act.extrude((0,0,1)),因为此函数默认使用曲面的局部方向。

该算法让我们可以构建优雅和古怪的形状。结果的类型主要取决于我们在脚本顶部附近的ut.create调用中提供的形状。
有关立方体和球体的清单3-12的示例，请参见图3-6和3-7。

清单3-12。随机i形状增长。

    import ut
    import importlib
    importlib.reload(ut)
    
    import bpy
    
    from random import randint
    from math import floor
    
    # Must start in object mode
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create a  cube
    bpy.ops.mesh.primitive_cube_add(radius=0.5,location=(0,0,0))
    bpy.context.object.name = 'Cube-1'
    
    bpy.ops.object.mode_set(mode='EDIT')
    bpy.ops.mesh.select_all(action='DESELECT')
    
    for i in range(0,100):
        
        # Grab the local coordinates
        coords = ut.coords('Cube-1','LOCAL')
        
        # Find the bounding box for the object
        lower_bbox = [floor(min(v[i] for v in coords])) for i in [0,1,2]]
        upper_bbox = [floor(max(v[i] for v in coords])) for i in [0,1,2]]
        
        # Select a random face 2x2x1 units wide,snapped to integer coordinates
        lower_sel = [randint(l,u) for l,u in zip(lower_bbox,upper_bbox)]
        upper_sel = [1+2 for l in lower_sel]
        upper_sel[randint(0,2)] -= 1
        
        ut.act.select_by_loc(lower_sel,upper_sel,'FACE','LOCAL')
        
        # Extrude the surface along it aggregate vertical normal
        bpy.ops.mesh.extrude_region_move(TRANSFORM_OT_translate={'value':(0,0,1),
                                            "constraint_axis":(True,True,True),
                                            "constraint_orientation": 'NORMAL'})
 
图3-6

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-6.png?raw=true)

图3-7

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-7.png?raw=true)

虽然这些示例看似微不足道，但它们说明了在Blender中自动执行编辑模式操作的强大功能。虽然清单3-12中的简短算法可以制作迷人的形状，
但是在给定正确的领域特定知识的情况下，其中的概念可用于在Blender中创建整个CAD系统。很好的例子包括：

*   商业建筑的模型

*   数学曲面的模型

*   原子和化学模型

所有这些都可以都可以通过本章讨论的概念来实现。就目前而言，我们的工具包并非特定于案例。在很多方面可以对其进行改进，
以适应不同学科和应用程序的建模需求。定制和改进我们的工具包的显着方法包括：

*   创建支持矩形棱柱以外的选区的ut.act.select_by_loc()函数。
有可能用于圆柱形，球形，二维和一维选择表面。

*   为它们创建额外的ut.create函数和特定于案例的自动命名模式。

*   以与添加ut.act.extrude和ut.act.subdivide相同的方式向ut.act添加其他编辑模式操作。有充分的机会探索并进一步参数化这些函数。

将LOCAL，NORMAL和GIMAL轴操作添加到ut.set。到目前为止，我们一直使用默认的GLOBAL。例如，平移，旋转和缩放都可以沿这些轴执行。

## 结论

在接下来的章节中，我们将讨论Blender中有效的插件开发所需的基本渲染概念。

                        
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter2.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/theBlenderPythonApi/chapter4.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>    
    
  
