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

    1。support——OFFICIAL,COMMUNITY或TESTING。如果官方提到官方支持的Blender插件，则社区引用社区支持的插件，而测试则指的是应该故意从Blender版本中排除的未完成或新添加的插件。
    
    2。tracker_url——指向bug追踪器的URL(例如Github issues或类似的)
    
    3。warning——字符串指定将出现在用户首选项窗口中的一些警告。

### 运算符和类继承(bpy.types.Operator)

在最简单的意义上，插件允许我们通过单击标准Blender GUI中的按钮来调用Blender Python函数。
必须首先将Blender GUI调用的函数注册为类bpy.types.Operator的运算符。以SimpleOperator为例。
当我们注册到这个类时，调用SimpleOperator。execute（）映射到bpy.ops中的函数对像。
函数是bpy.ops,它映射到的是由类头部的bl_idname值决定的。因此，在运行清单5-1中的脚本之后，
您可以通过从交互式控制台，加载项本身或不相关的Python脚本调用bpy.ops.object.simple_operator()来打印令人鼓舞的消息。

以下是在Blender中声明运算符的步骤。请参阅清单5-1中的SimpleOperator类定义。

    1。声明一个继承bpy.types.Operator的类。这将在我们的代码中显示为：
        class MyNewOperator(bpy.types.Operator):
    
    2。将bl_idname声明为具有您选择的类和函数名称的字符串，用句点分隔(例如，object.simple_operator或simple.message)。类和函数名称只能包含小写字符和下划线。稍后可以通过bpy.ops.my_bl_idname访问execute函数。
    
    3。(可选)将bl_label声明为描述功能的任何字符串。这将出现在Blender自动生成的功能文档和元数据中。
    
    4。声明一个执行函数。此函数将充当普通类函数，并始终接受对bpy.context的引用作为参数。通过设计bpy.types.Operator类，execute函数将始终定义：为：
        def execute(self,context):
    最佳做法是在运算符类中成功调用execute()返回{”FINISHED"}。
    
    5。(可选)声明用于注册和取消注册类的类方法。注册和取消注册函数将始终需要@classmethod装饰器并将cls作为参数。只要Blender尝试注册或取消注册运算符类，就会运行这些函数。在开发期间，包含有关类注册和注销的输出语句是有帮助的，如清单5-1所示，以检查Blender是否错误地重新注册现有类。同样重要的是要注意我们可以在这些函数中声明和删除场景属性。我们将在后面的章节中讨论。
    
为确保Blender可以使用我们的Python代码，需要遵循一些限制和指导原则。最终，
这些指南改变了我们编码的方式以及我们考虑构建Python代码库的方式。 这是我们理解Blender Python API的重点，
它开始感觉像是一个真正的应用程序编程接口(API)，而不仅仅是一组有用的函数。

### 面板和类继承(bpy.types.Panel)

bpy.types.Panel类是插件中继承的下一个最常见的类。面板已占Blender工具，Toolshelf和Properties窗口的大部分。
其中一个窗口的每个可折叠部分都是一个独特的面板。例如，如果我们导航到3D Viewport>Tools>Tools,
则默认情况下会看到三个面板：变换，编辑和历史。在Blender Python插件中，这些将由三个不同的bpy.types.Panel类表示。

以下是注册面板的要求。始终引用清单5-1中的SimplePanel类。

    1。声明一个继承bpy.types.Panel的类。这将显示为类:
        MyNewPanel(bpy.types.Panel)
    
    2。声明bl_space_type,bl_region_type,bl_category和bl_label。读者可能已经注意到这些的有序是有意的(虽然没有必要)。这四个变量按编写顺序和清单5-1中的顺序指定了用户到达面板所用的路径。在清单5-1中，它读取了VIEW_3D>TOOLS>SimpleAddon>CallSimple Operator,它看起来非常熟悉到目前为止我们在文本中找到GUI元素的方式。纠正这些变量中的案例和拼写问题。虽然类别和标签可以是任意值，但空间和区域必须引用Blender GUI的实际区域。有关bl_space_type和bl_region_type的可能参数列表，请参见5-2和5-3。
    
    Table5-2。bl_space_type Options
    ——————————————————————————————————————————————————————————————————————
    EMPTY               NLA_EDITOR          NODE_EDITOR     INFO
    VIEW_3D             IMAGE_EDITOR        LOGIC_EDITOR    FILE_BROWSER
    TIMELINE            SEQUENCE_EDITOR     PROPERTIES      CONSOLE
    GRAPH_EDITOR        CLIP_EDITOR         OUTLINER
    DOPESHEET_EDITOR    TEXT_EDITOR         USER_PREFERENCES    
    ——————————————————————————————————————————————————————————————————————
    
    Table5-3。bl_region_type Options
    ——————————————————————————————————————————————————————————————————————————————————————————
    WINDOW      HEADER      CHANNELS        TEMPORARY UI TOOLS      TOOL——PROPS     PREVIEW
    ——————————————————————————————————————————————————————————————————————————————————————————
    bl_space_type和bl_region_type的大多数组合不能一起工作，但逻辑组合通常会起作用。目前没有关于哪种空间类型和区域类型合作的完整文档。此外，并非所有空间类型和区域类型都需要声明bl_category或bl_label。再次，在逻辑上使用它们通常会产生良好的结果。
    
    3。(可选)声明bl_context。与前面的示例一样，我们可以将bl_context设置为等于objectmode,以使面板仅以对象模式显示。截至撰写本文时，我们没有针对此变量的有效选项的具体列表。API文档目前有一个TODO标记，要求更多解释。我们在后面的章节中介绍poll()方法，这是一种更灵活的方式来实现这种类型的行为。
    
    4。声明绘制方法。此函数将上下文作为参数，并始终声明为def draw(self,context):。在此函数定义中，请务必注意上下文是指bpy.context对象，但不应将其作为bpy.context传递。这个函数体中的重要变量是bpy.context.scene和self.layout。layout.prop()函数可以引用场景属性，对象属性和一些其他Blender内部属性。它将根据场景属性本身自动创建适当的输入字段。清单5-1中的encouraging_message场景属性被声明为字符串属性，因此将其作为布局的参数提供。prop()生成了一个文本输入字段。layout.operator()函数获取运算符的bl_idname,并创建一个带有text=argument指定标签的按钮。我们不会在这里详细介绍布局对象，因为高级GUI可能会非常复杂。我们将在本章后面详细讨论布局对象。
    
    5。(可选)使用装饰器@classmethod声明register()和unregister()函数，如我们对bpy.types.Operator类的讨论。
    
### register()和unregister()

在清单5-1的末尾附近有两个函数，register()和unregister(),这些函数在插件中是必需的。这两个函数负责调用bpy.utils.register_class(),bpy.utils.unregister_class(),
bpy.utils.register_module()和bpy.utils.unregister_module()。
任何继承bpy.type类的类都需要注册才能由Blender在插件中使用。当用户在用户首选项中关闭加载项时，
Blender使用unregister()函数。

注册和取消注册类有两种选择。有些工作比其他工作更好用于开发，而其他工作则更适合部署。

    1。明确注册和注销每个类。在这种情况下，我们希望以逻辑顺序注册类。依赖其他的类应在其依赖后进行注册。我们使用bpy.utils.register_class()在register()函数中执行此操作，并将类名作为参数传递。应使用unregister()函数中的bpy.utils.unregister_class()以相反的顺序取消注册类。
    
    2。根据模块中的成员资格隐式注册和注销类。我们使用bpy.utils.register_module()和bpy.utils.unregister_module()函数来实现。我们经常看到bpy.utils.register_module(__name__)在已发布的插件的register(0函数中调用，但在开发过程中它可能会很混乱，因为我们很快会解释。
    
回顾清单5-1,我们看到我们已经显示注册但隐式注销了我们的类。作者认为，这种设置非常适合单文件插件的实时编辑。
bpy.utils.unregister_module(__name__)用于清除先前运行的脚本中注册的类的插件环境。
在使用Blender的文本编辑器完成编辑期间，bpy.utils.register_module(__name__)通常会记录以前运行的脚本中已删除或未使用的类副本。

因此，实时编辑插件的清晰平板方法似乎是显式注册并隐式取消注册。隐式注销将从先前的运行中获取杂散类实例，
然后显式注册仅实例化当前运行中新创建的类。这违背了大多数文档的建议，这通常建议使用清单5-2中一种样式进行注册和注销。
清单5-1中的方法是安全，冗长的，可以很容易地修改，以符合清单5-2中普遍接受的做法。

清单5-2。注册协议

    # Option 1:
    # Using implicit registration
    
    def register():
        bpy.utils.register_module(__name__)
        
    def unregister():
        bpy.utils.unregister_module(__name__)
    
    if __name__ == "__main__":
        register()

    # Option 2:
    # Using explicit registration
    
    def register():
        bpy.utils.register_class(SimpleOperator)
        bpy.utils.register_class(SimplePanel)
        
    def unregister():
        bpy.utils.unregister_class(SimpleOperator)
        bpy.utils.unregister_class(SimplePanel)
        
    if __name__ == "__main__":
        register()
        
    # Option 3 (Recommended)
    # Explicit registration and implicit unregistration
    # with safe + verbose single-script run
    
    def register():
        bpy.utils.register_class(SimpleOperator)
        bpy.utils.register_class(SimplePanel)
        
    def unregister():
        bpy.utils.unregister_module(__name__)

    if __name__ == "__main__":
        try:
            unregister()
        except Exception as e:
            print(e)
            pass
            
        register()    
        
### 场景属性和bpy.props        

添加到场景和对象类型的属性将保存到.blend文件中。为了让用户通过Blender GUI修改变量，必须将它们注册为bpy.props.*object。
bpy.props类具有大多数数据类型的选项，包括浮点数，整数，字符串和布尔值。
它们可以注册到bpy.types.*classes,包括Scene和Object。在本节中
，我们将讨论如何将简单的场景属性注册到bpy.types.Scene.variables。这些是可以通过bpy.context.scene.*访问的任意命名变量。
虽然名称是任意的，但它仅限于小写字符和下划线。

我们可以在两个地方注册场景变量：

    1。在脚本底部的register()函数中。
    
    2。在任何继承bpy.types.*class(面板，运算符，菜单等)的类的register()类方法中。
    
最常见的是，场景变量直接与类相关联。为了清晰和组织起见，我们希望在该类的register()类方法中声明这些变量。
其他不完全适合定义的变量可以在脚本底部的register()函数中声明。在本文中，我们鼓励在register()类方法中声明场景属性，
如果与特定类密切相关，但这在现有社区插件中并不常见。

场景变量将是bpy.typs.variables的实例。这些包括Blender类型StringProperty，FloatProperty,IntProperty和BoolProperty。
每当面板通过调用self.layout.prop在GUI中包含变量时，该变量将根据其类型进行逻辑格式化。整数和浮点数显示在滑块栏中，
字符串显示为文本输入字段，布尔值显示为复选框，依次类推。

在清单5-3中，我们使用其他场景变量重新声明了清单5-1中的SimpleOpetator和SimplePanel。
读者将使用清单5-1作为模板重写这些类。结果GUI请参见图5-3。

清单5-3。探索场景属性

    # Simple Operator with Extra Properties
    class SimpleOpetator(bpy.types.Opetator):
        bl_idname = "object.simple_opetator"
        bl_label = "Print an Encourageing Message"
        
        def execute(self,context):
            print("\n\n#####################################################")
            print("# Add-on and Simple Opetator executed successfully!")
            print("# Encouraging Message:",context.scene.encouraging_message)
            print("# My Int:",context.scene.my_int_prop)
            print("# My Float:",context.scene.my_float_prop)
            print("# My Bool:",context.scene.my_bool_prop)
            print("# My Int Vector:",*context.scene.my_int_vector_prop)
            print("# My Float Vector:",*context.scene.my_float_vector_prop)
            print("# My Bool Vector:",*context.scene.my_bool_vector_prop)
            print("###########################################################")
            return {'FINISHED'}

        @classmethod
        def register(cls):
            print("Registered class:%s" % cls.bl_label)
            
            bpy.types.Scene.encouraging_message = bpy.props.StringProperty(
                name = "",
                description = "Message to print to user",
                default = "Have a nice day!")
                
            bpy.types.Scene.my_int_prop = bpy.props.IntProperty(
                name = "My Int",
                description = "Sample integer property to print to user",
                default = 123,
                min = 100,
                max = 200)
            
            bpy.types.Scene.my_float_prop = bpy.props.FloatProperty(
                name = "My Float",
                description = "Sample float property to print to user",
                default = 3.1415,
                min = 0.0,
                max = 10.0,
                precision = 4)
                
            bpy.types.Scene.my_bool_prop = bpy.props.BoolProperty(
                name = "My Bool",
                description = "Sample boolean property to print to user",
                default = True)
            
            bpy.types.Scene.my_int_vector_prop = bpy.props.IntVectorProperty(
                name = "My Int Vector",
                description = "Sample integer vector property to print to user",
                defautl = (1,2,3,4),
                subtype = 'NONE',
                size = 4)
                
            bpy.types.Scene.my_float_vector_prop = bpy.props.FloatVectorProperty(
                name = "My Float Vector",
                description = "Sample float vector property to print to user",
                default = (1.23,2.34,3.45),
                subtype = 'XYZ',
                size = 3,
                precision = 2)
                
            bpy.types.Scene.my_bool_vector_prop = bpy.props.BoolVectorProperty(
                name = "My Bool Vector",
                description = "Sample bool vector property to print to user",
                default = (True,False,True),
                subtype = 'XYZ',
                size = 3)
                
        @classmethod
        def unregister(cls):
            print('Unregistered class: %s " % cls.bl_label)
            del bpy.types.Scene.encouraging_message
            
    # Simple button in Tools panel
    class SimplePanel(bpy.types.Panel):
        bl_space_type = "VIEW_3D"
        bl_region_type = "TOOLS"
        bl_category = "Simple Addon"
        bl_label = "Call Simple Operator"
        bl_context = "objectmode"
        
        def draw(self,context):
            self.layout.operator("object.simple_operator",
                                text = "Print Encouraging Message")
            self.layout.prop(context.scene,'encouraging_message')
            self.layout.prop(context.scene,'my_int_prop')
            self.layout.prop(context.scene,'my_float_prop')            
            self.layout.prop(context.scene,'my_bool_prop')
            self.layout.prop(context.scene,'my_int_vector_prop')
            self.layout.prop(context.scene,'my_float_vector_prop')
            self.layout.prop(context.scene,'my_bool_vector_prop')
            
        @classmethod
        def register(cls):
            print("Registered class: %s " % cls.bl_label)
            # Register properties related to the class here
            
        @classmethod
        def unregister(cls):
            print("Unregistered class: %s " % cls.bl_babel)

图5-3

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/5-3.png?raw=true)  

有关可用bpy.props.*variables的列表，请参阅5-4。有关更多信息，请参阅bpy.props的API文档页面。到目前为止，
我们还没有涵盖EnumProperty，CollectionProperty或PointerProperty。我们将在本章后面介绍EnumProperty，
我们将在本章后面介绍EnumProperty,我们将在第七章介绍有关高级插件功能的CollectionProperty。

    Table5-4。Available Blender Properties
    ——————————————————————————————————————————————————————————————————————————————————
    BoolProperty            EnumProperty            IntProperty     StringProperty
    BoolVectorProperty      FloatProperty           IntVectorProperty
    CollectionProperty      FloatVectorProperty     PointerProperty
    ———————————————————————————————————————————————————————————————————————————————————

赋予属性声明的参数通常很简单，其中许多都是在不同属性之间共享的。最为显着地：

    1。default=是一个长度等于指定默认值的大小的值或元组。
    
    2。name=是将出现在输入字段左侧的GUI中的值。
    
    3。description=是用户将光标悬停在GUI元素上时显示的字符串。
    
    4。precision=指定任何float属性的显示中的小数精度。
    
    5。size=指定任何vector属性中所需的向量大小(通常为Vector,bpy_boolean或bpy_int类型)
    
    6。subtype=指定变量的所需显示格式字符串。有用的示例是XYZ和TRANSLATION，它们将在UI中的前四个变量之前显示X，Y，Z和W。另一个值得注意的例子是subtype=”COLOR“，当添加到面板时，它将创建一个有吸引力的颜色选择UI。有关颜色子类型的示例，请参见清单5-4和图5-4。请注意，Blender对颜色使用浮点范围(0.0,1.0)。表5-5和5-6显示了属性和向量属性子类型。
    
    7。min=和max=指定可以在GUI中显示的极值以及可以存储在变量中的极值。
    
    8。softmin和softmax=指定用于显示变量和缩放滑块的最小值和最大滑块值。任意值仍然可以手动输入，只要它们在最小值和最大值。
    
    9。update=接受函数作为参数。每次更新值时都会运行该函数。指定的函数应该接受self和context作为参数，
    无论它在何处被声明。此功能目前没有记录，但表现相当好。
    
    Table5-5。Available Property Subtypes
    ——————————————————————————————————————————————————————————————————————————
    PIXEL   PERCENTAGE  ANGLE   DISTANCE    UNSIGNED    FACTOR  TIME    NONE    
    ——————————————————————————————————————————————————————————————————————————
    
    Table5-6。Available Vector Property Subtypes
    ——————————————————————————————————————————————————————————————————————————
    COLOR           VELOCITY        EULER           XYZ                 NONE
    TRANSLATION     ACCELERATION    QUATERNION      COLOR_GAMMA
    DIRECTION       MATRIX          AXISANGLE       LAYER
    __________________________________________________________________________
    
清单5-4。使用颜色子类型。 

    bpy.types.Scene.my_color_prop = bpy.props.FloatVectorProperty(
        name = ”My Color Property",
        descritption = "Returns a vector of length 4",
        default=(0.322,1.0,0.182,1.0),
        min = 0.0,
        max = 1.0,
        subtype = 'COLOR',
        size = 4)

图5-4

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/5-4.png?raw=true)  

## 精确选择插件示例

在本文的这一点上，我们已经讨论了Blender Python API概念，它们具有足够的容量来开始构建有效的插件。
对于我们的第一个真正的插件，我们将参数化第3章中声明的ut.act.select_by_loc()函数，以便在编辑模式下启用精确的组选择。

在开始之前，请务必从http://blender.chrisconlan.com/ut.py 下载第三章的ut.py迭代。我们将在我们的插件中导入它。
社区使用了一些不同的协议来管理插件中的自定义导入。我们将讨论从单级目录管理自定义导入的通用协议。换句话说，
我们将导入与主脚本位于同一目录中的自定义模块。

### 我们的插件的代码概述

我们概述了从开发到部署和共享构建插件所采取的步骤：

    1。创建主脚本并在Blender的文本编辑器中将其命名为__init__.py。将清单5-1中的加载项模板复制到此脚本中。
    
    2。创建第二个脚本，并在Blender的文本编辑器中将其命名为ut.py。将http://blender.crisconlan.com/ut.py 上的Python模块复制到此脚本中。
    
    3。修改新插件的bl_info。
    
    4。添加自定义模块导入协议。请参阅清单5-5，从if “bpy” in locals():开始。很简单，为了测试我们是否处于部署模式或开发模式，我们检查bpy是否在当前命名空间中。
    
        (1)如果脚本中此时bpy位于命名空间中，我们之前已加载了插件及其相关模块。在这种情况下，使用importlib.reload()重新加载对象。
