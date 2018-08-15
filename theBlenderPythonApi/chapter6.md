# bgl和blf模块

bgl模块是Blender在3D视窗和Blender游戏引擎中常用的OpenGL函数的包装器。
OpenGL(开放图形库)是一个开源的低级API，用于无数的3D应用程序，以利用硬件加速计算。

对于熟悉OpenGL的读者来说，bgl文档似乎很熟悉。bgl模块本身旨在模仿OpenGL2.1的调用结构和逐帧渲染风格。

在阅读bgl文档时，我们注意到许多高级概念，如缓冲操作，面部剔除和光栅化。幸运的是，对于Blender Python程序员来说，
3D视窗已经在管理这些操作。我们更关注用额外信息标记3D视窗以帮助用户理解他的模型。本章主要关注使用bgl绘图。

blf模块是一小组用于显示文本和绘图字体的函数。它与bgl密切相关，在没有它的例子中很少提及。
Blender Python开发人员通常将bgl和blf模块组合在一起制作测量工具，用bgl绘制线条并用blf显示它们的测量值。
我们在本章就是这么做的。

请注意，这些模块通常在bge(Blender Game Engine)模块的示例中看到。我们不会在Blender游戏引擎中工作，
因此这些脚本将无法运行，并且尝试导入bge将失败。我们将绘图限制为3D视窗。

另请注意，bgl模块设置为在Blender 2.80+中替换或重新构建。本章可能是本文发布后第一次更新。

## 瞬间绘图

bgl和blf模块的教学方式与其他Blender Python模块不同。当通过这些模块中的任何一个在3D视窗上绘制线条或字符时，
它仅对单个帧可见。因此，我们无法像在其他模块中那样在交互式控制台中进行实验。
我们在交互式控制台中执行的功能可以无误地执行，但我们将无法在3D视窗中查看结果。

为了有效地使用bgl和blf模块，我们必须在处理函数中使用它们，该函数设置为在每次帧更改时更新。
因此，我们从使用非OpenGL概念的处理程序示例开始。

## 处理程序概述

本节提供了使用bpy.app.handlers的处理程序示例。这不是我们在处理bgl和blf时最终会使用的子模块，
但它对于在Blender中学习处理程序是有益的。

### 时钟示例

处理程序是设置为每次事件发生时运行的函数。为了实例化一个处理程序，我们声明一个函数，
然后将它添加到Blender中可能的处理程序列表之一。在清单6-1中，我们创建了一个用当前时间修改文本网格文本的函数。
然后我们将函数添加到bpy.app.handlers。scene_update_pre表示我们希望它在3D视窗更新和显示之前运行。

结果似乎是3D视窗中的时钟。实际上，它是一个每秒更新许多次的文本网格。此示例不安全或不完整，
但只要我们将对象保留在场景中命名为MyTextObj，我们就可以在后台运行时钟添加和编辑其他对象。结果见图6-1

清单6-1。Blender时钟处理程序示例

    import bpy
    import datetime
    
    # Clear the scene
    bpy.ops.object.select_all(action='SELECT')
    bpy.ops.object.delete()
    
    # Create an object for our clock
    bpy.ops.object.text_add(location=(0,0,0))
    bpy.context.object.name = 'MyTextObj'
    
    # Create a handler function
    def tell_time(dummy):
        current_time = datetime.datetime.now().strftime('%H:%M:%S.%f')[:-3]
        bpy.data.objects['MyTextObj'].data.body = current_time
        
    # Add to the list of handler functions "scene_update_pre"
    bpy.app.handlers.scene_update_pre.append(tell_time)

图6-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/6-1.png?raw=true)    

### 管理处理程序

在bpy.app.handlers的情况下，我们可以编辑各种函数列表来管理我们的处理程序。这些列表实际上是类型列表的Python类，
我们可以对它们进行操作。我们可以使用list类方法，如append(),pop(),remove()和clear()来管理我们的处理函数。
有关一些有用的示例，请参见清单6-2。

清单6-2。管理处理程序列表

    # Will only work if 'tell_time' is in scope
    bpy.app.handlers.scene_update_pre.remove(tell_time)
    
    # Useful in development for a clean slate
    bpy.app.handlers.scene_update_pre.clear()
    
    # Remove handler at the end of the list and return it
    bpy.app.handlers.scene_update_pre.pop()
    
### 处理程序类型

在清单6-1中，我们使用bpy.app.handlers.scene_update_pre在每次更新之前根据内部变量修改网格。
表6-1详细介绍了官方文档中出现的bpy.app.handlers中的处理程序类型。

