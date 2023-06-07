---
title: Tratando e gerando exceções no .NET
description: Saiba como tratar e lançar exceções no .NET. Exceções são como as operações do .NET indicam falha em aplicativos.
ms.date: 06/19/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- exceptions [.NET], handling
- runtime, exceptions
- filtering exceptions
- errors [.NET], exceptions
- exceptions [.NET], throwing
- exceptions [.NET]
- common language runtime, exceptions
ms.assetid: f99a1d29-a2a8-47af-9707-9909f9010735
ms.openlocfilehash: 89d88e3128917125d1a09466ed4e230604d6978c
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662765"
---
# <a name="handling-and-throwing-exceptions-in-net"></a>Tratando e gerando exceções no .NET

Aplicativos devem ser capazes de tratar de erros que ocorrem durante a execução de uma maneira consistente. O .NET fornece um modelo para notificar aplicativos sobre erros de maneira uniforme: operações do .NET indicam falhas por meio da geração de exceções.

## <a name="exceptions"></a>Exceções

Uma exceção é qualquer condição de erro ou comportamento inesperado encontrado por um programa em execução. Exceções podem ser geradas devido a uma falha em seu código ou no código que você chama (como uma biblioteca compartilhada), recursos do sistema operacional não disponíveis, condições inesperadas encontradas pelo runtime (como código que não pode ser verificado) e assim por diante. Seu aplicativo pode se recuperar de algumas dessas condições, mas não de outras. Embora você possa se recuperar da maioria das exceções de aplicativo, não é possível recuperar-se da maioria das exceções de runtime.

No .NET, uma exceção é um objeto herdado da classe <xref:System.Exception?displayProperty=nameWithType>. Uma exceção é lançada de uma área do código em que ocorreu um problema. A exceção é passada pilha acima até que o aplicativo trate dela ou o programa seja encerrado.

## <a name="exceptions-vs-traditional-error-handling-methods"></a>Métodos de tratamento de exceção vs. tratamento de erro tradicional

Tradicionalmente, o modelo de tratamento de erro da linguagem confiava na forma exclusiva da linguagem de detectar erros e localizar manipuladores para eles ou no mecanismo de tratamento de erro fornecido pelo sistema operacional. A maneira como o .NET implementa o tratamento de exceção oferece as seguintes vantagens:

- O lançamento e tratamento de exceção funciona da mesma maneira para linguagens de programação .NET.

- Não requer nenhuma sintaxe de linguagem específica para tratamento de exceção, mas permite que cada linguagem defina sua própria sintaxe.

- Exceções podem ser geradas pelos limites de processo e até mesmo de computador.

- O código de tratamento de exceção pode ser adicionado a um aplicativo para aumentar a confiabilidade do programa.

As exceções oferecem vantagens sobre outros métodos de notificação de erro, como códigos de retorno. Falhas não passam despercebidas porque se uma exceção for lançada e você não tratar dela, o runtime encerra o aplicativo. Valores inválidos não continuam a se propagar através do sistema como resultado do código que não consegue verificar se há um código de retorno de falha.

## <a name="common-exceptions"></a>Exceções comuns

A tabela a seguir lista algumas exceções comuns com exemplos do que pode causá-las.

| Tipo de exceção | Descrição | Exemplo |
| -------------- | ----------- | ------- |
| <xref:System.Exception> | A classe base para todas as exceções. | Nenhuma (use uma classe derivada dessa exceção). |
| <xref:System.IndexOutOfRangeException> | Gerada pelo runtime somente quando uma matriz é indexada incorretamente. | Indexar uma matriz fora do intervalo válido: <br /> `arr[arr.Length+1]` |
| <xref:System.NullReferenceException> | Gerada pelo runtime somente quando um objeto nulo é referenciado. | `object o = null;` <br /> `o.ToString();` |
| <xref:System.InvalidOperationException> | Gerada por métodos quando em um estado inválido. | Chamar `Enumerator.MoveNext()` após a remoção de um item da coleção subjacente. |
| <xref:System.ArgumentException> | A classe base para todas as exceções de argumento. | Nenhuma (use uma classe derivada dessa exceção). |
| <xref:System.ArgumentNullException> | Gerada por métodos que não permitem que um argumento seja nulo. | `String s = null;` <br /> `"Calculate".IndexOf(s);`|
| <xref:System.ArgumentOutOfRangeException> | Gerada por métodos que verificam se os argumentos estão em um determinado intervalo. | `String s = "string";` <br /> `s.Substring(s.Length+1);` |

## <a name="see-also"></a>Confira também

- [Classe de exceção e propriedades](exception-class-and-properties.md)
- [Como usar o bloco try-catch para capturar exceções](how-to-use-the-try-catch-block-to-catch-exceptions.md)
- [Como: usar exceções específicas em um bloco catch](how-to-use-specific-exceptions-in-a-catch-block.md)
- [Como gerar exceções explicitamente](how-to-explicitly-throw-exceptions.md)
- [Como: criar exceções definidas pelo usuário](how-to-create-user-defined-exceptions.md)
- [Usando manipuladores de exceção filtrados por usuário](using-user-filtered-exception-handlers.md)
- [Como usar blocos finally](how-to-use-finally-blocks.md)
- [Manipulando exceções de interoperabilidade COM](handling-com-interop-exceptions.md)
- [Práticas recomendadas para exceções](best-practices-for-exceptions.md)
- [O que todo desenvolvedor precisa saber sobre exceções no tempo de execução](https://github.com/dotnet/runtime/blob/master/docs/design/coreclr/botr/exceptions.md)
