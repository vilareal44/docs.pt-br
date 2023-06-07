---
title: Como executar a inicialização lenta de objetos
description: Consulte como executar a inicialização lenta de objetos usando a classe System. Lazy <T> . A inicialização lenta significa que os objetos não serão criados se nunca forem necessários.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- lazy initialization in .NET, how to perform
ms.assetid: 8cd68620-dcc3-4f20-8835-c728a6820e71
ms.openlocfilehash: dbee0d8a5c3075ad7429feb92b87a566fdd35454
ms.sourcegitcommit: 0fa2b7b658bf137e813a7f4d09589d64c148ebf5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/14/2020
ms.locfileid: "86309723"
---
# <a name="how-to-perform-lazy-initialization-of-objects"></a>Como executar a inicialização lenta de objetos
A classe <xref:System.Lazy%601?displayProperty=nameWithType> simplifica o trabalho de inicialização lenta e instanciação de objetos. Inicializando objetos de maneira lenta, você poderá evitar a necessidade de criá-los se eles nunca forem necessários ou você poderá adiar sua inicialização até que eles sejam acessados pela primeira vez. Para obter mais informações, veja [Inicialização lenta](lazy-initialization.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como inicializar um valor com <xref:System.Lazy%601>. Suponha que a variável lenta talvez não seja necessária, dependendo de algum outro código que define a variável `someCondition` como true ou false.  
  
```vb  
Dim someCondition As Boolean = False  
  
Sub Main()  
    'Initializing a value with a big computation, computed in parallel  
    Dim _data As Lazy(Of Integer) = New Lazy(Of Integer)(Function()  
                                                             Dim result =  
                                                                 ParallelEnumerable.Range(0, 1000).  
                                                                 Aggregate(Function(x, y)  
                                                                               Return x + y  
                                                                           End Function)  
                                                             Return result  
                                                         End Function)  
  
    '  do work that may or may not set someCondition to True  
    ' ...  
    '  Initialize the data only if needed  
    If someCondition = True Then  
  
        If (_data.Value > 100) Then  
  
            Console.WriteLine("Good data")  
        End If  
    End If  
End Sub  
```  
  
```csharp  
  static bool someCondition = false;
  //Initializing a value with a big computation, computed in parallel  
  Lazy<int> _data = new Lazy<int>(delegate  
  {  
      return ParallelEnumerable.Range(0, 1000).  
          Select(i => Compute(i)).Aggregate((x,y) => x + y);  
  }, LazyExecutionMode.EnsureSingleThreadSafeExecution);  
  
  // Do some work that may or may not set someCondition to true.  
  //  ...  
  // Initialize the data only if necessary  
  if (someCondition)  
  {  
    if (_data.Value > 100)  
      {  
          Console.WriteLine("Good data");  
      }  
  }  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar a classe <xref:System.Threading.ThreadLocal%601?displayProperty=nameWithType> para inicializar um tipo que é visível somente para a instância atual do objeto no thread atual.  
  
 [!code-csharp[CDS#13](../../../samples/snippets/csharp/VS_Snippets_Misc/cds/cs/cds2.cs#13)]
 [!code-vb[CDS#13](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds/vb/lazyhowto.vb#13)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Threading.LazyInitializer?displayProperty=nameWithType>
- [Inicialização lenta](lazy-initialization.md)