表6-1中存在一些功能重叠，并不是每个处理程序都表现出人们的期望。例如，
使用清单6-1的scene_update_post而不是scene_update_pre根本不起作用。鼓励读者尝试确定哪一个符合他们的需求。

    Table6-1.Type of Handlers
    ——————————————————————————————————————————————————————————————————————————————
    Handler                 Called on
    ——————————————————————————————————————————————————————————————————————————————
    frame_change_post       After frame change during rendering or playback
    frame_change_pre        Before frame change during rendering or playback
    render_cancel           Canceling a render job
    render_complete         Completing a render job
    render_init             Initia lizing a render job
    render_post             After render
    render_pre              Before render
    render_stats            Printing render statistics
    render_write            Directly after frame is written in rendering
    load_post               After loading a .blend file
    load_pre                Before loading a .blend file
    save_post               After saving a .blend file
    save_pre                Before saving a .blend file
    scene_update_post       After updating scene data(e.g,3D Viewport)
    scene_update_pre        Before updating scene data(e.g,3D Viewport)
    game_pre                Starting the game engine
    game_post               Ending the game engine
    ——————————————————————————————————————————————————————————————————————————————
    
### 持久处理程序

如果我们希望在加载.blend文件后继续处理程序，我们可以添加@persistent装饰器。通常，
在加载.blend文件时会释放处理程序，因此像bpy.app.handlers.load_post这样的某些处理程序需要这个修饰器。
清单6-3使用@persistent装饰器在加载.blend文件后打印文件诊断。

清单6-3。在加载时输出文件诊断。

    import bpy
    from bpy.app.handlers import persistent
    
    @persistent
    def load_diag(dummy):
        obs = bpy.context.scene.objects
        print('\n\n### File Diagnostics ###')
        print('Objects in Scene:',len(obs))
        for ob in obs:
            print(ob.name,'of type',ob.type)
            
    bpy.app.handlers.load_post.append(load_diag)
    
    # After reloading startup file:
    #
    # ### File Diagnostice ###
    # Objects in Scene: 3
    # Cube of type MESH
    # Lamp of type LAMP
    # Camera of type CAMERA
    
## 处理blf和bgl

现在我们已经对处理程序有了基本的了解，我们将详细介绍如何直接在3D视窗上使用OpenGL工具进行绘制。
用于在3D视窗上绘图的处理程序不是bpy.app.handlers的一部分，而是bpy.types.SpaceView3D的未记录的成员函数。
为了理解这些成员函数，我们减少了其他开发人员使用它们的实际示例。

清单6-4显示了如何使用bgl和blf在其原点上绘制对象的名称。

    import bpy
    from bpy_extras import view3d_utils
    import bgl 
    import blf
    
    # Color and font size of text
    rgb_label = (1,0.8,0.1,1.0)
    font_size = 16
    font_id = 0
    font_size = 16
    font_id = 0
    
    # Wrapper for mapping 3D Viewport to OpenGL 2D region
    
    def gl_pts(context,v):
        return view3d_utils.location_3d_to_region_2d(
            context.region,
            context.space_data.region_3d,
            v)
    # Get the active object,find its 2D points,draw the name
    
    def draw_name(context):
    
        ob = context.object
        v = gl_pts(context,ob.location)
        
        bgl.glColor4f(*rgb_label)
        blf.size(font_id,font_size,72)
        blf.position(font_id,v[0],v[1],0)
        blf.draw(font_id,ob.name)

    # Add the handler
    # aruments:
    # function = draw_name,
    # tuple of parameters = (bpy.context,),
    # constant1 = 'WINDOW'，
    # constant2 = 'POST_PIXEL'
    bpy.types.SpaceView3D.draw_handler_add(draw_name,(bpy.context,),'WINDOW','POST_PIXEL')

在文本编辑器中运行清单6-4将允许您查看在其原点上绘制的活动对象的名称。

使用bpy.types.SpaceView3D创建的处理程序不像bpy.app.handlers中那样容易访问，并且默认情况下是持久的。
除非我们创建更好的控件来打开和关闭这些处理程序，否则我们将不得不重新启动Blender以分离此处理程序。
在下一节中，我们将此处理程序放在一个插件中，该插件允许我们使用按钮来打开和关闭它。此外，
我们将处理程序存储在bpy.types.Operator中，因此在将函数添加到处理程序后我们不会丢失对该函数的引用。

