# bmesh 模块

到目前为止，我们已经讨论了创建，管理和装换整个对象的方法。Blender的默认模式是对象模式，
它允许我们选择和操作一个或多个对象，通常使用可以适当应用于不同对象组的转换，例如旋转和平移。

当我们进入编辑模式时，Blender开始成为3D艺术套件。此模式允许我们选择单个对象的一个或多个顶点来执行高级和详细的转换。
正如人们所预料的那样，大多数用于编辑模式的操作无法在对象模式下执行，反之亦然。

bmesh模块几乎专门处理编辑模式操作。因此，在深入研究bmesh的功能之前，我们将对对象模式和编辑模式之间的差异进行适当的处理。

## 编辑模式

要像传统的Blender 3D艺术家一样手动进入编辑模式，请转到3D Viewport>Interaction Mode Menu>Edit Mode,如图3-1所示。
使用相同的菜单切换回对象模式。

图3-1

![](https://github.com/BlenderCN/blenderTutorial/blob/master/mDrivEngine/3-1.png?raw=true)
