---
title: Como usar o bloco try-catch para capturar exceções
description: Use o bloco try para conter instruções que podem gerar ou gerar uma exceção. Coloque instruções para manipular exceções em um ou mais blocos catch.
ms.date: 02/06/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- exceptions, try/catch blocks
- try blocks
- try/catch blocks
- catch blocks
ms.assetid: a3ce6dfd-1f64-471b-8ad8-8cfaf406275d
ms.openlocfilehash: 60ed213ea777fe35873fd1e67555b7506e3ca587
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84768905"
---
# <a name="how-to-use-the-trycatch-block-to-catch-exceptions"></a>Como usar o bloco try/catch para capturar exceções

Colocar todas as instruções de código que podem elevar ou gerar uma exceção em um bloco `try` e posicionar instruções usadas para tratar a exceção ou exceções em um ou mais blocos `catch` abaixo do bloco `try`. Cada bloco `catch` inclui o tipo de exceção e pode conter instruções adicionais necessárias para lidar com esse tipo de exceção.

No exemplo a seguir, um <xref:System.IO.StreamReader> abre um arquivo chamado *data.txt* e recupera uma linha desse arquivo. Uma vez que o código pode gerar qualquer uma de três exceções, ele é colocado em um bloco `try`. Três blocos `catch` capturam as exceções e lidam com elas, exibindo os resultados no console.

[!code-csharp[CatchException#3](~/samples/snippets/csharp/VS_Snippets_CLR/CatchException/CS/catchexception2.cs#3)]
[!code-vb[CatchException#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/CatchException/VB/catchexception2.vb#3)]

O CLR (Common Language Runtime) captura exceções não manipuladas pelos blocos `catch`. Se uma exceção é capturada pelo CLR, um dos seguintes resultados pode ocorrer dependendo da configuração do CLR:

- Uma caixa de diálogo **Depurar** é exibida.
- O programa interromperá a execução e uma caixa de diálogo será exibida com informações de exceção.
- Um erro é impresso no [fluxo de saída de erro padrão](xref:System.Console.Error).

> [!NOTE]
> A maioria dos códigos pode lançar uma exceção, sendo que algumas exceções, tais como <xref:System.OutOfMemoryException>, podem ser geradas pelo próprio CLR, a qualquer momento. Embora os aplicativos não precisem lidar com essas exceções, esteja ciente dessa possibilidade ao gravar bibliotecas para serem usadas por outros. Para obter sugestões sobre quando definir código em um bloco `try`, confira [Práticas recomendadas para exceções](best-practices-for-exceptions.md).

## <a name="see-also"></a>Veja também

- [Exceções](index.md)
- [Tratamento de erros de E/S no .NET](../io/handling-io-errors.md)
