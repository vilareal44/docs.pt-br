---
title: Carregando e usando tipos dinamicamente
description: Carregar e usar dinamicamente os tipos no .NET. Use a reflexão, que fornece a infraestrutura usada pelos compiladores de linguagem para implementar a ligação tardia implícita.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- late binding, about late binding
- early binding
- dynamically loading and using types
- implicit late binding
- reflection, dynamically using types
ms.assetid: db985bec-5942-40ec-b13a-771ae98623dc
ms.openlocfilehash: 39a4a9a2ff77cb900db7f39a55dc17a5b8c62cf3
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475080"
---
# <a name="dynamically-loading-and-using-types"></a>Carregando e usando tipos dinamicamente
A reflexão fornece a infraestrutura usada pelos compiladores de linguagem para implementar a associação tardia implícita. Associação é o processo de localizar a declaração (ou seja, a implementação) que corresponde a um tipo especificado exclusivamente. Quando esse processo ocorre no tempo de execução em vez do tempo de compilação, ele é chamado de associação tardia. O Visual Basic permite que você use associação tardia implícita em seu código. O compilador do Visual Basic chama um método auxiliar que usa a reflexão para obter o tipo de objeto. Os argumentos passados para o método auxiliar fazem com que o método apropriado seja invocado no tempo de execução. Esses argumentos são a instância (um objeto) na qual o método será invocado, o nome do método invocado (uma cadeia de caracteres) e os argumentos passados para o método invocado (uma matriz de objetos).  
  
 No exemplo a seguir, o compilador do Visual Basic usa reflexão implicitamente para chamar um método em um objeto cujo tipo não é conhecido no tempo de compilação. Uma classe **HelloWorld** tem um método **PrintHello** que imprime “Hello World” concatenado com algum texto que é passado para o método **PrintHello**. O método **PrintHello** chamado neste exemplo é na verdade um <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType>; o código do Visual Basic permite que o método **PrintHello** seja invocado como se o tipo de objeto (helloObj) fosse conhecido durante o tempo de compilação (associação antecipada) em vez de no tempo de execução (associação tardia).  
  
```vb
Module Hello  
    Sub Main()  
        ' Sets up the variable.  
        Dim helloObj As Object  
        ' Creates the object.  
        helloObj = new HelloWorld()  
        ' Invokes the print method as if it was early bound  
        ' even though it is really late bound.  
        helloObj.PrintHello("Visual Basic Late Bound")  
    End Sub  
End Module  
```  
  
