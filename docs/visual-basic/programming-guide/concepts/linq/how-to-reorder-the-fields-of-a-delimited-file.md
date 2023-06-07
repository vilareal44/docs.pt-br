---
title: 'Como: reordenar os campos de um arquivo delimitado (LINQ)'
ms.date: 07/20/2015
ms.assetid: c451c7db-663b-4daf-b8ba-a2093095d672
ms.openlocfilehash: 6f87374978425e0d51542c6eceda23697d7a3a67
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397889"
---
# <a name="how-to-reorder-the-fields-of-a-delimited-file-linq-visual-basic"></a>Como: reordenar os campos de um arquivo delimitado (LINQ) (Visual Basic)

Um CSV (arquivo de valores separados por vírgula) é um arquivo de texto que é frequentemente usado para armazenar dados de planilha ou outros dados de tabela que são representados por linhas e colunas. Ao usar o método <xref:System.String.Split%2A> para separar os campos, é muito fácil consultar e manipular arquivos CSV usando LINQ. Na verdade, a mesma técnica pode ser usada para reordenar as partes de qualquer linha estruturada de texto. Ela não é limitada a arquivos CSV.

No exemplo a seguir, suponha que as três colunas representam o "sobrenome", o "nome" e a "ID" dos alunos. Os campos estão em ordem alfabética com base nos sobrenomes dos alunos. A consulta gera uma nova sequência, na qual a coluna ID é exibida em primeiro, seguida por uma segunda coluna que combina o nome e o sobrenome do aluno. As linhas são reordenadas acordo com o campo ID. Os resultados são salvos em um novo arquivo e os dados originais não são modificados.

### <a name="to-create-the-data-file"></a>Para criar o arquivo de dados

1. Copie as seguintes linhas em um arquivo de texto sem formatação denominado spreadsheet1.csv. Salve o arquivo na pasta do seu projeto.

    ```csv
    Adams,Terry,120
    Fakhouri,Fadi,116
    Feng,Hanying,117
    Garcia,Cesar,114
    Garcia,Debra,115
    Garcia,Hugo,118
    Mortensen,Sven,113
    O'Donnell,Claire,112
    Omelchenko,Svetlana,111
    Tucker,Lance,119
    Tucker,Michael,122
    Zabokritski,Eugene,121
    ```

## <a name="example"></a>Exemplo

```vb
Class CSVFiles

    Shared Sub Main()

        ' Create the IEnumerable data source.
        Dim lines As String() = System.IO.File.ReadAllLines("../../../spreadsheet1.csv")

        ' Execute the query. Put field 2 first, then
        ' reverse and combine fields 0 and 1 from the old field
        Dim lineQuery = From line In lines
                        Let x = line.Split(New Char() {","})
                        Order By x(2)
                        Select x(2) & ", " & (x(1) & " " & x(0))

        ' Execute the query and write out the new file. Note that WriteAllLines
        ' takes a string array, so ToArray is called on the query.
        System.IO.File.WriteAllLines("../../../spreadsheet2.csv", lineQuery.ToArray())

        ' Keep console window open in debug mode.
        Console.WriteLine("Spreadsheet2.csv written to disk. Press any key to exit")
        Console.ReadKey()
    End Sub
End Class
' Output to spreadsheet2.csv:
' 111, Svetlana Omelchenko
' 112, Claire O'Donnell
' 113, Sven Mortensen
' 114, Cesar Garcia
' 115, Debra Garcia
' 116, Fadi Fakhouri
' 117, Hanying Feng
' 118, Hugo Garcia
' 119, Lance Tucker
' 120, Terry Adams
' 121, Eugene Zabokritski
' 122, Michael Tucker
```

## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (Visual Basic)](linq-and-strings.md)
- [LINQ e diretórios de arquivos (Visual Basic)](linq-and-file-directories.md)
- [Como gerar XML a partir de arquivos CSV](how-to-generate-xml-from-csv-files.md)
