---
title: Conversões boxing e unboxing – Guia de Programação em C#
description: Saiba mais sobre boxing e unboxing na programação em C#. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
f1_keywords:
- cs.boxing
helpviewer_keywords:
- C# language, boxing
- C# language, unboxing
- unboxing [C#]
- boxing [C#]
ms.assetid: 8da9bbf4-bce9-4b08-b2e5-f64c11c56514
ms.openlocfilehash: 5a5bfcc79de8ba3ff66ca8aab9d86d69d89f9221
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87380690"
---
# <a name="boxing-and-unboxing-c-programming-guide"></a>Conversões boxing e unboxing (Guia de Programação em C#)

Conversão boxing é o processo de conversão de um [tipo de valor](../../language-reference/builtin-types/value-types.md) para o tipo `object` ou para qualquer tipo de interface implementada por esse tipo de valor. Quando as caixas de Common Language Runtime (CLR) um tipo de valor, ele encapsula o valor dentro de uma <xref:System.Object?displayProperty=nameWithType> instância e a armazena no heap gerenciado. A conversão unboxing extrai o tipo de valor do objeto. A conversão boxing é implícita, a conversão unboxing é explícita. O conceito de conversões boxing e unboxing serve como base para a exibição unificada de C# do sistema de tipos em que um valor de qualquer tipo pode ser tratado como um objeto.

No exemplo a seguir, a variável de inteiro `i` é submetida à *conversão boxing* e atribuída ao objeto `o`.

[!code-csharp[csProgGuideTypes#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#14)]

O objeto `o` pode ser submetido à conversão unboxing e atribuído à variável de inteiro `i`:

[!code-csharp[csProgGuideTypes#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#15)]

Os exemplos a seguir ilustram como a conversão boxing é usada em C#.

[!code-csharp[csProgGuideTypes#47](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#47)]

## <a name="performance"></a>Desempenho

Em relação às atribuições simples, as conversões boxing e unboxing são processos computacionalmente dispendiosos. Quando um tipo de valor é submetido à conversão boxing, um novo objeto deve ser alocado e construído. A um grau menor, a conversão necessária para a conversão unboxing também é computacionalmente dispendiosa. Para obter mais informações, consulte [Desempenho](../../../framework/performance/performance-tips.md).

## <a name="boxing"></a>Conversão boxing

A conversão boxing é usada para armazenar tipos de valor no heap coletado como lixo. A conversão boxing é uma conversão implícita de um [tipo de valor](../../language-reference/builtin-types/value-types.md) para o tipo `object` ou para qualquer tipo de interface implementada por esse tipo de valor. A conversão boxing de um tipo de valor aloca uma instância de objeto no heap e copia o valor no novo objeto.

Considere a seguinte declaração de uma variável de tipo de valor:

[!code-csharp[csProgGuideTypes#17](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#17)]

A instrução a seguir aplica implicitamente a operação de conversão boxing na variável `i`:

[!code-csharp[csProgGuideTypes#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#18)]

O resultado dessa instrução é a criação de uma referência de objeto `o`, na pilha, que faz referência a um valor do tipo `int`, no heap. Esse valor é uma cópia do valor do tipo de valor atribuído à variável `i`. A diferença entre as duas variáveis, `i` e `o`, é ilustrada na figura de conversão boxing a seguir:

![Gráfico mostrando a diferença entre variáveis i e o.](./media/boxing-and-unboxing/boxing-operation-i-o-variables.gif)

Também é possível executar a conversão boxing explicitamente como no exemplo a seguir, mas a conversão boxing explícita nunca é necessária:

[!code-csharp[csProgGuideTypes#19](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#19)]

## <a name="description"></a>Descrição

Este exemplo converte uma variável de inteiro `i` em um objeto `o` usando a conversão boxing. Em seguida, o valor armazenado na variável `i` é alterado de `123` para `456`. O exemplo mostra que o tipo do valor original e o objeto submetido à conversão boxing usa locais de memória separados e, portanto, pode armazenar valores diferentes.

## <a name="example"></a>Exemplo

[!code-csharp[csProgGuideTypes#16](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#16)]

## <a name="unboxing"></a>Conversão unboxing

A conversão unboxing é uma conversão explícita do tipo `object` para um [tipo de valor](../../language-reference/builtin-types/value-types.md) ou de um tipo de interface para um tipo de valor que implementa a interface. Uma operação de conversão unboxing consiste em:

- Verificar a instância do objeto para garantir que ele é um valor da conversão boxing de um determinado tipo de valor.

- Copiar o valor da instância para a variável de tipo de valor.

As instruções a seguir demonstram operações conversão boxing e unboxing:

[!code-csharp[csProgGuideTypes#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#21)]

A figura a seguir demonstra o resultado das instruções anteriores:

![Gráfico mostrando uma conversão unboxing.](./media/boxing-and-unboxing/unboxing-conversion-operation.gif)

Para a conversão unboxing de tipos de valor ter êxito em tempo de execução, o item sendo submetido à conversão unboxing deve ser uma referência para um objeto que foi criado anteriormente ao realizar a conversão boxing de uma instância desse tipo de valor. Tentar realizar a conversão unboxing de `null` causa uma <xref:System.NullReferenceException>. Tentar realizar a conversão unboxing de uma referência para um tipo de valor incompatível causa uma <xref:System.InvalidCastException>.

## <a name="example"></a>Exemplo

O exemplo a seguir demonstra um caso de conversão unboxing inválida e o `InvalidCastException` resultante. Usando `try` e `catch`, uma mensagem de erro é exibida quando o erro ocorre.

[!code-csharp[csProgGuideTypes#20](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#20)]

Este programa produz:

`Specified cast is not valid. Error: Incorrect unboxing.`

Se você alterar a instrução:

```csharp
int j = (short) o;
```

para:

```csharp
int j = (int) o;
```

a conversão será executada e você receberá a saída:

`Unboxing OK.`

## <a name="c-language-specification"></a>especificação da linguagem C#

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Veja também

- [Guia de programação em C#](../index.md)
- [Tipos de referência](../../language-reference/keywords/reference-types.md)
- [Tipos de valor](../../language-reference/builtin-types/value-types.md)
