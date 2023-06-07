---
title: 'Como: Examinar tipos genéricos e criar instâncias deles com a reflexão'
description: Veja como examinar e instanciar tipos genéricos com reflexão. Use as propriedades isgenéricatype, IsGenericParameter e GenericParameterPosition.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reflection, generic types
- generics [.NET Framework], reflection
ms.assetid: f93b03b0-1778-43fc-bc6d-35983d210e74
ms.openlocfilehash: b57a0ed0c809da442dc9fcf202ad364060971f80
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86865093"
---
# <a name="how-to-examine-and-instantiate-generic-types-with-reflection"></a>Como: Examinar tipos genéricos e criar instâncias deles com a reflexão
As informações sobre tipos genéricos são obtidas da mesma forma que as informações sobre os outros tipos: examinando um objeto <xref:System.Type> que representa o tipo genérico. A diferença de princípio é que um tipo genérico tem uma lista de objetos <xref:System.Type> que representam seus parâmetros de tipo genérico. O primeiro procedimento nesta seção examina os tipos genéricos.  
  
 Você pode criar um objeto <xref:System.Type> que representa um tipo construído associando argumentos de tipo aos parâmetros de tipo de uma definição de tipo genérico. O segundo procedimento demonstra isso.  
  
### <a name="to-examine-a-generic-type-and-its-type-parameters"></a>Para examinar um tipo genérico e seus parâmetros de tipo  
  