## 示例插件

此插件是一个独立的脚本，因此可以通过将其复制到文本编辑器或通过用户首选项导入它来运行。
我们鼓励读者通过文本编辑器运行它，以便进行简单的试验。有关插件，请参见清单6-5,
有关结果的屏幕截图，请参见图6-2(在编辑模式下)。

清单6-5。简单的线条和文字绘图

    bl_info = {
        "name" : "Simple Line and Text Drawing",
        "author" : "Chris Conlan",
        "location" : "View3D > Tools > Drawing",
        "version" : (1,0,0),
        "blender" : (2,7,8),
        "description" : "Minimal add-on for line and text drawing with bgl and blf."
                        "Adapted from Antonio Vazquez's (antonioya) Archmesh.",
        "wiki_url" : "http://example.com",
        "category" : "Development"
        }
        
    import bpy
    import bmesh
    import os
    import bpy_extras
    import bgl
    import blf
    
    # view3d_utils must be imported explicitly
    from bpy_extras import view3d_utils
    
    def draw_main(self,context):
        """Main function,toggled by handler"""
        
        scene = context.scene
        indices = context.scene.gl_measure_indices
        
        # Set color and fontsize parameters
        rgb_line = (0.173,0.545,1.0,1.0)
        rgb_label = (1,0.8,0.1,1.0)
        fsize = 16
        
        # Enable OpenGL drawing
        bgl.glEnable(bgl.GL_BlEND)
        bgl.glLineWidth(1)
        
        # Store reference to active object
        ob = context.object
        
        # Draw vertex indices
        if scene.gl_display_verts:
            label_verts(context,ob,rgb_label,fszie)
            
        # Draw measurement
        if scene.gl_display_measure:
            if (indices[1] < len(ob.data.vertices)):
                draw_measurement(context,ob,indices,rgb_line,rgb_label,fsize)

        # Draw name
        if scene.gl_display_names:
            draw_name(context,ob,rgb_label,fsize)
            
        # Disable OpenGL drawings and restore defaults
        bgl.glLineWidth(1)
        bgl.glDisable(bgl.GL_BLEND)
        bgl.glColor4f(0.0,0.0,0.0,1.0)
        
    class glrun(bpy.types.Operator):
        """Main operator,flicks handler on/off"""
        
        bl_idname = "glinfo.glrun"
        bl_label = "Dispaly object data"
        bl_description = "Display additional information in the 3D Viewport"
        
        # For storing function handler
        _handle = None
        
        # Enable GL drawing and add handler
        @staticmethod
        def handle_add(self,context):
            if glrun._handle is None:
                glrun._handle = bpy.types.SpaceView3D.draw_handler_add(draw_main,(self,context),'WINDOW','POST_PXEL')
                context.window_manager.run_opengl = True
        
        # Disable GL drawing and remove handler
        @staticmethod
        def handle_remove(self,context):
            if glrun.handle is not None:
                bpy.tpes.SpaceView3D.draw_handler_remove(glrun._handle,'WINDOW')
            glrun._handle = None
            context.window_manager.run_opengl = False
            
        # Flicks OpenGL handler on and off
        # Make sure to flick "off" before reloading script when live editing
        def execute(self,context):
            if context.area.type == 'VIEW_3D':
                
                if context.window_manager.run_opengl is False:
                    self.handle_add(self,context)
                    context.area.tag_redraw()
                else:
                    self.handle_remove(self,context)
                    context.area.tag_redraw()
                
                return {'FINISHED'}
            else:
                print("3D Viewport not found,cannot run operator.")
                return {'CANCELLED'}
                
    class glpanel(bpy.types.Panel):
        """Standard panel with scene variables"""
        
        bl_idname = "glinfo.glpanel"
        bl_label = "Display Object Data"
        bl_space_type = 'VIEW_3D'
        bl_region_type = " TOOLS"
        bl_category = 'Drawing'
        
        def draw(self,context):
            lay = self.layout
            scn = context.scene
            
            box = lay.box()
            
            if context.window_manager.run_opengl is False:
                icon = 'PLAY'
                txt = 'Display'
            else:
                icon = 'PAUSE'
                txt = 'Hide'
            box.operator("glinfo.glrun",text=txt,icon=icon)
            
            box.prop(scn,"gl_display_names",toggle=True,icon="OUTLINER_OB_FONT")
            box.prop(scn,"gl_display_verts",toggle=True,icon='DOT')
            box.prop(scn,"gl_display_measure",toggle=True,icon="ALIGN")
            box.prop(scn,"gl_measure_indices")
        @classmethod
        def register(cls):
        
            bpy.types.Scene.gl_display_measure = bpy.props.BoolProperty(
                name = "Measures",
                description = "Display measurement for specified indices in active mesh.",
                default = True,)
                
            bpy.types.Scene.gl_display_names = bpy.props.BoolProperty(
                name = "Names",
                description = "Sisplay names for selected meshes.",
                default = True,)
                
            bpy.types.Scene.gl_display_verts = bpy.props.BoolProperty(
                name = "Verts",
                description = "Display vertex indices for selected meshes.",
                default = True,)
                
            bpy.types.Scene.gl_measure_indices = bpy.props.IntVectorProperty(
                name = "Indices",
                description = "Display measurement between supplied vertices.",
                default = (0,1),
                min = 0,
                subtype = 'NONE',
                size = 2)
                
            print("registered class %s " % cls.bl_label)
            
        @classmethod
        def unregister(cls):
            del bpy.types.Scene.gl_display_verts
            del bpy.types.Scene.gl_display_names
            del bpy.types.Scene.gl_display_measure
            del bpy.types.Scene.gl_measure_indices
            
            print("unregistered class %s " % cls.bl_label)
                
    #### Button-activated drawing functions #####
    
    # Draw the name of the object on its origin
    def draw_name(context,ob,rgb_label,fsize):
        a = gl_pts(context,ob.location)
        bgl.glColor4f(rgb_label[0],rgb_label[1],rgb_label[2],rgb_label[3])
        draw_text(a,ob.name,fsize)
        
    # Draw line between two points,draw the distance
    def draw_measurement(context,ob,pts,rgb_line,rgb_label,fsize):
        # pts = (index of vertex #q,index of vertex #2)
        
        a = coords(ob,pts[0])
        b = coords(ob,pts[1])
        
        d = dist(a,b)
        
        mp = midpoint(a,b)
        
        a = gl_pts(context,a)
        b = gl_pts(context,b)
        mp = gl_pts(context,mp)
        
        bgl.glColor4f(rb_line[0],rgb_line[1],rgb_line[2],rgb_line[3])
        draw_line(a,b)
        
        bgl.glColor4f(rgb_label[0],rgb_label[1],rgb_label[2],rgb_label[3])
        draw_text(mp,'%.3f' % d, fsize)

    # Label all possible vertices of object
    def label_verts(context,ob,rgb,fsize):
        try:
            # attempt get coordinates,will except if object does not have vertices
            v = coords(ob)
            bgl.glColor4f(rgb[0],rgb[1],rgb[2],rgb[3])
            for i in range(0,len(v)):
                loc = gl_pts(context,v[i])
                draw_text(loc,str(i),fsize)
        except AttributeError:
            # Except attribute error to not fail on lights,cameras,etc
            pass
    # Convert 3D points to OpenGL-compatible 2D points
    def gl_pts(context,v):
        return bpy_extras.view3d_utils,location_3d_to_region_2d(
            context.region,
            context.space_data.region_3d,
            v)
    ##### Registration ########
    def register():
        """Register objects inheriting bpy.types in current file and scope"""
        
        # bpy.utils.register_module(__name__)
        
        # Explicitly register objects
        bpy.utils.register_class(glrun)
        bpy.utils.register_class(glpanel)
        wm = bpy.types.WindowManager
        wm.run_opengl = bpy.props.BoolProperty(default = False)
        
        print("%s registration complete\n" % bl_info.get('name'))

    def unregister():
    
        wm = bpy.context.window_manager
        p = 'run_opengl'
        if p in wm:
            del wm[p]
        
        # remove OpenGL data
        glrun.handle_remove(glrun,bpy.context)
        
        # Always unregister in reverse order to prevent error due to
        # interdependencies
        
        # Explicitly unregister objects
        # bpy.utils.unregister_class(glpanel)
        # bpy.utils.unregister_class(glrun)
        
        # Unregister objects inheriting bpy.types in current file and scope
        bpy.utils.unregister_module(__name__)
        print("%s unregister complete\n" % bl_info.get('name'))
        
    # Only called during development with 'Text Editor -> Run Scrpt'
    # When distributed as pluin,Blender will directly call register()
    if __name__ == "__main__":
        try:
            os.system('clear')
            unregister()
        except Exception as e:
            print(e)
            pass
        finally:
            register()
            
![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/6-2.png?raw=true)            
