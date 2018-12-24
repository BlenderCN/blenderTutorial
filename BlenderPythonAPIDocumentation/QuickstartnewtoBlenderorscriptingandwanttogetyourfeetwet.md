## Preface

这种API一般稳定，但一些地区仍在进行补充和完善

在Blender/Python API可以执行以下操作：

*   任何编辑数据的用户接口可以（Scenes,Meshes,Particles等）。

*   修改用户偏好，键映射和主题

*   工具的设置与运行

*   创建用户接口元素，诸如菜单、标题和面板

*   创建新的工具

*   创建交互式工具

*   创建的渲染引擎，结合Blender。

*   在现有Blender数据中自定义新设置

*   从Python使用OpenGL命令绘制的3D视图

Blender/Python API还不能...

*   创建新的空间类型

*   为每种类型分配自定义属性

*   定义在数据更改时要通知的回调或侦听器

## Before Starting

本文件并不旨在完全涵盖每个主题。相反，其目的是让你熟悉Blender Python API

在开始之前要了解的有用事项的快速列表：

*   Blender使用Python3.x；一些在线文档仍然假设2.x。

*   交互式控制台非常适合测试单行程序。它还具有自动完成功能，因此你可以快速检查API。

*   按钮工具提示显示Python属性和operator名称。

*   右键单击按钮和和菜单项可直接链接到API文档。

*   有关更多示例，本文菜单有一个模板部分，其中可以找到一些示例运算符。

*   要检查使用Blender分发的其他脚本，请参阅：

scripts/startup/bl_ui用于用户界面。

scripts/startup/bl_operators用于operators。

确切位置取决于平台，请参阅配置和数据路径。

### Running Scripts

执行Python脚本的两种最常用方法是使用内置文本编辑器或在Python控制台中输入命令。

Text Editor和Python Console都是可以从视图标题中选择的空间类型。

你可能更喜欢使用Scripting屏幕（Blender默认情况下包含），可从顶部标题屏幕选择器访问，而不是手动配置Python开发空间。

从文本编辑器中，你可以打开.py文件或从粘贴板粘贴，然后使用Run Script进行测试。

Python控制台通常用于输入代码段和测试以获得即时反馈，但也可以将整个脚本粘贴到其中。

脚本也可以使用Blender从命令行运行，但要学习Blender/Python，这不是必需的。

## Key Concepts

### Data ACCESS

### Accessing DataBlocks

Python以与动画系统和用户界面相同的方式访问Blender的数据；这意味着任何可以通过按钮更改的设置也可以从Python更改。

使用模块bpy.data访问当前加载的blend文件中的数据。这样可以访问库数据。例如：

    >>>bpy.data.objects
    <bpy_collection[3], BlendDataObjects>
    
    >>bpy.data.scenes
    <bpy_collection[1], BlendDataScenes>
    
    >>>bpy.data.materials
    <bpy_collection[1], BlendDataMaterials>
    
### About Collections    

你会注意到索引和字符串可用于访问集合的成员。

与Python的词典不同，这两种方法都是可以接受的；但是，运行Blender时，成员的索引可能会发生变化。

    >>>list(bpy.data.objects)
    [bpy.data.objects['Camera'], bpy.data.objects['Cube'], bpy.data.objects['Light']]
    
    >>>bpy.data.objects['Cube']
    bpy.data.objects["Cube"]
    
    >>>bpy.data.objects[0]
    bpy.data.objects["Cube"]
    
### Accessing Attributes

一旦有了数据块，例如材质，对象，集合等，就可以像使用图形基面更改设置一样访问其属性。事实上，每个按钮的工具提示也会显示Python属性，这有助于查找脚本中要更改的设置。

    >>>bpy.data.objects[0].name
    'Camera'
    
    >>>bpy.data.scenes["Scene"]
    bpy.data.scenes['Scene']
    
    >>>bpy.data.materials.new("MyMaterial")
    bpy.data.materials['MyMaterial']
    
为了测试访问它的数据，使用Console很有用，它是自己的空间类型。这支持自动完成，为你提供了一种快速浏览文件中不同的数据方法。

可以通过控制台快速找到的数据路径示例：

    >>>bpy.data.scenes[0].render.resolution_percentage
    100
    >>>bpy.data.scenes[0].objects["Cube"].data.vertices[0].co.x
    1.0

