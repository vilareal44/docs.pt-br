---
title: 'Como: consultar metadados de um assembly com reflexão (LINQ)'
ms.date: 07/20/2015
ms.assetid: 53caa336-ab83-4181-b0f6-5c87c5f9e4ee
ms.openlocfilehash: a4f73bd2c8c01cbf92fac67991f01a1cb3dee932
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396448"
---
# <a name="how-to-query-an-assemblys-metadata-with-reflection-linq-visual-basic"></a>Como consultar metadados de um assembly com reflexão (LINQ) (Visual Basic)
O exemplo a seguir mostra como o LINQ pode ser usado com a reflexão para recuperar metadados específicos sobre os métodos que correspondem a um critério de pesquisa especificado. Nesse caso, a consulta localizará os nomes de todos os métodos no assembly que retornam tipos enumeráveis como matrizes.  
  
## <a name="example"></a>Exemplo  
  
```vb
Imports System.Linq
Imports System.Reflection  

Module Module1  
    Sub Main()  
        Dim asmbly As Assembly =
            Assembly.Load("System.Core, Version=3.5.0.0, Culture=neutral, PublicKeyToken= b77a5c561934e089")  
  
        Dim pubTypesQuery = From type In asmbly.GetTypes()
                            Where type.IsPublic
                            From method In type.GetMethods()
                            Where method.ReturnType.IsArray = True
                            Let name = method.ToString()
                            Let typeName = type.ToString()
                            Group name By typeName Into methodNames = Group  
  
        Console.WriteLine("Getting ready to iterate")  
        For Each item In pubTypesQuery  
            Console.WriteLine(item.methodNames)  
  
            For Each type In item.methodNames  
                Console.WriteLine(" " & type)  
            Next  
        Next  
        Console.WriteLine("Press any key to exit... ")  
        Console.ReadKey()  
    End Sub  
End Module  
```  
  
O exemplo usa o método <xref:System.Reflection.Assembly.GetTypes%2A?displayProperty=nameWithType> para retornar uma matriz de tipos no assembly especificado. O filtro de [cláusula WHERE](../../../language-reference/queries/where-clause.md) é aplicado para que apenas tipos públicos sejam retornados. Para cada tipo de público, uma subconsulta é gerada usando a matriz <xref:System.Reflection.MethodInfo> que é retornada da chamada <xref:System.Type.GetMethods%2A?displayProperty=nameWithType>. Esses resultados são filtrados para retornar apenas os métodos cujo tipo de retorno é uma matriz ou um tipo que implementa <xref:System.Collections.Generic.IEnumerable%601>. Por fim, esses resultados são agrupados usando o nome do tipo como uma chave.  
  
## <a name="see-also"></a>Confira também

- [LINQ to Objects (Visual Basic)](linq-to-objects.md)
