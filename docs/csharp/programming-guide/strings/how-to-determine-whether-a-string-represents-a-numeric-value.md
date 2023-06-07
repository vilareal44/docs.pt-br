---
title: Como determinar se uma cadeia de caracteres representa um valor numérico – guia de programação C#
description: Saiba como determinar se uma cadeia de caracteres é uma representação válida de um tipo numérico especificado. Consulte exemplos de código e exiba recursos adicionais.
ms.date: 07/20/2015
helpviewer_keywords:
- numeric strings [C#]
- validating numeric input [C#]
- strings [C#], numeric
ms.assetid: a4e84e10-ea0a-489f-a868-503dded9d85f
ms.openlocfilehash: c248c6c54de493ab06a833fc525252fa812d60da
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381743"
---
# <a name="how-to-determine-whether-a-string-represents-a-numeric-value-c-programming-guide"></a>Como determinar se uma cadeia de caracteres representa um valor numérico (guia de programação C#)
Para determinar se uma cadeia de caracteres é uma representação válida de um tipo numérico especificado, use o método estático `TryParse` implementado por todos os tipos numéricos primitivos e também por tipos como <xref:System.DateTime> e <xref:System.Net.IPAddress>. O exemplo a seguir mostra como determinar se "108" é um [int](../../language-reference/builtin-types/integral-numeric-types.md) válido.  
  
```csharp  
int i = 0;
string s = "108";  
bool result = int.TryParse(s, out i); //i now = 108  
```  
  
 Se a cadeia de caracteres contiver caracteres não numéricos ou o valor numérico for muito grande ou muito pequeno para o tipo especificado, `TryParse` retornará false e definirá o parâmetro de saída como zero. Caso contrário, ele retornará true e definirá o parâmetro de saída como o valor numérico da cadeia de caracteres.  
  
> [!NOTE]
> Uma cadeia de caracteres pode conter apenas caracteres numéricos e ainda não ser válida para o método `TryParse` do tipo usado. Por exemplo, "256" não é um valor válido para `byte`, mas é válido para `int`. “98,6” não é um valor válido para `int`, mas é válido para `decimal`.  
  
## <a name="example"></a>Exemplo  
 Os exemplos a seguir mostram como usar `TryParse` com representações de cadeia de caracteres dos valores `long`, `byte` e `decimal`.  
  
 [!code-csharp[csProgGuideStrings#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStrings/CS/Strings.cs#14)]  
  
## <a name="robust-programming"></a>Programação robusta  
 Os tipos numéricos primitivos também implementam o método estático `Parse`, que lançará uma exceção se a cadeia de caracteres não for um número válido. Geralmente, `TryParse` é mais eficiente, pois retornará false apenas se o número não for válido.  
  
## <a name="net-security"></a>Segurança do .NET  
 Sempre use os métodos `TryParse` ou `Parse` para validar entradas de usuário em controles como caixas de texto e caixas de combinação.  
  
## <a name="see-also"></a>Veja também

- [Como converter uma matriz de bytes em um int](../types/how-to-convert-a-byte-array-to-an-int.md)
- [Como converter uma cadeia de caracteres em um número](../types/how-to-convert-a-string-to-a-number.md)
- [Como converter entre cadeias de caracteres hexadecimais e tipos numéricos](../types/how-to-convert-between-hexadecimal-strings-and-numeric-types.md)
- [Analisar cadeias de caracteres numéricas](../../../standard/base-types/parsing-numeric.md)
- [Formatar tipos](../../../standard/base-types/formatting-types.md)
