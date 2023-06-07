---
title: 'Como: dividir um arquivo em vários arquivos usando grupos (LINQ)'
ms.date: 07/20/2015
ms.assetid: 5e8b2a2b-0b1d-4933-8a2b-03e91dfaf77f
ms.openlocfilehash: f6b11ab4b4fe11dbf1cb4cf07654b5ef3f6785ad
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397707"
---
# <a name="how-to-split-a-file-into-many-files-by-using-groups-linq-visual-basic"></a>Como dividir um arquivo em vários arquivos usando grupos (LINQ) (Visual Basic)

Este exemplo mostra uma maneira de mesclar o conteúdo de dois arquivos e, em seguida, criar um conjunto de novos arquivos que organizam os dados em uma nova forma.

### <a name="to-create-the-data-files"></a>Para criar os arquivos de dados

1. Copie esses nomes em um arquivo de texto chamado names1.txt e salve-o na sua pasta do projeto:

    ```text
    Bankov, Peter
    Holm, Michael
    Garcia, Hugo
    Potra, Cristina
    Noriega, Fabricio
    Aw, Kam Foo
    Beebe, Ann
    Toyoshima, Tim
    Guy, Wey Yuan
    Garcia, Debra
    ```

2. Copie esses nomes em um arquivo de texto chamado names2.txt e salve-o na sua pasta do projeto: observe que os dois arquivos têm alguns nomes em comum.

    ```text
    Liu, Jinghao
    Bankov, Peter
    Holm, Michael
    Garcia, Hugo
    Beebe, Ann
    Gilchrist, Beth
    Myrcha, Jacek
    Giakoumakis, Leo
    McLin, Nkenge
    El Yassir, Mehdi
    ```

## <a name="example"></a>Exemplo

```vb
Class SplitWithGroups

    Shared Sub Main()

        Dim fileA As String() = System.IO.File.ReadAllLines("../../../names1.txt")
        Dim fileB As String() = System.IO.File.ReadAllLines("../../../names2.txt")

        ' Concatenate and remove duplicate names based on
        Dim mergeQuery As IEnumerable(Of String) = fileA.Union(fileB)

        ' Group the names by the first letter in the last name
        Dim groupQuery = From name In mergeQuery
                     Let n = name.Split(New Char() {","})
                     Order By n(0)
                     Group By groupKey = n(0)(0)
                     Into groupName = Group

        ' Create a new file for each group that was created
        ' Note that nested foreach loops are required to access
        ' individual items with each group.
        For Each gGroup In groupQuery
            Dim fileName As String = "..'..'..'testFile_" & gGroup.groupKey & ".txt"
            Dim sw As New System.IO.StreamWriter(fileName)
            Console.WriteLine(gGroup.groupKey)
            For Each item In gGroup.groupName
                Console.WriteLine("   " & item.name)
                sw.WriteLine(item.name)
            Next
            sw.Close()
        Next

        ' Keep console window open in debug mode.
        Console.WriteLine("Files have been written. Press any key to exit.")
        Console.ReadKey()

    End Sub
End Class
' Console Output:
' A
'    Aw, Kam Foo
' B
'    Bankov, Peter
'    Beebe, Ann
' E
'    El Yassir, Mehdi
' G
'    Garcia, Hugo
'    Garcia, Debra
'    Giakoumakis, Leo
'    Gilchrist, Beth
'    Guy, Wey Yuan
' H
'    Holm, Michael
' L
'    Liu, Jinghao
' M
'    McLin, Nkenge
'    Myrcha, Jacek
' N
'    Noriega, Fabricio
' P
'    Potra, Cristina
' T
'    Toyoshima, Tim
```

O programa grava um arquivo separado para cada grupo na mesma pasta que os arquivos de dados.

## <a name="compile-the-code"></a>Compilar o código

Crie um projeto de aplicativo de console Visual Basic, com uma `Imports` instrução para o namespace System. Linq.

## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (Visual Basic)](linq-and-strings.md)
- [LINQ e diretórios de arquivos (Visual Basic)](linq-and-file-directories.md)
