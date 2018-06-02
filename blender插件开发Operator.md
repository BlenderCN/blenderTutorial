毫无疑问，Operator是Blender中最核心的一个对象，而且里面保罗万象（不仅可以操作几何对象，所有的菜单都可以控制，
因为blender其实是一个三维图形的超级命令解释器嘛！）。

我们先定义一个简单的Operator。把下面的代码复制到文本编辑中，点击“执行脚本”。

    import bpy

    class SimpleOperator(bpy.types.Operator):
        bl_idname = "object.simple_operator"
        bl_label = "Tool Name"

        def execute(self, context):
            print("Hello World")
            return {'FINISHED'}

    bpy.utils.register_class(SimpleOperator)
    
在三维视窗中，鼠标点击，按下空格键输入“tool”，将会列出这个“Tool Name”的Operator工具。

继续，选中“Tool Name”项，并没有出现上面的execute中定义的“Hello World”，怎么回事！？

因为Blender把输出信息定义到标准输出了，如果从控制台窗口启动，将从控制台上看到输出的信息。Blender并没有把信息输出到信息窗口和python控制台，
这是一个让图形界面使用者有点困惑的地方，先按下不表，后面再想办法解决。

现在，换一种方式来定义Operator：

    import bpy

    class BaseOperator:
        def execute(self, context):
            print("Hello World BaseClass")
            return {'FINISHED'}

    class SimpleOperator(bpy.types.Operator, BaseOperator):
        bl_idname = "object.simple_operator"
        bl_label = "Tool Name"

    bpy.utils.register_class(SimpleOperator)
    
这里用到了python类的继承特性。这是官方文档的例子，但我觉得更符合对象的定义应该是：

    import bpy

    class BaseOperator:
        bl_idname = "object.simple_operator"
        bl_label = "Tool subclass."

    class SimpleOperator(BaseOperator,bpy.types.Operator):
        def execute(self, context):
            print("Hello World，i am subclass.")
            return {'FINISHED'}

    bpy.utils.register_class(SimpleOperator)

酸菜萝卜，各有所爱。一种是行为优先，符合脚本化思路;第二种是标识优先，符合OO的习惯。测试了下，两种方法倒是都能运行。

上面直接把Operator的执行类注册到系统了，下面采用更通用的做法：

    import bpy

    class SimpleOperator(bpy.types.Operator):
        """ See example above """
        def register():
            bpy.utils.register_class(SimpleOperator)
        def unregister():
            bpy.utils.unregister_class(SimpleOperator)

    if __name__ == "__main__":
        register()  
        
一个更综合一点的例子：

    # Create new property group with a sub property
    # bpy.data.materials[0].my_custom_props.sub_group.my_float
    import bpy

    class MyMaterialSubProps(bpy.types.PropertyGroup):
        my_float = bpy.props.FloatProperty()

    class MyMaterialGroupProps(bpy.types.PropertyGroup):
        sub_group = bpy.props.PointerProperty(type=MyMaterialSubProps)

    def register():
        bpy.utils.register_class(MyMaterialSubProps)
        bpy.utils.register_class(MyMaterialGroupProps)
        bpy.types.Material.my_custom_props = bpy.props.PointerProperty(type=MyMaterialGroupProps)

    def unregister():
        del bpy.types.Material.my_custom_props
        bpy.utils.unregister_class(MyMaterialGroupProps)
        bpy.utils.unregister_class(MyMaterialSubProps)

    if __name__ == "__main__":
        register()
        
因为“一切皆对象”，Python脚本还可以创建动态类型。如下：
    
    # add a new property to an existing type
    bpy.types.Object.my_float = bpy.props.FloatProperty()

    # remove
    del bpy.types.Object.my_float
    
对类对象同样适用：
    
    class MyPropGroup(bpy.types.PropertyGroup):
        pass
    MyPropGroup.my_float = bpy.props.FloatProperty()

    ＃等价于下面的语法:
    class MyPropGroup(bpy.types.PropertyGroup):
        my_float = bpy.props.FloatProperty()
        
