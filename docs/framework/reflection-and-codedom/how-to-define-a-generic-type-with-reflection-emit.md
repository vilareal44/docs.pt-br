---
title: 'Como: Definir um tipo genérico com a emissão de reflexão'
description: Consulte como definir um tipo genérico com emissão de reflexo. Crie um tipo genérico com dois parâmetros de tipo, aplique restrições de classe, restrições de interface e muito mais.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET Framework], reflection emit
- generics [.NET Framework], dynamic types
- reflection emit, generic types
ms.assetid: 07d5f01a-7b5b-40ea-9b15-f21561098fe4
ms.openlocfilehash: fe8fb731fd160ab87e5c65debf367a96bc0dea2a
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865119"
---
# <a name="how-to-define-a-generic-type-with-reflection-emit"></a>Como: Definir um tipo genérico com a emissão de reflexão
Este tópico mostra como criar um tipo genérico simples com dois parâmetros de tipo, como aplicar restrições de classe, restrições de interface e restrições especiais aos parâmetros de tipo e como criar membros que usam os parâmetros de tipo da classe como tipos de parâmetro e tipos de retorno.  
  
> [!IMPORTANT]
> Um método não é genérico apenas porque pertence a um tipo genérico e usa os parâmetros de tipo desse tipo. Um método será genérico somente se ele tiver sua própria lista de parâmetros de tipo. A maioria dos métodos em tipos genéricos não é genérica, como neste exemplo. Para obter um exemplo de emissão de um método genérico, consulte [Como definir um método genérico com a emissão de reflexão](how-to-define-a-generic-method-with-reflection-emit.md).  
  
### <a name="to-define-a-generic-type"></a>Para definir um tipo genérico  
  
