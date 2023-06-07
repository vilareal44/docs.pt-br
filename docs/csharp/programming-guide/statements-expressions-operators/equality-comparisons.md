---
title: Comparações de igualdade – Guia de Programação em C#
description: Saiba mais sobre comparações de igualdade. Consulte descrições de ' igualdade de valor ' e ' igualdade de referência ' e exibir recursos adicionais.
ms.date: 07/20/2015
helpviewer_keywords:
- object equality [C#]
ms.assetid: 10b865ea-4e7b-4127-9242-c9b8f57d9f04
ms.openlocfilehash: d10d1851978ef25b7b02503f196cd2a436aab608
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381678"
---
# <a name="equality-comparisons-c-programming-guide"></a>Comparações de igualdade (Guia de Programação em C#)

Às vezes, é necessário comparar dois valores em relação à igualdade. Em alguns casos, testa-se a *igualdade de valor*, também conhecida como *equivalência*, o que significa que os valores contidos pelas duas variáveis são iguais. Em outros casos, é necessário determinar se duas variáveis se referem ao mesmo objeto subjacente na memória. Esse tipo de igualdade é chamado *igualdade de referência* ou *identidade*. Este tópico descreve esses dois tipos de igualdade e fornece links para outros tópicos que fornecem mais informações.  
  
## <a name="reference-equality"></a>Igualdade de referência

 Igualdade de referência significa que as duas referências de objeto se referem ao mesmo objeto subjacente. Isso pode ocorrer por meio de uma atribuição simples, conforme mostrado no exemplo a seguir.  
  
 [!code-csharp[csProgGuideStatements#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#18)]  
  
 Nesse código, dois objetos são criados, mas após a instrução de atribuição, ambas as referências se referem ao mesmo objeto. Portanto, eles têm igualdade de referência. Use o método <xref:System.Object.ReferenceEquals%2A> para determinar se duas referências referenciam o mesmo objeto.  
  
O conceito de igualdade de referência se aplica apenas a tipos de referência. Objetos de tipo de valor não podem ter igualdade de referência, pois quando uma instância de um tipo de valor é atribuída a uma variável, uma cópia do valor é gerada. Portanto, não é possível ter dois structs desconvertidos que referenciam o mesmo local na memória. Além disso, se <xref:System.Object.ReferenceEquals%2A> for usado para comparar dois tipos de valor, o resultado sempre será `false`, mesmo se os valores contidos nos objetos forem idênticos. Isso ocorre porque cada variável é convertido em uma instância de objeto separada. Para obter mais informações, consulte [como testar a igualdade de referência (identidade)](./how-to-test-for-reference-equality-identity.md).

## <a name="value-equality"></a>Igualdade de valor

 Igualdade de valor significa que dois objetos contêm o mesmo valor ou valores. Para tipos de valor primitivos, como [int](../../language-reference/builtin-types/integral-numeric-types.md) ou [bool](../../language-reference/builtin-types/bool.md), os testes de igualdade de valor são simples. Você pode usar o [==](../../language-reference/operators/equality-operators.md#equality-operator-) operador, conforme mostrado no exemplo a seguir.  
  
```csharp  
int a = GetOriginalValue();  
int b = GetCurrentValue();  
  
// Test for value equality.
if (b == a)
{  
    // The two integers are equal.  
}  
```  
  
 Para a maioria dos outros tipos, o teste de igualdade de valor é mais complexo, pois é necessário entender como o tipo o define. Para classes e structs que têm vários campos ou propriedades, a igualdade de valor geralmente é definida para determinar que todos os campos ou propriedades tenham o mesmo valor. Por exemplo, dois objetos `Point` podem ser definidos para serem equivalentes se pointA.X for igual a pointB.X e pointA.Y for igual a pointB.Y.  
  
No entanto, não há nenhuma exigência de que a equivalência seja baseada em todos os campos em um tipo. Ela pode ser baseada em um subconjunto. Ao comparar tipos que não são de sua propriedade, certifique-se de que a forma como a equivalência é definida especificamente para esse tipo foi entendida. Para obter mais informações sobre como definir a igualdade de valor em suas próprias classes e estruturas, consulte [como definir a igualdade de valor para um tipo](./how-to-define-value-equality-for-a-type.md).
  
### <a name="value-equality-for-floating-point-values"></a>Igualdade de valor para valores de ponto flutuante

 As comparações de igualdade de valores de ponto flutuante ([double](../../language-reference/builtin-types/floating-point-numeric-types.md) e [float](../../language-reference/builtin-types/floating-point-numeric-types.md)) são problemáticas devido à imprecisão da aritmética de ponto flutuante em computadores binários. Para obter mais informações, consulte os comentários no tópico <xref:System.Double?displayProperty=nameWithType>.  
  
## <a name="related-topics"></a>Tópicos relacionados  
  
|Título|Descrição|  
|-----------|-----------------|  
|[Como testar a igualdade de referência (identidade)](./how-to-test-for-reference-equality-identity.md)|Descreve como determinar se duas variáveis têm igualdade de referência.|  
|[Como definir a igualdade de valor para um tipo](./how-to-define-value-equality-for-a-type.md)|Descreve como fornecer uma definição personalizada de igualdade de valor a um tipo.|  
|[Guia de programação C#](../index.md)|Fornece links para informações detalhadas sobre recursos importantes da linguagem C# e recursos que estão disponíveis para C# por meio do .NET.|  
|[Types](../types/index.md)|Fornece informações sobre o sistema de tipos do C# e links para mais informações.|  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
