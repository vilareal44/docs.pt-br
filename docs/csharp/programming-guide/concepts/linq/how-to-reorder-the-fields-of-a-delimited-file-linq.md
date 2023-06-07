---
title: Como reordenar os campos de um arquivo delimitado (LINQ) (C#)
description: Saiba como reorganizar os campos em um arquivo. csv no LINQ em C#. O exemplo altera as ordens de colunas, mescla em colunas e classifica as linhas por um valor de coluna.
ms.date: 07/20/2015
ms.assetid: 4e62d82c-61b7-4f18-b9a1-86723746d7d2
ms.openlocfilehash: 3ebc56b418d2732a296896a19d770136a56e2fbb
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87103405"
---
# <a name="how-to-reorder-the-fields-of-a-delimited-file-linq-c"></a>Como reordenar os campos de um arquivo delimitado (LINQ) (C#)
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
  
```csharp  
class CSVFiles  
{  
    static void Main(string[] args)  
    {  
        // Create the IEnumerable data source  
        string[] lines = System.IO.File.ReadAllLines(@"../../../spreadsheet1.csv");  
  
        // Create the query. Put field 2 first, then  
        // reverse and combine fields 0 and 1 from the old field  
        IEnumerable<string> query =  
            from line in lines  
            let x = line.Split(',')  
            orderby x[2]  
            select x[2] + ", " + (x[1] + " " + x[0]);  
  
        // Execute the query and write out the new file. Note that WriteAllLines  
        // takes a string[], so ToArray is called on the query.  
        System.IO.File.WriteAllLines(@"../../../spreadsheet2.csv", query.ToArray());  
  
        Console.WriteLine("Spreadsheet2.csv written to disk. Press any key to exit");  
        Console.ReadKey();  
    }  
}  
/* Output to spreadsheet2.csv:  
111, Svetlana Omelchenko  
112, Claire O'Donnell  
113, Sven Mortensen  
114, Cesar Garcia  
115, Debra Garcia  
116, Fadi Fakhouri  
117, Hanying Feng  
118, Hugo Garcia  
119, Lance Tucker  
120, Terry Adams  
121, Eugene Zabokritski  
122, Michael Tucker  
 */  
```  
  
## <a name="compiling-the-code"></a>Compilando o código  
Criar um projeto de aplicativo de console em C# com diretivas `using` para os namespaces System.Linq e System.IO.
  
## <a name="see-also"></a>Confira também

- [LINQ e cadeias de caracteres (C#)](./linq-and-strings.md)
- [LINQ e diretórios de arquivos (C#)](./linq-and-file-directories.md)
- [Como gerar XML de arquivos CSV (C#)](./how-to-generate-xml-from-csv-files.md)
