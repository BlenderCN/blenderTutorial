blender版本 2.79

### 工具栏面板
先创建一个简单的面板

    import bpy

    class View3DPanel():
        bl_space_type = 'VIEW_3D'
        bl_region_type = 'TOOLS'

        @classmethod
        def poll(cls, context):
            return (context.object is not None)

    class PanelOne(View3DPanel, bpy.types.Panel):
        bl_idname = "VIEW3D_PT_test_1"
        bl_label = "Panel One"

        def draw(self, context):
            self.layout.label("Small Class")

    class PanelTwo(View3DPanel, bpy.types.Panel):
        bl_idname = "VIEW3D_PT_test_2"
        bl_label = "Panel Two"

        def draw(self, context):
            self.layout.label("Also Small Class")

    bpy.utils.register_class(PanelOne)
    bpy.utils.register_class(PanelTwo)
到工具面板栏的“Misc”，可以看见创建的面板。  
！[]()

### 对象属性面板

    import bpy

    class ObjectSelectPanel(bpy.types.Panel):
        bl_idname = "OBJECT_PT_select"
        bl_label = "Select"
        bl_space_type = 'PROPERTIES'
        bl_region_type = 'WINDOW'
        bl_context = "object"
        bl_options = {'DEFAULT_CLOSED'}

        @classmethod
        def poll(cls, context):
            return (context.object is not None)

        def draw_header(self, context):
            layout = self.layout
            obj = context.object
            layout.prop(obj, "select", text="")

        def draw(self, context):
            layout = self.layout

            obj = context.object
            row = layout.row()
            row.prop(obj, "hide_select")
            row.prop(obj, "hide_render")

            box = layout.box()
            box.label("Selection Tools")
            box.operator("object.select_all").action = 'TOGGLE'
            row = box.row()
            row.operator("object.select_all").action = 'INVERT'
            row.operator("object.select_random")

    bpy.utils.register_class(ObjectSelectPanel)
    
### 面板对象的属性域
对用到的各个域说明如下：

    class bpy.types.Panel(bpy_struct)
    Panel containing UI elements：

    bl_category
        Type:string, default “”, (never None)
    bl_context
        Type:string, default “”, (never None)   
        The context in which the panel belongs to. 
        (TODO: explain the possible combinations bl_context/bl_region_type/bl_space_type)   
    bl_idname
        Type:string, default “”, (never None)  
        If this is set, the panel gets a custom ID, otherwise it takes 
        the name of the class used to define the panel. For example, if the 
        class name is “OBJECT_PT_hello”, and bl_idname is not set by the script,
        then bl_idname = “OBJECT_PT_hello”
    bl_label
        Type:string, default “”, (never None) 
        The panel label, shows up in the panel header at the right of the triangle 
        used to collapse the panel。
    bl_options
        Options for this panel type
        DEFAULT_CLOSED Default Closed, Defines if the panel has to be open or collapsed 
        at the time of its creation.
    HIDE_HEADER
        Hide Header, If set to False, the panel shows a header, which contains a
        clickable arrow to collapse the panel and the label (see bl_label).
        Type:enum set in {‘DEFAULT_CLOSED’, ‘HIDE_HEADER’}, default {‘DEFAULT_CLOSED’}    
    bl_region_type
        The region where the panel is going to be used in
        Type:enum in [‘WINDOW’, ‘HEADER’, ‘CHANNELS’, ‘TEMPORARY’, 
        ‘UI’, ‘TOOLS’, ‘TOOL_PROPS’, ‘PREVIEW’], default ‘WINDOW’    
    bl_space_type
        Type:enum in [‘EMPTY’, ‘VIEW_3D’, ‘TIMELINE’, ‘GRAPH_EDITOR’, ‘DOPESHEET_EDITOR’,
        ‘NLA_EDITOR’, ‘IMAGE_EDITOR’, ‘SEQUENCE_EDITOR’, ‘CLIP_EDITOR’, 
        ‘TEXT_EDITOR’, ‘NODE_EDITOR’, ‘LOGIC_EDITOR’, ‘PROPERTIES’, ‘OUTLINER’, 
        ‘USER_PREFERENCES’, ‘INFO’, ‘FILE_BROWSER’, ‘CONSOLE’], default ‘EMPTY’  
        面板的space域是一枚举值，可用的属性值如下：
        EMPTY：空值。
        VIEW_3D：三维视口。
        TIMELINE：时间线和回放控制。
        GRAPH_EDITOR：Graph编辑器，关键帧编辑。
        DOPESHEET_EDITOR：Dope Sheet, 关键帧调节。
        NLA_EDITOR：NLA Editor, 合并和层操作。
        IMAGE_EDITOR：UV/Image Editor, UV Maps和图像编辑器。
        SEQUENCE_EDITOR：视频序列编辑工具。
        CLIP_EDITOR：电影剪辑编辑, 动作捕捉编辑。
        TEXT_EDITOR：文本编辑器。
        NODE_EDITOR：节点编辑器, node-based shading and compositing tools.
        LOGIC_EDITOR：逻辑编辑器, Game logic editing.
        PROPERTIES：属性编辑, Edit properties of active object and related datablocks.
        OUTLINER：Outliner, Overview of scene graph and all available datablocks.
        USER_PREFERENCES：用户偏好设置, Edit persistent configuration settings.
        INFO：信息显示窗口, Main menu bar and list of error messages (drag down to expand and display).
        FILE_BROWSER：文件浏览器, Browse for files and assets.
        CONSOLE Python控制台, 交互运行和脚本开发.
      
    bl_translation_context
        Type:string, default “*”, (never None)
    layout
        Defines the structure of the panel in the UI
        Type:UILayout, (readonly)    
    text
        XXX todo
        Type:string, default “”, (never None)    
    use_pin
        Type:boolean, default False    
    classmethod poll(context)
        If this method returns a non-null output, then the panel can be drawn
        Return type:boolean    
    draw(context)
        Draw UI elements into the panel UI layout
    draw_header(context)
        Draw UI elements into the panel’s header UI layout




[参考url](https://my.oschina.net/u/2306127/blog/372116)
