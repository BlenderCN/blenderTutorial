按照python的说法一切皆对象，Blender中亦然。

Blender的根对象是bpy，因此任何模块都需要：
    
    import pby
    
### bpy    
然后，任何对象其实都是数据，因此访问blender的第一个关键对象是bpy.data。现在，我们打开python的命令行窗口，
拖动3D视图的右上角，向左下，扩展出一个窗口。选择左下角的立方体，选择python控制台，然后就可以输入python脚本了。
在其中输入

    >>bpy.data.objects
    <bpy_collection[4], BlendDataObjects>
    
bpy_collection是一个python的词典对象，显示有四个对象(窗口初始是三个对象，我加了一个）。想知道是哪几个对象？
很好办。用一个遍历，如下所示，列出了所有对象的名字：

    >>> for obj in bpy.data.objects:
    ...     print(obj.name)
    ...     
    Camera
    Cube
    Lamp
    融球
   
通过这种方法，我们可以操作Blender的几乎所有数据。

### bpy.context
bpy.context顾名思义就是运行的上下文环境，这个在脚本环境里到处碰到了。
    
    >>> bpy.context.object
    >>> bpy.context.selected_objects
    >>> bpy.context.visible_bones
这里可以看到，context也可以访问到object，这个是当前的环境可用的对象，其数据跟data里是一致的。后面的ops还可以来访问和修改对象的属性值。
这是典型的python式的”面向对象“，数据按照对象来组织，但是访问时却是以函数方式出现，从而适应脚本环境的运行需要。

### bpy.ops
bpy.ops是Operator的简写，封装了很多的对象操作在里面，是Blender功能的核心了。

    >>> bpy.ops.mesh.flip_normals()
    {'FINISHED'}

    >>> bpy.ops.mesh.hide(unselected=False)
    {'FINISHED'}

    >>> bpy.ops.object.scale_apply()
    {'FINISHED'}
    
一个操作器的完整例子：

    import bpy

    def main(context):
        for ob in context.scene.objects:
            print(ob)

    class SimpleOperator(bpy.types.Operator):
        """Tooltip"""
        bl_idname = "object.simple_operator"
        bl_label = "Simple Object Operator"

        @classmethod
        def poll(cls, context):
            return context.active_object is not None

        def execute(self, context):
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

注意里面的register/unregister，以及其中的class和poll、execute成员函数，这是一个典型的插件的实现结构。
大部分插件照着上面这个结构依瓢画葫芦就可以了。

### bpy.types
bpy.types不是python的type对象的简单封装，而是Blender各种”类“的集合，不要问我到底有啥区别，谁用谁知道啊。
Panel也是types里面的东西。下面是定义Panel的插件：

    import bpy
    class HelloWorldPanel(bpy.types.Panel):
        """Creates a Panel in the Object properties window"""
        bl_label = "Hello World Panel"
        bl_idname = "OBJECT_PT_hello"
        bl_space_type = 'PROPERTIES'
        bl_region_type = 'WINDOW'
        bl_context = "object"

        def draw(self, context):
            layout = self.layout

            obj = context.object

            row = layout.row()
            row.label(text="Hello world!", icon='WORLD_DATA')

            row = layout.row()
            row.label(text="Active object is: " + obj.name)
            row = layout.row()
            row.prop(obj, "name")

            row = layout.row()
            row.operator("mesh.primitive_cube_add")

    def register():
        bpy.utils.register_class(HelloWorldPanel)

    def unregister():
        bpy.utils.unregister_class(HelloWorldPanel)

    if __name__ == "__main__":
        register()
        
这里多了一些变量的声明，这是插件的一些说明。

    bl_label = "Hello World Panel"
    bl_idname = "OBJECT_PT_hello"
    bl_space_type = 'PROPERTIES'
    bl_region_type = 'WINDOW'
    bl_context = "object"

所有的插件原则上都应该包含上面这些信息，引号内的字符串可以修改，变量名不能修改（否则就成自己的了）。
这个插件信息声明了这个panel是在对象的属性面板中出现。

bpy.types包含的一些对象如下:

    bpy.types.Panel
    bpy.types.Menu
    bpy.types.Operator
    bpy.types.PropertyGroup
    bpy.types.KeyingSet
    bpy.types.RenderEngine

这里再次出现Operator对象，跟前面的bpy.ops是不是一样的啊？我猜测应该是，考虑到type是python的基本对象，
这个现象一点也不奇怪。要想一探究竟，很简单，到python控制台运行一下即可。

### 注意：

        Blender的脚本print("hello")函数是输出信息到启动的控制台的，直接运行的时候看不到输出的信息。

        可以扩展出一个窗口，选择“信息”，就可以把当前所有操作的命令和执行结果都显示在其中，非常方便。
        
[url](https://my.oschina.net/u/2306127/blog/372034)
