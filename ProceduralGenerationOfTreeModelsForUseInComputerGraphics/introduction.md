# 1.1 Motivation

在这个项目中，我打算开发一个系统，用于生成高度详细的树木和类似植物的三维模型，用于计算机生成的图像（CGI)。

近年来，CGI在娱乐业中变得越来越普遍，技术的进步使得越来越大规模和复杂的场景得以构建。因此，人们对提供快速、有效的生产资产的方法产生了更大的渴望，这些资产可以在这些场景中使用。随着植物在现实世界中的大量存在，对植物资产的高需求也就不足为奇了。因此，寻找一种简单的方法来产生大量真实的植物模型将对从事电影和游戏环境的艺术家们有很大的帮助。

我的目标是实现一个系统，它可以伪随机的方式产生各种各样的树种模型。我选择创建两个使用替代技术的系统，目的是比较这些方法和所取得的成果。我将实现基于Aristid Lindenmayer的分形方法[PL 90](http://algorithmicbotany.org/papers/selforg.sig2009.small.pdf)的系统，以及Weber和Penn的参数化方法[wp 95](http://www.cs.duke.edu/courses/cps124/spring08/assign/07_papers/p119-weber.pdf),这两种方法都在下面详细描述。我还希望通过使用维数约简和遗传算法等技术来简化/自动化任务的某些方面，从而改进艺术家树模型的设计过程。

# 1.2 Related Work

这一领域现有的工作相当多。最成功的商业产品是SpeedTree[Inc],这是一个专门为游戏和电影制作植物模型而设计的软件包。研究界也展开了一些活动，并提出了一些完全不同的方法来解决这一问题。

## 1.2.1 Lindenmayer-Systems

一种技术是使用Lindenmayer系统（L-system），由Przemyslaw prusinkiewicz和Aristid Lindenmayer在1990年的标志性书籍“植物的算法美[PL 90](http://algorithmicbotany.org/papers/selforg.sig2009.small.pdf)中提出，并在后者1968年的论文[lin68](http://www.sciencedirect.com/science/article/pii/0022519368900805)中首次描述。L-systems建立在由mandelbrot[man 82]()推广的公认的自然结构的自相似性基础上，使用分形风格的方法来模拟过程。Smith在Lucasfilm[Smi84](http://www.alvyray.com/Papers/CG/PlantsFractalsandFormalLanguages.pdf)上使用了这项技术，取得了令人印象深刻的效果，尽管近年来它已经不再受欢迎了。

我选择了一个基于L-systems的工具，目的是融合当时最流行的现代技术。L-systems的主要方面在第[2.3.1](https://github.com/BlenderCN/blenderTutorial/blob/master/ProceduralGenerationOfTreeModelsForUseInComputerGraphics/preparation.md#231-L-Systems)节中有更详细的描述。

## 1.2.2 Parametric Approach

Honda是第一个提出通过使用多个简单参数来表示树木的[Hon71](http://www.speedtree.com/)。Aono和Knii采用了这种方法来生成3D模型[AK 84](http://dx.doi.org/10.1109/MCG.1984.276141)。Oppenheimer也开始采用这种方法[Opp 86](moz-extension://893a0fe3-8072-40ae-b8cb-cd873af8d934/content_scripts/pdfjs/web/viewer.html#cite.0@oppen),尽管两者仍然使用基于分形的结构，从而产生了一些不现实的模型。

Weber和Penn后来为树模型引入了更为全面的参数描述，并取得了令人印象深刻的结果[Wp 95](http://www.cs.duke.edu/courses/cps124/spring08/assign/07_papers/p119-weber.pdf)。他们的系统递归地构建一个基于数字输入参数列表的3D模型。现有的实现，例如Arbaro[Die 15](http://arbaro.sourceforge.net/),一个独立java应用程序，输出到标准3D文件格式，以及Sapling Tree插件[Hal](https://en.blender.org/index.php/Extensions:2.6/Py/Scripts/Curve/Sapling_Tree),一个blender插件，都是相当成功的。

我选择了基于Weber和Penn的方法来实现一个系统，考虑到它的相对易用性和令人印象深刻的视觉效果。第[2.3.2](https://github.com/BlenderCN/blenderTutorial/blob/master/ProceduralGenerationOfTreeModelsForUseInComputerGraphics/preparation.md#232-Weber-and-Penn)节概述了Weber和Penn的模型，附录c概述了所有参数。

## 1.2.3 Modular Approach

另一种最近的方法是从许多较小的块构建树模型，通常是从真实的树中手工建模或3D扫描。这些块的排列和合并方式使得每个连接都是连续的，因此得到的完整的树模型在视觉上是令人愉悦的。这允许具有许多复杂元素的非常详细和视觉感兴趣的模型，但将生成限制在手动获得的树段上。

使用这种方法[Xie+15](https://www.cs.bgu.ac.il/~asharf/tree.pdf)已经取得了令人印象深刻的结果,而用于Blender的[Modular Tree](https://github.com/MaximeHerpin/modular_tree)插件使用了这种方法的许多方面，但我们决定不关注这种方法，因为它需要一个重要的建模/扫描树部件库，这是我无法轻易获得的。

## 1.2.4 Space Colonisation

Runions等人最近提出了一个完全不同的系统，这涉及到使用space colonisation算法[RLP07](http://algorithmicbotany.org/papers/colonization.egwnp2007.pdf)对树木进行建模。该算法采用了植物信息的生长过程，对封闭的包络进行了定植，并由Palubicki等人对其进行了极大的改进[Pal+09](http://algorithmicbotany.org/papers/selforg.sig2009.small.pdf)

自那时以来，已经有了许多成功的实现，如[TreeSketch](http://algorithmicbotany.org/papers/TreeSketch.SBM2012.large.pdf)和Blender的[SpaceTree](https://github.com/varkenvarken/spacetree)插件。使用这种方法产生的结果在现实上肯定可以与Weber和Penn的方法相媲美，但由于时间限制，我决定在这个项目中不采用space colonisation。

## 1.2.5 Botanical Simulation

一种不太突出的方法是试图精确地模拟树的生长过程，以便创建一个三维模型。De Reffye等人这一方法取得了相对成功[Ref+88](http://doi.acm.org/10.1145/54852.378505),尽管最近在这方面的工作很少。这很可能是由于模拟增长过程的计算复杂性，当可以用更简单的模型，如上面提出的那些模型实现可比较的视觉结果时。因此，我决定不探讨这一方法。