## <a name="custom-binding"></a>Associação personalizado  
 Além de ser usado implicitamente por compiladores para associação tardia, a reflexão pode ser usada explicitamente no código para realizar a associação tardia.  
  
 O [Common Language Runtime](../../standard/clr.md) dá suporte a várias linguagens de programação e as regras de associação dessas linguagens são diferentes. No caso de associação antecipada, os geradores de código podem controlar essa associação completamente. No entanto, na associação tardia por meio de reflexão, a associação deve ser controlada pela associação personalizada. A classe <xref:System.Reflection.Binder> fornece controle personalizado do membro de seleção e da invocação.  
  
 Usando a associação personalizada, você pode carregar um assembly no tempo de execução, obter informações sobre tipos nesse assembly, especificar o tipo que você deseja e, em seguida, invocar métodos ou acessar campos ou propriedades nesse tipo. Essa técnica é útil se você não souber o tipo de um objeto no tempo de compilação, como quando o tipo de objeto depende da entrada do usuário.  
  
 O exemplo a seguir demonstra um associador personalizado simples que não fornece nenhuma conversão de tipo de argumento. O código de `Simple_Type.dll` precede o exemplo principal. Verifique se você compilou `Simple_Type.dll` e incluiu uma referência a ele no projeto no tempo de compilação.  
  
 [!code-cpp[Conceptual.Types.Dynamic#1](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.dynamic/cpp/source1.cpp#1)]
 [!code-csharp[Conceptual.Types.Dynamic#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.dynamic/cs/source1.cs#1)]
 [!code-vb[Conceptual.Types.Dynamic#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.dynamic/vb/source1.vb#1)]  
  
### <a name="invokemember-and-createinstance"></a>InvokeMember e CreateInstance  
 Use <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType> para invocar um membro de um tipo. Os métodos **CreateInstance** de várias classes, como <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> e <xref:System.Reflection.Assembly.CreateInstance%2A?displayProperty=nameWithType>, são formas especializadas de **InvokeMember** que criam novas instâncias do tipo especificado. A classe **Binder** é usada para resolução de sobrecarga e coerção de argumento nesses métodos.  
  
 O exemplo a seguir mostra três combinações possíveis de coerção de argumento (conversão de tipo) e seleção de membro. No caso 1, não é necessária nenhuma coerção de argumento ou seleção de membro. No caso 2, somente a seleção de membro é necessária. No caso 3, somente a coerção de argumento é necessária.  
  
 [!code-cpp[Conceptual.Types.Dynamic#2](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.types.dynamic/cpp/source2.cpp#2)]
 [!code-csharp[Conceptual.Types.Dynamic#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.types.dynamic/cs/source2.cs#2)]
 [!code-vb[Conceptual.Types.Dynamic#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.types.dynamic/vb/source2.vb#2)]  
  
 A resolução de sobrecarga é necessária quando mais de um membro com o mesmo nome está disponível. Os métodos <xref:System.Reflection.Binder.BindToMethod%2A?displayProperty=nameWithType> e <xref:System.Reflection.Binder.BindToField%2A?displayProperty=nameWithType> são usados para resolver a associação para um único membro. O **Binder.BindToMethod** também fornece a resolução de propriedade por meio de dos acessadores de propriedade **get** e **set**.  
  
 O **BindToMethod** retorna o <xref:System.Reflection.MethodBase> a ser invocado ou uma referência nula (**Nothing** no Visual Basic) se tal invocação não for possível. O valor retornado de **MethodBase** não precisa ser um dos contidos no parâmetro *match*, embora isso seja o comum.  
  
 Quando houver argumentos ByRef, o chamador poderá tentar recuperá-los. Portanto, **Binder** permite a um cliente mapear a matriz de argumentos para sua forma original se **BindToMethod** tiver manipulado a matriz de argumento. Para fazer isso, é necessário garantir ao chamador que a ordem dos argumentos se mantém inalterada. Quando os argumentos são passados por nome, o **Binder** reorganiza a matriz de argumentos e é isso que o chamador vê. Para obter mais informações, consulte <xref:System.Reflection.Binder.ReorderArgumentArray%2A?displayProperty=nameWithType>.  
  
 O conjunto de membros disponíveis aqueles definidos no tipo ou em qualquer tipo base. Se <xref:System.Reflection.BindingFlags> for especificado, membros de qualquer acessibilidade serão retornados ao conjunto. Se **BindingFlags.NonPublic** não for especificado, o associador deverá impor regras de acessibilidade. Ao especificar o sinalizador de associação **Public** ou **NonPublic**, você deverá especificar também o sinalizador de associação **Instance** ou **Static** ou nenhum membro será retornado.  
  
 Se houver somente um membro do nome fornecido, nenhum retorno de chamada é necessário e a associação é realizada nesse método. O caso 1 do exemplo de código ilustra este ponto: apenas um método **PrintBob** está disponível e, portanto, nenhum retorno de chamada é necessário.  
  
 Se houver mais de um membro do conjunto disponível, todos esses métodos são passados para **BindToMethod**, que seleciona o método apropriado e o retorna. No caso 2 do exemplo de código, há dois métodos chamados **PrintValue**. O método apropriado é selecionado pela chamada para **BindToMethod**.  
  
 O <xref:System.Reflection.Binder.ChangeType%2A> executa a coerção de argumento (conversão de tipo), que converte os argumentos reais para o tipo dos argumentos formais do método selecionado. **ChangeType** é chamado para cada argumento, mesmo se os tipos de corresponderem exatamente.  
  
 No caso 3 do exemplo de código, um argumento real do tipo **String** com um valor de “5.5” é passado para um método com um argumento formal do tipo **Double**. Para que a invocação seja bem-sucedida, o valor da cadeia de caracteres “5.5” deve ser convertido em um valor duplo. O **ChangeType** executa essa conversão.  
  
 O **ChangeType** executa apenas [coerções de expansão](../../standard/base-types/type-conversion.md) ou sem perdas, conforme mostrado na tabela a seguir.  
  
|Tipo de origem|Tipo de destino|  
|-----------------|-----------------|  
|Qualquer tipo|O tipo base|  
|Qualquer tipo|Interface implementada|  
|Char|UInt16, UInt32, Int32, UInt64, Int64, Single e Double|  
|Byte|Char, UInt16, Int16, UInt32, Int32, UInt64, Int64, Single e Double|  
|SByte|Int16, Int32, Int64, Single e Double|  
|UInt16|UInt32, Int32, UInt64, Int64, Single e Double|  
|Int16|Int32, Int64, Single e Double|  
|UInt32|UInt64, Int64, Single e Double|  
|Int32|Int64, Single e Double|  
|UInt64|Single, Double|  
|Int64|Single, Double|  
|Single|Double|  
|Tipo Nonreference|Tipo de referência|  
  
 A classe <xref:System.Type> tem métodos **Get** que usam parâmetros do tipo **Binder** para resolver referências a um determinado membro. <xref:System.Type.GetConstructor%2A?displayProperty=nameWithType>, <xref:System.Type.GetMethod%2A?displayProperty=nameWithType> e <xref:System.Type.GetProperty%2A?displayProperty=nameWithType> pesquisam um determinado membro do tipo atual, fornecendo informações de assinatura para esse membro. <xref:System.Reflection.Binder.SelectMethod%2A?displayProperty=nameWithType> e <xref:System.Reflection.Binder.SelectProperty%2A?displayProperty=nameWithType> são chamados de volta para selecionar as informações de determinada assinatura dos métodos apropriados.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Type.InvokeMember%2A?displayProperty=nameWithType>
- <xref:System.Reflection.Assembly.Load%2A?displayProperty=nameWithType>
- [Exibindo informações de tipo](viewing-type-information.md)
- [Conversão de tipos no .NET Framework](../../standard/base-types/type-conversion.md)