1. Defina um assembly dinâmico chamado `GenericEmitExample1`. Neste exemplo, o assembly é executado e salvo no disco, portanto, <xref:System.Reflection.Emit.AssemblyBuilderAccess.RunAndSave?displayProperty=nameWithType> é especificado.  
  
     [!code-cpp[EmitGenericType#2](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#2)]
     [!code-csharp[EmitGenericType#2](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#2)]
     [!code-vb[EmitGenericType#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#2)]  
  
2. Defina um módulo dinâmico. Um assembly é composto por módulos executáveis. Para um assembly de modo único, o nome do módulo é igual ao nome do assembly e o nome do arquivo é o nome do módulo com a adição de uma extensão.  
  
     [!code-cpp[EmitGenericType#3](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#3)]
     [!code-csharp[EmitGenericType#3](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#3)]
     [!code-vb[EmitGenericType#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#3)]  
  
3. Defina uma classe. Nesse exemplo, a classe é chamada `Sample`.  
  
     [!code-cpp[EmitGenericType#4](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#4)]
     [!code-csharp[EmitGenericType#4](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#4)]
     [!code-vb[EmitGenericType#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#4)]  
  
4. Defina os parâmetros de tipo genérico de `Sample` passando uma matriz de cadeias de caracteres que contém os nomes dos parâmetros para o método <xref:System.Reflection.Emit.TypeBuilder.DefineGenericParameters%2A?displayProperty=nameWithType>. Isso torna a classe um tipo genérico. O valor retornado é uma matriz de objetos <xref:System.Reflection.Emit.GenericTypeParameterBuilder> que representam os parâmetros de tipo, que podem ser usados em seu código emitido.  
  
     No código a seguir, `Sample` se torna um tipo genérico om os parâmetros de tipo `TFirst` e `TSecond`. Para tornar o código mais fácil de ler, cada um <xref:System.Reflection.Emit.GenericTypeParameterBuilder> é colocado em uma variável com o mesmo nome que o parâmetro de tipo.  
  
     [!code-cpp[EmitGenericType#5](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#5)]
     [!code-csharp[EmitGenericType#5](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#5)]
     [!code-vb[EmitGenericType#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#5)]  
  
5. Adicione restrições especiais aos parâmetros de tipo. Neste exemplo, o parâmetro de tipo `TFirst` é restrito para tipos que têm construtores sem parâmetros e para tipos de referência.  
  
     [!code-cpp[EmitGenericType#6](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#6)]
     [!code-csharp[EmitGenericType#6](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#6)]
     [!code-vb[EmitGenericType#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#6)]  
  
6. Opcionalmente, adicione restrições de classe e interface aos parâmetros de tipo. Neste exemplo, o parâmetro de tipo `TFirst` é restrito a tipos que derivam da classe base representada pelo objeto <xref:System.Type> contido na variável `baseType` e que implementam as interfaces cujos tipos estão contidos nas variáveis `interfaceA` e `interfaceB`. Consulte o exemplo de código para a declaração e atribuição dessas variáveis.  
  
     [!code-cpp[EmitGenericType#7](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#7)]
     [!code-csharp[EmitGenericType#7](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#7)]
     [!code-vb[EmitGenericType#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#7)]  
  
7. Defina um campo. Neste exemplo, o tipo do campo é especificado pelo parâmetro de tipo `TFirst`. <xref:System.Reflection.Emit.GenericTypeParameterBuilder> deriva de <xref:System.Type>, portanto, você pode usar parâmetros de tipo genérico em qualquer lugar que um tipo pode ser usado.  
  
     [!code-cpp[EmitGenericType#21](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#21)]
     [!code-csharp[EmitGenericType#21](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#21)]
     [!code-vb[EmitGenericType#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#21)]  
  
8. Defina um método que usa os parâmetros de tipo do tipo genérico. Observe que esses métodos não são genéricos, a menos que tenham suas próprias listas de parâmetros de tipo. O código a seguir define um método `static` (`Shared` no Visual Basic) que usa uma matriz de `TFirst` e retorna um `List<TFirst>` (`List(Of TFirst)` no Visual Basic) que contém todos os elementos da matriz. Para definir esse método, é necessário criar o tipo `List<TFirst>` chamando <xref:System.Type.MakeGenericType%2A> na definição de tipo genérico, `List<T>`. (O `T` é omitido quando você usa o `typeof` operador ( `GetType` em Visual Basic) para obter a definição de tipo genérico.) O tipo de parâmetro é criado usando o <xref:System.Type.MakeArrayType%2A> método.  
  
     [!code-cpp[EmitGenericType#22](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#22)]
     [!code-csharp[EmitGenericType#22](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#22)]
     [!code-vb[EmitGenericType#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#22)]  
  
9. Emita o corpo do método. O corpo do método consiste em três opcodes que carregam a matriz de entrada para a pilha, chamem o construtor `List<TFirst>` que recebe `IEnumerable<TFirst>` (que realiza todo o trabalho de colocar os elementos de entrada na lista) e retorna (deixando o novo objeto <xref:System.Collections.Generic.List%601> na pilha). A parte difícil de emitir esse código é obter o construtor.  
  
     O método <xref:System.Type.GetConstructor%2A> não tem suporte em um <xref:System.Reflection.Emit.GenericTypeParameterBuilder>, portanto, não é possível obter o construtor de `List<TFirst>` diretamente. Primeiro, é necessário obter o construtor da definição de tipo genérico `List<T>` e, em seguida, chamar um método que o converte para o construtor correspondente do `List<TFirst>`.  
  
     O construtor usado para este exemplo de código utiliza um `IEnumerable<T>`. No entanto, observe que essa não é a definição de tipo genérico da interface genérica <xref:System.Collections.Generic.IEnumerable%601>, em vez disso, o parâmetro de tipo `T` de `List<T>` deve ser substituído pelo parâmetro de tipo `T` de `IEnumerable<T>`. (Isso parece confuso apenas porque ambos os tipos têm parâmetros de tipo chamados `T`. É por isso que este exemplo de código usa os nomes `TFirst` e `TSecond` .) Para obter o tipo do argumento do Construtor, comece com a definição de tipo genérico `IEnumerable<T>` e chame <xref:System.Type.MakeGenericType%2A> com o primeiro parâmetro de tipo genérico de `List<T>` . A lista de argumentos do construtor deve ser passada como uma matriz, com apenas um argumento nesse caso.  
  
    > [!NOTE]
    > A definição de tipo genérico é expressa como `IEnumerable<>` quando você usa o operador `typeof` em C# ou `IEnumerable(Of )` quando usa o operador `GetType` no Visual Basic.  
  
     Agora é possível obter o construtor de `List<T>` chamando <xref:System.Type.GetConstructor%2A> na definição de tipo genérico. Para converter esse construtor para o construtor correspondente do `List<TFirst>`, passe `List<TFirst>` e o construtor de `List<T>` para o método <xref:System.Reflection.Emit.TypeBuilder.GetConstructor%28System.Type%2CSystem.Reflection.ConstructorInfo%29?displayProperty=nameWithType> estático.  
  
     [!code-cpp[EmitGenericType#23](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#23)]
     [!code-csharp[EmitGenericType#23](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#23)]
     [!code-vb[EmitGenericType#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#23)]  
  
10. Crie o tipo e salve o arquivo.  
  
     [!code-cpp[EmitGenericType#8](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#8)]
     [!code-csharp[EmitGenericType#8](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#8)]
     [!code-vb[EmitGenericType#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#8)]  
  
11. Invoque o método. `ExampleMethod` não é genérico, mas o tipo ao qual ele pertence é genérico, portanto, para obter um <xref:System.Reflection.MethodInfo> que pode ser invocado é necessário criar um tipo construído da definição de tipo para `Sample`. O tipo construído usa a classe `Example`, que atende às restrições em `TFirst` porque é um tipo de referência e tem um construtor sem parâmetros padrão e a classe `ExampleDerived` que atende às restrições em `TSecond`. (O código para `ExampleDerived` pode ser encontrado na seção de código de exemplo.) Esses dois tipos são passados para <xref:System.Type.MakeGenericType%2A> para criar o tipo construído. O <xref:System.Reflection.MethodInfo> é então obtido usando o método <xref:System.Type.GetMethod%2A>.  
  
     [!code-cpp[EmitGenericType#9](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#9)]
     [!code-csharp[EmitGenericType#9](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#9)]
     [!code-vb[EmitGenericType#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#9)]  
  
12. O código a seguir cria uma matriz de objetos `Example`, coloca essa matriz em uma matriz do tipo <xref:System.Object> que representa os argumentos do método a ser invocado e os passa para o método <xref:System.Reflection.MethodBase.Invoke%28System.Object%2CSystem.Object%5B%5D%29>. O primeiro argumento do método <xref:System.Reflection.MethodBase.Invoke%2A> é uma referência nula porque o método é `static`.  
  
     [!code-cpp[EmitGenericType#10](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#10)]
     [!code-csharp[EmitGenericType#10](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#10)]
     [!code-vb[EmitGenericType#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#10)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir define uma classe chamada `Sample`, junto com uma classe base e duas interfaces. O programa define dois parâmetros de tipo genérico para `Sample`, transformando-o em um tipo genérico. Os parâmetros de tipo são a única coisa que torna um tipo genérico. O programa mostra isso exibindo uma mensagem de teste antes e após a definição dos parâmetros de tipo.  
  
 O parâmetro de tipo `TSecond` é usado para demonstrar as restrições de interface e de classe, usando a classe base e interfaces e o parâmetro de tipo `TFirst` é usado para demonstrar restrições especiais.  
  
 O exemplo de código define um campo e um método usando os parâmetros de tipo da classe para o tipo de campo e para o parâmetro e tipo de retorno do método.  
  
 Após a classe `Sample` ter sido criada, o método é invocado.  
  
 O programa inclui um método que lista informações sobre um tipo genérico e um método que lista as restrições especiais em um parâmetro de tipo. Esses métodos são usados para exibir informações sobre a classe `Sample` finalizada.  
  
 O programa salva o módulo finalizado no disco como `GenericEmitExample1.dll`, portanto, você pode abri-lo com o [Ildasm.exe (IL Disassembler)](../tools/ildasm-exe-il-disassembler.md) e examinar o MSIL para a classe `Sample`.  
  
 [!code-cpp[EmitGenericType#1](../../../samples/snippets/cpp/VS_Snippets_CLR/EmitGenericType/CPP/source.cpp#1)]
 [!code-csharp[EmitGenericType#1](../../../samples/snippets/csharp/VS_Snippets_CLR/EmitGenericType/CS/source.cs#1)]
 [!code-vb[EmitGenericType#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/EmitGenericType/VB/source.vb#1)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Reflection.Emit.GenericTypeParameterBuilder>
- [Usando a emissão de reflexão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/3y322t50(v=vs.100))
- [Cenários de assemblies dinâmicos para a emissão de reflexão](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/tt9483fk(v=vs.100))
