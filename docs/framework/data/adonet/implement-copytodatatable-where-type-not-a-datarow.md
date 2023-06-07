---
title: Como implementar CopyToDataTable<T> no qual o tipo genérico T não é um DataRow
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: b27b52cf-6172-485f-a75c-70ff9c5a2bd4
ms.openlocfilehash: a1427747d03f01e52f1ee7ad1fc11d47d310edbe
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590599"
---
# <a name="how-to-implement-copytodatatablet-where-the-generic-type-t-is-not-a-datarow"></a>Como implementar CopyToDataTable\<T> no qual o tipo genérico T não é um DataRow
O objeto de <xref:System.Data.DataTable> é freqüentemente usado para associação de dados. O método <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> recebe os resultados de uma consulta e copia os dados em um <xref:System.Data.DataTable>, que podem ser usados para vinculação de dados. Os métodos de <xref:System.Data.DataTableExtensions.CopyToDataTable%2A> , no entanto, somente operam em uma fonte de <xref:System.Collections.Generic.IEnumerable%601> onde o parâmetro genérico `T` é do tipo <xref:System.Data.DataRow>. Embora isso é útil, não permite que as tabelas são criadas de uma sequência de tipos escalares, as consultas que o projeto tipos anônimos, ou de consultas que executam a tabela join.  
  
 Este tópico descreve como implementar dois métodos personalizados de extensão de `CopyToDataTable<T>` que aceita um parâmetro genérico `T` de um tipo diferente <xref:System.Data.DataRow>. A lógica para criar <xref:System.Data.DataTable> de uma fonte de <xref:System.Collections.Generic.IEnumerable%601> está contida na classe de `ObjectShredder<T>` , que é empacotada em dois métodos sobrecarregados de extensão de `CopyToDataTable<T>` . O método de `Shred` da classe de `ObjectShredder<T>` retorna <xref:System.Data.DataTable> preenchido e três aceita parâmetros de entrada: uma fonte de <xref:System.Collections.Generic.IEnumerable%601> , um <xref:System.Data.DataTable>, e uma enumeração de <xref:System.Data.LoadOption> . O esquema inicial de <xref:System.Data.DataTable> retornado é baseado no esquema de tipo `T`. Se uma tabela existente é fornecida como entrada, o esquema deve ser consistente com o esquema de tipo `T`. Cada propriedade pública e campo de tipo `T` são convertidos na tabela a <xref:System.Data.DataColumn> retornado. Se a sequência de origem contiver um tipo derivado de `T`, o esquema retornado da tabela é expandido para todas as propriedades públicas e campos adicionais.  
  
 Para obter exemplos de como usar os métodos `CopyToDataTable<T>` personalizados, confira [Criando uma DataTable com base em uma consulta](creating-a-datatable-from-a-query-linq-to-dataset.md).  
  
### <a name="to-implement-the-custom-copytodatatablet-methods-in-your-application"></a>Para implementar os métodos CopyToDataTable personalizados \<T> em seu aplicativo  
  
1. Implementar a classe de `ObjectShredder<T>` para criar <xref:System.Data.DataTable> de uma fonte de <xref:System.Collections.Generic.IEnumerable%601> :  
  
     [!code-csharp[DP Custom CopyToDataTable Examples#ObjectShredderClass](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#objectshredderclass)]
     [!code-vb[DP Custom CopyToDataTable Examples#ObjectShredderClass](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#objectshredderclass)]  

    O exemplo anterior pressupõe que as propriedades e os campos dos `DataColumn` não são tipos de valor anuláveis. Para lidar com propriedades e campos com tipos de valores anuláveis, use o seguinte código:

    ```csharp
    // Nullable-aware code for properties.
    DataColumn dc = table.Columns.Contains(p.Name) ? table.Columns[p.Name] : table.Columns.Add(p.Name, Nullable.GetUnderlyingType(p.PropertyType) ?? p.PropertyType);

    // Nullable-aware code for fields.
    DataColumn dc = table.Columns.Contains(f.Name) ? table.Columns[f.Name] : table.Columns.Add(f.Name, Nullable.GetUnderlyingType(f.FieldType) ?? f.FieldType);
    ```

2. Implementar métodos personalizados de extensão de `CopyToDataTable<T>` em uma classe:  
  
     [!code-csharp[DP Custom CopyToDataTable Examples#CustomCopyToDataTableMethods](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/CS/Program.cs#customcopytodatatablemethods)]
     [!code-vb[DP Custom CopyToDataTable Examples#CustomCopyToDataTableMethods](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP Custom CopyToDataTable Examples/VB/Module1.vb#customcopytodatatablemethods)]  
  
3. Adicionar a classe de `ObjectShredder<T>` e métodos de extensão de `CopyToDataTable<T>` ao seu aplicativo.  
  
```vb  
Module Module1  
    Sub Main()  
        ' Your application code using CopyToDataTable<T>.  
    End Sub  
End Module  
  
Public Module CustomLINQtoDataSetMethods  
…  
End Module  
  
Public Class ObjectShredder(Of T)  
…  
End Class
```
  
```csharp
class Program  
{  
    static void Main(string[] args)  
    {  
        // Your application code using CopyToDataTable<T>.  
    }  
}  
public static class CustomLINQtoDataSetMethods  
{  
…  
}  
public class ObjectShredder<T>  
{  
…  
}  
```
  
## <a name="see-also"></a>Consulte também

- [Criando um DataTable de uma consulta](creating-a-datatable-from-a-query-linq-to-dataset.md)
- [Guia de programação](programming-guide-linq-to-dataset.md)
