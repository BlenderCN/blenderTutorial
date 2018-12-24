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
    collection["MySeting"] = {"foo": 10, "bar": "spam", "baz": {}}
    
    del collection["MySettings"]


<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/README.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/APIOverviewamorecompleteexplanationofPythonintegration.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
