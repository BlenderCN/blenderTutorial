## Property Definitions(bpy.props)

该模块定义了扩展Blender内部数据的属性。这些函数的结果用于为使用Blender注册的类分配属性，不能直接使用。

！Note——必须将这些函数的所有参数作为关键字传递。

## Assigning to Existing Classes

可以将自定义属性添加到ID，Bone和PoseBone任何子类中。

这些属性可以是动画，可以通过用户界面和Python访问，也可以像Blender的现有属性一样访问。

    import bpy
    
    # Assign a custom property to an existing type.
    
    bpy.types.Material.custom_float = bpy.props.FloatProperty(name="Test Prob")
    
    # Test the property is there
    bpy.data.materials[0].custom_float = 5.0

## Operator Example

自定义属性的常见用途是基于python的Operator类。通过在文本编辑器中运行此代码，或单击3D视口的工具面板中的按钮来测试此代码。后者将在Redo面板中显示属性，并允许你更改它们。

    import bpy
    
    class OBJECT_OT_property_example(bpy.types.Operator):
        bl_idname = "object.property_example"
        bl_label = "Property Example"
        bl_options = {'REGISTER','UNDO'}
        
        my_float = bpy.props.FloatProperty(name="Some Floating Point")
        my_bool = bpy.props.BoolPropery(name="Toggle Option")
        my_string = bpy.props.StringPropert(name="String Value")
        
        def execute(self,context):
            self.reprot({'INFO'},'F:%.2f B: %s S: %r' % (self.my_float,self.my_bool,self.my_string))
            print('My float:',self.my_float)
            print('My bool:',self.my_bool)
            print('My string:',self.my_string)
            return {'FINISHED'}
            
    class OBJECT_PT_property_example(bpy.types.Panel):
        bl_idname = "object_PT_property_example"
        bl_label = "Property Example"
        bl_space_type = "VIEW_3D"
        bl_region_type = "TOOLS"
        bl_category = "Tools"
        
        def draw(self,context):
            # You can set the property values that should be used when the user
            # presses the button in the UI.
            props = self.layout.operator('object.property_example')
            props.my_bool = True
            props.my_string = "Shouldn't that be 47?"
            
            # You can set properties dynamically:
            if context.object:
                props.my_float = context.object.location.x
            else:
                props.my_float = 327
                
    bpy.utils.register_class(OBJECT_OT_property_example)
    bpy.utils.register_class(OBJECT_PT_property_example)
    
    # Demo call. Be sure to also test in the 3D Viewport.
    bpy.ops.object.property_example(my_float=47，my_tool = True,my_string="Shouldn't that be 327?")


<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/ApplicationDatabpyapp.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoleft.png" align="left">
</a>
<a href="https://github.com/BlenderCN/blenderTutorial/blob/master/BlenderPythonAPIDocumentation/MathTypesUtilitiesmathutils.md">
  <img src="https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/blenderpng/logoright.png" align="right">
</a>
