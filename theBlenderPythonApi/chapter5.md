# 介绍插件开发

本章使用Blender的Python API构建基本的插件。插件开发的最大障碍之一是从开发环境过渡到整齐打包和独立于操作系统的插件，
因此我们在本章中花费了大量时间讨论各种开发实践。到本章结束时，读者应该能够在开发和部署环境中注册简单的插件。
以下章节以此知识为基础，将更多高级功能集成到插件中。

## 一个简单插件模板

对于此部分，在Blender中输入脚本视图，然后转到Text Editor>New以创建新脚本。给它起一个名字，例如simpleaddon.py。
有关我们可以开始构建插件的简单模板，请参阅清单5-1。运行此脚本将在工具面板中创建一个名为简单插件的新选项卡，
该选项卡具有简单的文本输入字段和按钮。该按钮将向控制台输出一条消息，验证插件是否正常工作，
然后鹦鹉式地返回文本输入字段中的字符串。有关插件GUI的外观和位置，请参见图5-1。

清单5-1。简单的插件模板

    bl_info = {
        "name" : "Simple Add-on Template",
        "author" : "Fofight",
        "location" : "View3D >Tools > Simple Addon",
        "version" : (1,0,0),
        "blender" : (2,7,8),
        "description" : "Starting point for new add-ons.",
        "wiki_url" : "http://example.com",
        "category" : "Development"
                }
    
    # Custom modules are imported here
    # See end of chapter example for suggested protocal
    
    import bpy
    
    # Panels,buttons,operators,menus,and
    # functions are all declared in this area
    
    # A simple Operator class
    
    class SimpleOperator(bpy.types.Operator):
        bl_idname = "object.simple_operator"
        bl_label = "Print an Encouraging Message"
        
        def execute(self,context):
            print("\n\n###################################################"
            print("# Add-on and Simple Operator executed successfully!")
            print(" # " + context.scene.encouraging_message)
            print("######################################################")
            return {'FINISHED'}
            
        @classmethod
        def register(cls):
            print("Registered class: %s " % cls.bl_label)
            
            # Register properties related to the class here
            bpy.types.Scene.encouraging_message = bpy.props.StringProperty(
                name = "",
                description = "Message to print to user",
                default = “Have a nice day!")

        @classmethod
        def unregister(cls):
            print("Unregistered class: %s " % cls.bl_label)
            
            # Delete parameters related to the class here
            del bpy.types.Scene.encouraging_message
    
    # A simple button and input field in the Tools panel
    class SimplePanel(bpy.types.Panel):
        bl_space_type = "VIEW_3D"
        bl_region_type = "TOOLS"
        bl_category = "Simple Addon"
        bl_label = "Call Simple Operator"
        bl_context = objectmode"
        
        def draw(self,context):
            self.layout.operator("object.simple_operator",
                                text="Print Encouraging Message")
        @classmethod
        def register(cls):
            print("Registered class: %s" % cls.bl_label)
            # Register properties related to the class here.
            
        @classmethod
        def unregister(cls):
            print("Unregistered class: %s " % cls.bl_label)
            # Delete parameters related to the class here
    
    def register():
        # Implicitly register objects inheriting bpy.types in current file and scope
        # bpy.utils.register_module(__name__)
        
        # Or explicitly register objects
        bpy.utils.register_class(SimpleOperator)
        bpy.utils.register_class(SimplePanel)
        
        print("%s registration complete\n" % bl_info.get('name'))
        
    def unregister():
        
        # Always unregister in reverse order to prevent  error due to
        # interdependencies
        
        # Explicitly unregister objects
        # bpy.utils.unregister_class(SimpleOperator)
        # bpy.utils.unregister_class(SimplePanel)
        
        # Or unregister objects inheriting bpy.types in current file and scope
        bpy.utils.unregister_module(__name__)
        print("%s unregister complete\n" % bl_info.get('name'))
        
    # Only called during development with 'Text Editor -> Run Script'
    # When distributed as plugin,Blender will directly
    # and call register() and unregister()
    if __name__ == "__main__":
    
        try:
            unregister()
        except Exception as e:
            # Catch failure to unregister explicitly
            print(e)
            pass
        
        register()
        
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/5-1.png?raw=true)        
