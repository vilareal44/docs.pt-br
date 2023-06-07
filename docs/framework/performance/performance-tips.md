---
title: Dicas de desempenho do .NET
description: Explore as dicas de desempenho para melhorar a velocidade de execução de um programa no .NET. Confira dicas para conversão boxing e unboxing, cadeias de caracteres e destruidores.
ms.date: 03/30/2017
helpviewer_keywords:
- C# language, performance
- performance [C#]
- Visual Basic, performance
- performance [Visual Basic]
ms.assetid: ae275793-857d-4102-9095-b4c2a02d57f4
author: BillWagner
ms.author: wiwagn
ms.openlocfilehash: 2b3be8b42b5046e52074236de01ca312a0a9a361
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86474261"
---
# <a name="net-performance-tips"></a>Dicas de desempenho do .NET
O termo *desempenho* geralmente se refere à velocidade de execução de um programa. Às vezes, você pode aumentar a velocidade de execução seguindo algumas regras básicas em seu código-fonte. Em alguns programas, é importante examinar atentamente o código e usar criadores de perfil para verificar se eles estão executando o mais rápido possível. Em outros programas, você não precisa executar essa otimização porque o código é executado em velocidade aceitável conforme ele é gravado. Este artigo lista algumas áreas comuns em que o desempenho pode ser prejudicado e dicas para melhorá-lo, bem como links para tópicos adicionais sobre desempenho. Para obter mais informações sobre como planejar e medir o desempenho, consulte [Desempenho](index.md)  
  
## <a name="boxing-and-unboxing"></a>Conversão boxing e unboxing  
 É melhor evitar o uso de tipos de valor em situações em que eles devem sofrer conversão boxing um grande número de vezes, por exemplo, em classes de coleções não genéricas como <xref:System.Collections.ArrayList?displayProperty=nameWithType>. Você pode evitar a conversão boxing de tipos de valor por meio de coleções genéricas como <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>. As conversões boxing e unboxing são processos computacionalmente dispendiosos. Quando um tipo de valor é convertido, um objeto totalmente novo deve ser criado. Isso pode levar até 20 vezes mais tempo que a atribuição de uma referência simples. Ao fazer unboxing, o processo de conversão pode demorar quatro vezes mais que uma atribuição. Para obter mais informações, consulte [Conversões boxing e unboxing](../../csharp/programming-guide/types/boxing-and-unboxing.md).  
  
## <a name="strings"></a>Cadeias de caracteres  
 Ao concatenar um grande número de variáveis de cadeia de caracteres, por exemplo em um loop estreito, use <xref:System.Text.StringBuilder?displayProperty=nameWithType> em vez do [operador +](../../csharp/language-reference/operators/addition-operator.md) de C# ou dos [operadores de concatenação](../../visual-basic/language-reference/operators/concatenation-operators.md) do Visual Basic. Para obter mais informações, consulte [como concatenar várias cadeias de caracteres](../../csharp/how-to/concatenate-multiple-strings.md) e [operadores de concatenação no Visual Basic](../../visual-basic/programming-guide/language-features/operators-and-expressions/concatenation-operators.md).  
  
## <a name="destructors"></a>Destruidores  
 Destruidores vazios não devem ser usados. Quando uma classe contém um destruidor, uma entrada é criada na fila Finalizar. Quando o destruidor é chamado, o coletor de lixo é invocado para processar a fila. Se o destruidor estiver vazio, isso apenas resultará em uma perda de desempenho. Para obter mais informações, consulte [Destruidores](../../csharp/programming-guide/classes-and-structs/destructors.md) e [Tempo de vida do objeto: como os objetos são criados e destruídos](../../visual-basic/programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md).  
  
## <a name="other-resources"></a>Outros recursos  
  
- [Gravar código gerenciado mais rápido: conheça o custo das coisas](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973852(v=msdn.10))  
  
- [Gravação de aplicativos de alto desempenho gerenciados: informações elementares](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973858(v=msdn.10))  
  
- [Noções básicas do coletor de lixo e dicas de desempenho](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973837(v=msdn.10))  
  
- [Dicas e truques sobre desempenho em aplicativos .NET](https://docs.microsoft.com/previous-versions/dotnet/articles/ms973839(v=msdn.10))  

- [Informações úteis sobre desempenho, por Rico Mariani](https://docs.microsoft.com/archive/blogs/ricom/)  

- [Blog de Vance Morrison](https://docs.microsoft.com/archive/blogs/vancem/)
  
## <a name="see-also"></a>Consulte também

- [Desempenho](index.md)
- [Guia de programação do Visual Basic](../../visual-basic/programming-guide/index.md)
- [Guia de programação C#](../../csharp/programming-guide/index.md)