### Data Creation/Removal

熟悉其他Python API的人可能会惊讶于bpy API中的新数据块不能通过调用该类创建。

    >>>bpy.types.Mesh()
    Traceback (most recent call last):
        File "<blender_console>", line 1, in <module>
    TypeError: bpy_struct.__new__(type): expected a single argument

这是API设计的一部分。Blender/Python API无法创建存在于主Blender数据库之外（通过[bpy.data](https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/DataAccessbpydata.md)访问）的Blender数据，因为此数据由Blender管理(/save/load/undo/append...等）。

通过bpy.data中的集合上的方法添加和删除数据，例如：

    >>>mesh = bpy.data.meshes.new(name="MyMesh")
    >>>print(mesh)
    <bpy_struct, Mesh("MyMesh")>
    
    >>>bpy.data.meshes.remove(mesh)
    
### Custom Properties    

Python可以访问具有ID的任何数据块的属性（可以在[bpy.data](https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/DataAccessbpydata.md)中链接和访问的数据。在分配属性时，你可以组成自己的名称，这些名称将在需要时创建或覆盖（如果存在）。

该数据保存在Blend文件中，并与对象一起复制。

例如：

    bpy.context.object["MyOwnProperty"] = 42
    
    if "SomeProp" in bpy.context.object:
        print("Property found")

    # Use the get function like a Python dictionary
    # which can have a fallback value.    
    value = bpy.data.scenes["Scene"].get("test_prop","fallback value")
    
    # dictionaries can be assigned as long as they only use basic types.
    
    collection = bpy.data.collections.new("MyTestCollection")
    collection["MySetting"] = {"foo": 10, "bar": "spam", "baz": {}}
    
    del collection["MySettings"]

注意，这些属性只能分配基本的Python类型。

*   int，float,string

*   array of ints/floats

*   dictionary(只支持字符串，值也必须是基本类型)

这些属性在Python之外是有效的。它们可以通过曲线动画或用于驱动程序路径。

### Context

虽然能够通过名称或列表直接访问数据很有用，但是对用户的选择进行操作更常见。内容总是可以从bpy.context获得，可用于获取活动对象，场景，工具设置以及许多其他属性

常见用例：

    >>>bpy.context.object
    >>>bpy.context.selected_objects
    >>bpy.context.visible_bones
    
注意，context是只读的。不能直接修改这些值，但是可以通过运行API函数或使用data API来更改它们。    

所以bpy.context.object = obj将会引发错误。

但是bpy.context.scene.objects.active = obj将会按预期工作。  ?????

context属性根据访问它们的位置而变化。3D视图的context成员与console不同，因此在访问用户状态已知的context属性时要小心

查看[bpy.context](https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/ContextAccessbpycontext.md) API参考。

### Operators(Tools)

Operators通常是用户通过按钮，菜单项或键快捷键访问的工具。从用户的角度来看，它们是一个工具，但是Python可以通过bpy.ops模块使用自己的设置来运行它们。

例如：

    >>>bpy.ops.mesh.flip_normals()
    {'FINISHED'}
    >>>bpy.ops.mesh.hide(unselected=False)
    {'FINISHED'}
    >>>bpy.ops.object.scale_apply()
    {'FINISHED'}
    
！Note——菜单项：Help>Operator Cheat Sheet提供了Python中所有operators及默认值的列表，以及生成的文档。这是了解所有Blender的operators概况的好方法。

### Operator Poll()

许多operators都有poll函数，可以检查光标是否位于有效区域，或者对象是否处于正确模式（编辑模式、绘制权重等）。当operator的poll
函数在Python中失败时，会引发异常。

例如，从控制台调用bpy.ops.view3d.render_border()会引发以下错误：

    RuntimeError: Operator bpy.ops.view3d.render_border.poll() failed, context is incorrect
    
在这种情况下，context必须是带有活动摄像机的3d视图。

为了避免在调用运算符的地方使用try/except子句，可以调用operators自己的poll()函数来检查它是否可以在当前context中运行。

    if bpy.ops.view3d.render_border.poll():
        bpy.ops.view3d.render_border()
        
### Integration

python脚本可以通过以下方式与Blender集成：

*   通过定义渲染引擎。

*   通过定义operators

*   通过定义菜单，标题和面板。

*   通过在现有菜单，标题和面板中插入新按钮。

在Python中，这是通过定义一个类来完成的，该类是现有类型的子类。

### Example Operator

    import bpy
    
    def main(context):
        for ob in context.scene.objects:
            print(ob)
            
    class SimpleOperator(bpy.types.Operator):
        """Tooltip"""
        bl_idname = "object.simple_operator"
        bl_label = "Simple Object Operator"
        
        @classmethod
        def poll(cls,context):
            return context.active_object is not None
        
        def execute(self,context):
            main(context)
            return {'FINISHED'}
            
    def register():
        bpy.utils.register_class(SimpleOperator)
        
    def unregister():
        bpy.utils.unregister_class(SimpleOperator)
        
    if __name__ == "__main__":
        register()
        
        # test call
        bpy.ops.object.simple_operator()
        
运行此脚本后，SimpleOperator将在Blender中注册，可以从operator搜索弹出窗口中调用或添加到工具栏中。

要运行脚本：

1. 突出显示上面的代码，然后按Ctrl-C复制它。

2. 启动Blender。

3. 按Ctrl-Right两次以更改为Scripting布局。

4. 单击标记为新建的按钮，弹出确认以创建新的文本块。

5. 按Ctrl-V将代码粘贴到文本面板（左上方框架）。

6. 单击“运行脚本”按钮。

7. 将光标移动到3D视图，按空格键选择operator搜索菜单，然后键入Simple。

8. 单击搜索中的Simple Operator项。

!See also——具有bl_前缀的类成员记录在API参考bpy.types.Operator中。
    
！Note——主函数的输出发送到终端；为了看到这一点，一定要[使用终端](https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/TipsandTricksHintstohelpyouwhilewritingscriptsforBlender.md)。

### Example Panel

面板将自己注册为类，就像operator一样。注意额外的bl_变量用于设置它们显示的context。

    import bpy
    
    class HelloWorldPanel(bpy.types.Panel):
        """Creates a Panel in the Object properties window"""
        bl_label = "Hello World Panel"
        bl_idname = "OBJECT_PT_hello"
        bl_space_type = "PROPERTIES"
        bl_region_type = "WINDOW"
        bl_context = "object"
        
        def draw(self,context):
            layout = self.layout
            
            obj = context.object
            
            row = layout.row()
            row.label(text="Hello world!",icon="WORLD_DATA")
            
            row.layout.row()
            row.label(text="Active object is: " + obj.name)
            row = layout.row()
            row.prop(obj,"name")
            
            row = layout.row()
            row.operator("mesh.primitive_cube_add")
            
    def register():
        bpy.utils.register_class(HelloWorldPanel)
        
    def unregister():
        bpy.utils.unregister_class(HelloWorldPanel)
        
    if __name__ == "__main__":
        register()
        
要运行脚本：


1. 突出显示上面的代码，然后按Ctrl-C复制它。

2. 启动Blender。

3. 单击Scripting工作区的选项卡

4. 单击标记为新建的按钮以创建新文本块。

5. 按Ctrl-V将代码粘贴到文本面板（左上方框架）。

6. 单击Run Script按钮。
        
查看结果：

1. 选择默认立方体

2. 单击按钮面板中的对象属性图标（最右侧；显示为一个小立方体）。

3. 向下滚动以查看名为Hello World Panel的面板。

4. 更改对象名称还会更新Hello World Panel的名称：字段。

请注意行分布以及代码中可用的标签和属性。

！See also——[bpy.types.Panel](https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/Typesbpytypes.md)

### Types

Blender定义了许多Python类型，但也使用Python本地类型。

Blender的Python API可以分为3类。

### Native Types

在简单的情况下，将数字或字符串作为自定义类型返回会很麻烦，因此可以将它们作为普通的Python类型进行访问。

*   Blender float/int/boolean->float/int/boolean

*   Blender enumerator->string

    >>>C.object.rotation_mode = 'AXIS_ANGLE'
    
*   Blender enumerator(multiple)->set of strings

    # setting multiple camera overlay guides


<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/README.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/APIOverviewamorecompleteexplanationofPythonintegration.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
