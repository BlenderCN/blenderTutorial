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

当我们运行脚本时，我们应该得到关于注册和取消注册清单5-1中声明的类的控制台输出。
通过更改消息并选择Print Encouraging Message,我们应该在控制台中获得以下内容：

    Unregistered class:Print an Encouraging Message
    Unregistered class:Call simple Operator
    Simple Add-on Template unregister complete
    
    Registered class:Print an Encouraging Message
    Registered class:Call Simple Operator
    Simple Add-on Template registration complete
    
    ####################################################
    # Add-on and Simple Operator executed successfully!
    # Have a nice day!
    ####################################################
    
    ####################################################
    # Add-on and Simple Operator executed successfully!
    # I changed the message!
    ####################################################
    
虽然有许多细节需要解释，但Blender插件相当优雅且易读。虽然每行代码都有一个目的，但脚本可以通过重复获得一致性。
图5-1中显示的模板相当小，但我们还包括一些可选的质量控制。在继续使用更高级的插件之前，我们会讨论每个组件。

## Blender插件的组件

Blender插件依赖于许多不同的，特别命名的变量和类函数来正常运行。我们在这里按类别详细说明。

### bl_info字典

在Blender插件中出现的第一件事应该是bl_info字典。此字典是从源文件的前1024个字节解析的，
因此bl_info必须出现在文件的顶部。我们将使用单词字典来编写类dict的Python对象。

Blender的内部引擎使用此字典中的数据来填充与插件本身相关的各种元数据。
如果我们导航到Header Menu>File>User Preferences>Add-ons,我们可以在Blender中看到各种官方和社区插件。
单击任何加载项上的插入符号显示如何使用bl_info信息填充此GUI，如图5-2所示。

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/5-2.png?raw=true)

重要的是要注意bl_info字典加载项对插件没有任何功能，而是确定最终用户如何在此窗口中找到并激活它。请参阅此处的详细说明：

    1。name——插件的名称，显示在用户首选项的插件选项卡中（例如，Math Vis（控制台），动作捕捉工具。它被写成一个单独的字符串。

    2。author——出现在用户偏好中的作者姓名(例如，Campbell Barton，Fabian Fricke)。它可以是带逗号或元组的字符串

    3。location——插件GUI的主要位置。常用语法是工具，属性和工具架面板中Window>Panel>Tab>Section。
如有疑问，请遵循其他插件建立的惯例。

    4。version——作为元组的插件的版本号。

    5。blender——根据Blender Wiki，这是运行插件所需的最小Blender版本号。社区插件经常被错误地列出(2,7,8)作为较低版本可以支持插件的版本。在许多情况下，数字是指开发人员选择支持的最低版本。

    6。description——显示在指定为单个字符串的用户首选项窗口中的简要说明。

    7。wiki_url——指向指定为单个字符串的插件的手册或指南的URL。

    8。category——字符串指定表5-1中列出的类别之一。

    Table 5-1。The bl-info Category Options
    ——————————————————————————————————————————————————————————————————————————————————
    3D View     Compositing     Lighting        Object      Rigging     Text Editor
    Add Mesh    Development     Material        Paint       Scene       UV
    Add Curve   Game Engine     Mesh            Physics     Sequencer   User Interface
    Animation   Import-Export   Node            Render      System
    ———————————————————————————————————————————————————————————————————————————————————

还有一些不常见的bl_info选项。