1. Obtenha uma instância de <xref:System.Type> que representa o tipo genérico. No código a seguir, o tipo é obtido usando o operador `typeof` C# (`GetType` no Visual Basic, `typeid` no Visual C++). Consulte o tópico da classe <xref:System.Type> para encontrar outras maneiras de obter um objeto <xref:System.Type>. Observe que no restante deste procedimento, o tipo está contido em um parâmetro do método chamado `t`.  
  
     [!code-cpp[HowToGeneric#2](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#2)]
     [!code-csharp[HowToGeneric#2](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#2)]
     [!code-vb[HowToGeneric#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#2)]  
  
2. Use a propriedade <xref:System.Type.IsGenericType%2A> para determinar se o tipo é genérico e use a propriedade <xref:System.Type.IsGenericTypeDefinition%2A> para determinar se o tipo é uma definição de tipo genérico.  
  
     [!code-cpp[HowToGeneric#3](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#3)]
     [!code-csharp[HowToGeneric#3](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#3)]
     [!code-vb[HowToGeneric#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#3)]  
  
3. Obtenha uma matriz que contém os argumentos de tipo genérico, usando o método <xref:System.Type.GetGenericArguments%2A>.  
  
     [!code-cpp[HowToGeneric#4](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#4)]
     [!code-csharp[HowToGeneric#4](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#4)]
     [!code-vb[HowToGeneric#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#4)]  
  
4. Para cada argumento de tipo, determine se ele é um parâmetro de tipo (por exemplo, em uma definição de tipo genérico) ou um tipo que foi especificado para um parâmetro de tipo (por exemplo, em um tipo construído), usando a propriedade <xref:System.Type.IsGenericParameter%2A>.  
  
     [!code-cpp[HowToGeneric#5](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#5)]
     [!code-csharp[HowToGeneric#5](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#5)]
     [!code-vb[HowToGeneric#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#5)]  
  
5. No sistema de tipo, um parâmetro de tipo genérico é representado por uma instância de <xref:System.Type>, exatamente como tipos comuns são. O código a seguir exibe a posição do nome e do parâmetro de um objeto <xref:System.Type> que representa um parâmetro de tipo genérico. A posição de parâmetro é uma informação trivial aqui. Ela é mais interessante quando você está examinando um parâmetro de tipo que foi usado como um argumento de tipo de outro tipo genérico.  
  
     [!code-cpp[HowToGeneric#6](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#6)]
     [!code-csharp[HowToGeneric#6](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#6)]
     [!code-vb[HowToGeneric#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#6)]  
  
6. Determine a restrição de tipo base e as restrições de interface de um parâmetro de tipo genérico usando o método <xref:System.Type.GetGenericParameterConstraints%2A> para obter todas as restrições em uma única matriz. Não há garantia de que as restrições estarão em uma ordem específica.  
  
     [!code-cpp[HowToGeneric#7](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#7)]
     [!code-csharp[HowToGeneric#7](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#7)]
     [!code-vb[HowToGeneric#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#7)]  
  
7. Use a propriedade <xref:System.Type.GenericParameterAttributes%2A> para descobrir as restrições especiais em um parâmetro de tipo, como exigir que ele seja um tipo de referência. A propriedade também inclui valores que representam a variação, que você pode mascarar, conforme mostrado no código a seguir.  
  
     [!code-cpp[HowToGeneric#8](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#8)]
     [!code-csharp[HowToGeneric#8](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#8)]
     [!code-vb[HowToGeneric#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#8)]  
  
8. Os atributos de restrição especial são sinalizadores e o mesmo sinalizador (<xref:System.Reflection.GenericParameterAttributes.None?displayProperty=nameWithType>) que não representa restrições especiais também não representa nenhuma covariância ou contravariância. Portanto, para testar se há alguma dessas condições, você deve usar a máscara apropriada. Nesse caso, use <xref:System.Reflection.GenericParameterAttributes.SpecialConstraintMask?displayProperty=nameWithType> para isolar os sinalizadores de restrição especial.  
  
     [!code-cpp[HowToGeneric#9](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#9)]
     [!code-csharp[HowToGeneric#9](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#9)]
     [!code-vb[HowToGeneric#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#9)]  
  
## <a name="constructing-an-instance-of-a-generic-type"></a>Criando uma instância de um tipo genérico  
 Um tipo genérico é como um modelo. Você não pode criar instâncias dele a menos que especifique tipos reais para seus parâmetros de tipo genérico. Fazer isso em tempo de execução, usando reflexão, requer o método <xref:System.Type.MakeGenericType%2A>.  
  
#### <a name="to-construct-an-instance-of-a-generic-type"></a>Para criar uma instância de um tipo genérico  
  
1. Obtenha um objeto <xref:System.Type> que representa o tipo genérico. O código a seguir obtém o tipo genérico <xref:System.Collections.Generic.Dictionary%602> de duas maneiras diferentes: usando as sobrecargas de método <xref:System.Type.GetType%28System.String%29?displayProperty=nameWithType> com uma cadeia de caracteres descrevendo o tipo e chamando o método <xref:System.Type.GetGenericTypeDefinition%2A> no tipo construído `Dictionary\<String, Example>` (`Dictionary(Of String, Example)` no Visual Basic). O método <xref:System.Type.MakeGenericType%2A> requer uma definição de tipo genérico.  
  
     [!code-cpp[HowToGeneric#10](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#10)]
     [!code-csharp[HowToGeneric#10](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#10)]
     [!code-vb[HowToGeneric#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#10)]  
  
2. Crie uma matriz dos argumentos de tipo para substituir pelos parâmetros de tipo. A matriz deve conter o número correto de objetos <xref:System.Type>, na mesma ordem em que aparecem na lista de parâmetros de tipo. Nesse caso, a chave (primeiro parâmetro de tipo) é do tipo <xref:System.String> e os valores no dicionário são instâncias de uma classe chamada `Example`.  
  
     [!code-cpp[HowToGeneric#11](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#11)]
     [!code-csharp[HowToGeneric#11](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#11)]
     [!code-vb[HowToGeneric#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#11)]  
  
3. Chame o método <xref:System.Type.MakeGenericType%2A> para associar os argumentos de tipo para os parâmetros de tipo e criar o tipo.  
  
     [!code-cpp[HowToGeneric#12](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#12)]
     [!code-csharp[HowToGeneric#12](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#12)]
     [!code-vb[HowToGeneric#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#12)]  
  
4. Use a sobrecarga de método <xref:System.Activator.CreateInstance%28System.Type%29> para criar um objeto do tipo construído. O código a seguir armazena duas instâncias da classe `Example` no objeto `Dictionary<String, Example>` resultante.  
  
     [!code-cpp[HowToGeneric#13](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#13)]
     [!code-csharp[HowToGeneric#13](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#13)]
     [!code-vb[HowToGeneric#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#13)]  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir define um método `DisplayGenericType` para examinar as definições de tipo genérico e tipos construídos usados no código e exibir suas informações. O método `DisplayGenericType` mostra como usar as propriedades <xref:System.Type.IsGenericType%2A>, <xref:System.Type.IsGenericParameter%2A> e <xref:System.Type.GenericParameterPosition%2A> e o método <xref:System.Type.GetGenericArguments%2A>.  
  
 O exemplo também define um método `DisplayGenericParameter` para examinar um parâmetro de tipo genérico e exibir suas restrições.  
  
 O exemplo de código define um conjunto de tipos de teste, incluindo um tipo genérico que ilustra as restrições de parâmetro de tipo e mostra como exibir informações sobre esses tipos.  
  
 O exemplo cria um tipo na classe <xref:System.Collections.Generic.Dictionary%602> criando uma matriz de argumentos de tipo e chamando o método <xref:System.Type.MakeGenericType%2A>. O programa compara o objeto <xref:System.Type> criado usando <xref:System.Type.MakeGenericType%2A> com um objeto <xref:System.Type> obtido usando `typeof` (`GetType` no Visual Basic), demonstrando que são iguais. Da mesma forma, o programa usa o método <xref:System.Type.GetGenericTypeDefinition%2A> para obter a definição de tipo genérico do tipo construído e o compara com o objeto <xref:System.Type> que representa a classe <xref:System.Collections.Generic.Dictionary%602>.  
  
 [!code-cpp[HowToGeneric#1](../../../samples/snippets/cpp/VS_Snippets_CLR/HowToGeneric/cpp/ur.cpp#1)]
 [!code-csharp[HowToGeneric#1](../../../samples/snippets/csharp/VS_Snippets_CLR/HowToGeneric/CS/ur.cs#1)]
 [!code-vb[HowToGeneric#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/HowToGeneric/VB/ur.vb#1)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Type>
- <xref:System.Reflection.MethodInfo>
- [Reflexão e tipos genéricos](reflection-and-generic-types.md)
- [Exibindo informações de tipo](viewing-type-information.md)
- [Genéricos](../../standard/generics/index.md)