动态定义的对象：

    for i in range(10):
        idname = "object.operator_%d" % i

        def func(self, context):
            print("Hello World", self.bl_idname)
            return {'FINISHED'}

        opclass = type("DynOp%d" % i,(bpy.types.Operator, ),
                       {"bl_idname": idname, "bl_label": "Test", "execute": func},
                       )
        bpy.utils.register_class(opclass)
        
有点头大，不多解释了。运行下看： 

    >>> bpy.ops.object.operator_1()
    Hello World OBJECT_OT_operator_1
    {'FINISHED'}

    >>> bpy.ops.object.operator_2()
    Hello World OBJECT_OT_operator_2
    {'FINISHED'}

现在，我们创建一个操作几何对象的Addon:

    import bpy

    bl_info = {
        "name": "Move X Axis",
        "category": "Object",
        }

    class ObjectMoveX(bpy.types.Operator):
        """My Object Moving Script"""      
        # blender will use this as a tooltip for menu items and buttons.

        bl_idname = "object.move_x"  #unique identifier for buttons and menu items to reference.
        bl_label = "Move X by One"         # display name in the interface.
        bl_options = {'REGISTER', 'UNDO'}  # enable undo for the operator.

        # execute() is called by blender when running the operator.
        def execute(self, context):     # The original script
            scene = context.scene
            for obj in scene.objects:
                obj.location.x += 1.0
            return {'FINISHED'}      #this lets blender know the operator finished successfully.

    def register():
        bpy.utils.register_class(ObjectMoveX)

    def unregister():
        bpy.utils.unregister_class(ObjectMoveX)

    # This allows you to run the script directly from blenders text editor
    # to test the addon without having to install it.
    if __name__ == "__main__":
        register()

一个扩展了菜单和快捷键映射的例子：

    bl_info = {
        "name": "Cursor Array",
        "category": "Object",}

    import bpy

    class ObjectCursorArray(bpy.types.Operator):
        """Object Cursor Array"""
        bl_idname = "object.cursor_array"
        bl_label = "Cursor Array"
        bl_options = {'REGISTER', 'UNDO'}

        total = bpy.props.IntProperty(name="Steps", default=2, min=1, max=100)

        def execute(self, context):
            scene = context.scene
            cursor = scene.cursor_location
            obj = scene.objects.active

            for i in range(self.total):
                obj_new = obj.copy()
                scene.objects.link(obj_new)

                factor = i / self.total
                obj_new.location = (obj.location * factor) + (cursor * (1.0 - factor))

            return {'FINISHED'}

    def menu_func(self, context):
        self.layout.operator(ObjectCursorArray.bl_idname)
    
    # store keymaps here to access after registration
    addon_keymaps = []

    def register():
        bpy.utils.register_class(ObjectCursorArray)
        bpy.types.VIEW3D_MT_object.append(menu_func)

        # handle the keymap
        wm = bpy.context.window_manager

        # Note that in background mode (no GUI available), 
        # keyconfigs are not available either, so we have to check this
        # to avoid nasty errors in background case.
        kc = wm.keyconfigs.addon
        if kc:
            km = wm.keyconfigs.addon.keymaps.new(name='Object Mode', space_type='EMPTY')
            kmi = km.keymap_items.new(ObjectCursorArray.bl_idname, 'SPACE', 'PRESS', 
                ctrl=True, shift=True)
            kmi.properties.total = 4
            addon_keymaps.append((km, kmi))

    def unregister():
        # Note: when unregistering, it's usually good practice to do it in reverse order you registered.
        # Can avoid strange issues like keymap still referring to operators already unregistered...
        # handle the keymap
        for km, kmi in addon_keymaps:
            km.keymap_items.remove(kmi)
        addon_keymaps.clear()

        bpy.utils.unregister_class(ObjectCursorArray)
        bpy.types.VIEW3D_MT_object.remove(menu_func)

    if __name__ == "__main__":
        register()
        
复制到文本框，点击“运行脚本”运行下看： 
![](http://static.oschina.net/uploads/img/201501/28091359_vsQD.png)

运行这个菜单项，结果如下：
![](http://static.oschina.net/uploads/img/201501/28091401_BJel.png)

当选中以后，可以选择一次创建多个Cube对象。

注意：直接运行这个脚本，将会导致菜单项多次加载。但是放到Addon就没有这个问题了。
