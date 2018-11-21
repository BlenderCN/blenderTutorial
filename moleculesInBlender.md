首先，让我们看看我们将如何指定化学结构。化学世界充斥着可怕的文件格式，并且由于传统标准的普遍弹性，我们需要考虑它们。
然而，在我们开始之前，让我们决定分子数据的良好扩展表示。编码中使用的常用表示称为Javascript Objiect Notation或json，
它非常擅长简洁地表示任意数据。如果你以前从未听说过，我强烈建议你继续阅读它。这是
[wiki](http://en.wikipedia.org/wiki/JSON)
由于分子是任意数据的一个子集，json应该适合我们的需要。我们的drawer需要原子元素类型和3D位置，以及键连接和顺序。
因此，让我们使用如下所示的东西（例子是乙烷）

    {
        "atoms": [
            { "element": "C", "location": [ 0.252, -0.116, -0.704 ] },
            { "element": "C", "location": [ -0.252, 0.116, 0.704 ] }
        ],
        "bonds": [
            { "atoms": [ 0, 1 ], "order": 1 }
        ]
    }

这很容易在Python中解析

    import json
    data =json.loads(string)

在这两行中，我们现在有一个Python数据结构！如果我们想要打印第一个原子的y坐标，我们可以这样做

    pirnt（data["atoms"][0]["location"][1]).

这将在以后派上用场。

现在，回到手头的问题。我们希望为数百种文件格式提供支持（molfiles,smiles,sdf,cif,pdb等，但解析他们都是绝对的噩梦。
在这篇文章那的先前版本中，我建议直接解析molfile。我现在收回我的话。那太愚蠢了。让我们使用Pybel，一个
[openbabel](http://openbabel.org/wiki/Main_Page)
库的Python包。下载它，然后我们可以在一行代码中统一所有这些文件格式。

    molecule = pybel.readstring(format,sting).

如果输入文件格式中没有位置数据，我们可以通过简单地调用来生成它。

    molecule.make3D()
    
现在，我们可以将这种统一格式转换为json，我们将拥有统一的数据格式，而无需再编写任何解析逻辑

    def molecule_to_json(molecule):
        """Converts an OpenBabel molecule to json for use in Blender."""

        # Save atom element type and 3D location.
        atoms = [{"element": atom.type,
                "location": atom.coords}
                for atom in molecule.atoms]

        # Save number of bonds and indices of endpoint atoms
        bonds = [{"atoms": [b.GetBeginAtom().GetIndex(), b.GetEndAtom().GetIndex()],
                "order": b.GetBondOrder()}
                for b in openbabel.OBMolBondIter(molecule.OBMol)]

        return json.dumps({"atoms": atoms, "bonds": bonds})


    

[link](https://patrickfuller.github.io/molecules-from-smiles-molfiles-in-blender/)
