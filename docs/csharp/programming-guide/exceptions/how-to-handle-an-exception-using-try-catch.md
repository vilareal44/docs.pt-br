---
title: Como tratar uma exceção usando o guia de programação do try-catch-C#
description: Saiba como tratar uma exceção usando um bloco try-catch. Consulte um exemplo de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#], try/catch blocks
- exceptions [C#], try/catch blocks
- try/catch blocks [C#]
ms.assetid: ca8e3773-980e-4767-8633-7408540e9818
ms.openlocfilehash: 357aebe042bc5b6e761b3c1bad258021441de22c
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301990"
---
# <a name="how-to-handle-an-exception-using-trycatch-c-programming-guide"></a>Como tratar uma exceção usando try/catch (guia de programação C#)
A finalidade de um bloco [try-catch](../../language-reference/keywords/try-catch.md) é capturar e manipular uma exceção gerada pelo código de trabalho. Algumas exceções podem ser manipuladas em um bloco `catch` e o problema pode ser resolvido sem que a exceção seja gerada novamente. No entanto, com mais frequência, a única coisa que você pode fazer é certificar-se de que a exceção apropriada seja gerada.  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, <xref:System.IndexOutOfRangeException> não é a exceção mais apropriada: <xref:System.ArgumentOutOfRangeException> faz mais sentido para o método porque o erro é causado pelo argumento `index` passado pelo chamador.  
  
 [!code-csharp[csProgGuideExceptions#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#5)]  
  
## <a name="comments"></a>Comentários  
 O código que causa uma exceção fica dentro do bloco `try`. A instrução `catch` é adicionada logo após para manipular `IndexOutOfRangeException`, se ocorrer. O bloco `catch` manipula o `IndexOutOfRangeException` e gera a exceção `ArgumentOutOfRangeException`, que é mais adequada. Para fornecer ao chamador tantas informações quanto possível, considere especificar a exceção original como o <xref:System.Exception.InnerException%2A> da nova exceção. Como a <xref:System.Exception.InnerException%2A> propriedade é [somente leitura](../../properties.md#read-only), você deve atribuí-la no construtor da nova exceção.  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Exceções e tratamento de exceções](./index.md)
- [Tratamento de exceção](./exception-handling.md)
