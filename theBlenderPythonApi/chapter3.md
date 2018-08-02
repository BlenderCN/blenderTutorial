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
    
    
    


    

  
