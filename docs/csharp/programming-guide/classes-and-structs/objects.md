---
title: Objetos – Guia de Programação em C#
description: O C# usa uma definição de classe ou estrutura para definir tipos de objetos. Em uma linguagem orientada a objeto, como C#, um programa consiste em objetos que interagem dinamicamente.
ms.date: 07/20/2015
helpviewer_keywords:
- objects [C#], about objects
- variables [C#]
ms.assetid: af4a5230-fbf3-4eea-95e1-8b883c2f845c
ms.openlocfilehash: b54db325c568dec702e4e50c3c265286662092fe
ms.sourcegitcommit: 3d84eac0818099c9949035feb96bbe0346358504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/21/2020
ms.locfileid: "86864755"
---
# <a name="objects-c-programming-guide"></a>Objetos (Guia de Programação em C#)
Uma definição de classe ou struct é como um esquema que especifica o que o tipo pode fazer. Um objeto é basicamente um bloco de memória que foi alocado e configurado de acordo com o esquema. Um programa pode criar vários objetos da mesma classe. Objetos também são chamados de instâncias e podem ser armazenados em uma variável nomeada ou em uma matriz ou coleção. O código de cliente é o código que usa essas variáveis para chamar os métodos e acessar as propriedades públicas do objeto. Em uma linguagem orientada a objetos, como o C#, um programa típico consiste em vários objetos que interagem dinamicamente.  
  
> [!NOTE]
> Tipos estáticos se comportam de modo diferente do que está descrito aqui. Para obter mais informações, consulte [classes estáticas e membros de classe estática](./static-classes-and-static-class-members.md).
  
## <a name="struct-instances-vs-class-instances"></a>Instâncias de struct versus instâncias de classe  
 Como as classes são tipos de referência, uma variável de um objeto de classe contém uma referência ao endereço do objeto no heap gerenciado. Se um segundo objeto do mesmo tipo for atribuído ao primeiro objeto, as duas variáveis farão referência ao objeto nesse endereço. Esse ponto é abordado com mais detalhes posteriormente neste tópico.  
  
 Instâncias de classes são criadas usando o [operador new](../../language-reference/operators/new-operator.md). No exemplo a seguir, `Person` é o tipo e `person1` e `person 2` são instâncias ou objetos desse tipo.  
  
 [!code-csharp[csProgGuideStatements#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#30)]  
  
 Como structs são tipos de valor, uma variável de um objeto de struct mantém uma cópia do objeto inteiro. Instâncias de structs também podem ser criadas usando o operador `new`, mas isso não é obrigatório, conforme mostrado no exemplo a seguir:  
  
 [!code-csharp[csProgGuideStatements#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#31)]  
  
 A memória de `p1` e `p2` é alocada na pilha de thread. Essa memória é recuperada em conjunto com o tipo ou método em que ela é declarada. Esse é um dos motivos pelos quais os structs são copiados na atribuição. Por outro lado, a memória alocada a uma instância de classe é recuperada automaticamente (o lixo é coletado) pelo Common Language Runtime quando todas as referências ao objeto tiveram saído do escopo. Não é possível destruir de forma determinista um objeto de classe, como é possível no C++. Para obter mais informações sobre a coleta de lixo no .NET, consulte [coleta de lixo](../../../standard/garbage-collection/index.md).  
  
> [!NOTE]
> A alocação e a desalocação de memória no heap gerenciado é altamente otimizada no Common Language Runtime. Na maioria dos casos, não há uma diferença significativa quanto ao custo do desempenho de alocar uma instância da classe no heap em vez de alocar uma instância de struct na pilha.
  
## <a name="object-identity-vs-value-equality"></a>Identidade do objeto versus igualdade de valor  
 Quando compara dois objetos quanto à igualdade, primeiro você precisa distinguir se quer saber se as duas variáveis representam o mesmo objeto na memória ou se os valores de um ou mais de seus campos são equivalentes. Se estiver pretendendo comparar valores, você precisa considerar se os objetos são instâncias de tipos de valor (structs) ou tipos de referência (classes, delegados, matrizes).  
  
- Para determinar se duas instâncias de classe se referem ao mesmo local na memória (o que significa que elas têm a mesma *identidade*), use o método <xref:System.Object.Equals%2A> estático. (<xref:System.Object?displayProperty=nameWithType> é a classe base implícita para todos os tipos de valor e tipos de referência, incluindo classes e structs definidos pelo usuário.)  
  
- Para determinar se os campos de instância em duas instâncias de struct têm os mesmos valores, use o método <xref:System.ValueType.Equals%2A?displayProperty=nameWithType>. Como todos os structs herdam implicitamente de <xref:System.ValueType?displayProperty=nameWithType>, você chama o método diretamente em seu objeto, conforme mostrado no exemplo a seguir:  
  
 [!code-csharp[csProgGuideStatements#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#32)]  
  
 A implementação de <xref:System.ValueType?displayProperty=nameWithType> de `Equals` usa reflexão porque ela precisa ser capaz de determinar quais são os campos em qualquer struct. Ao criar seus próprios structs, substitua o método `Equals` para fornecer um algoritmo de igualdade eficiente que é específico ao seu tipo.  
  
- Para determinar se os valores dos campos em duas instâncias de classe são iguais, você pode usar o método <xref:System.Object.Equals%2A> ou o [Operador ==](../../language-reference/operators/equality-operators.md#equality-operator-). No entanto, use-os apenas se a classe os tiver substituído ou sobrecarregado para fornecer uma definição personalizada do que "igualdade" significa para objetos desse tipo. A classe também pode implementar a interface <xref:System.IEquatable%601> ou a interface <xref:System.Collections.Generic.IEqualityComparer%601>. As duas interfaces fornecem métodos que podem ser usados para testar a igualdade de valores. Ao criar suas próprias classes que substituem `Equals` , certifique-se de seguir as diretrizes declaradas em [como definir a igualdade de valor para um tipo](../statements-expressions-operators/how-to-define-value-equality-for-a-type.md) e <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> .
  
## <a name="related-sections"></a>Seções relacionadas  
 Para mais informações:  
  
- [Classes](./classes.md)  
  
- [Construtores](./constructors.md)  
  
- [Finalizadores](./destructors.md)  
  
- [Eventos](../events/index.md)  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [object](../../language-reference/builtin-types/reference-types.md)
- [Herança](./inheritance.md)
- [class](../../language-reference/keywords/class.md)
- [Tipos de estrutura](../../language-reference/builtin-types/struct.md)
- [novo operador](../../language-reference/operators/new-operator.md)
- [Sistema de tipos comum](../../../standard/base-types/common-type-system.md)
