---
title: Usando exceções – Guia de Programação em C#
description: Saiba como usar exceções. As exceções são geradas pelo código que encontra um erro e detectadas pelo código que corrige o erro.
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#], about exception handling
- exceptions [C#], about exceptions
ms.assetid: 71472c62-320a-470a-97d2-67995180389d
ms.openlocfilehash: fb45381f1c6cfa2f27d036ead8e662b7a0d8d924
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303368"
---
# <a name="use-exceptions-c-programming-guide"></a>Usar exceções (guia de programação C#)

No C#, os erros no programa em tempo de execução são propagados pelo programa usando um mecanismo chamado exceções. As exceções são geradas pelo código que encontra um erro e capturadas pelo código que pode corrigir o erro. As exceções podem ser geradas pelo tempo de execução do .NET ou pelo código em um programa. Uma vez que uma exceção é gerada, ela é propagada acima na pilha de chamadas até uma instrução `catch` para a exceção ser encontrada. As exceções não capturadas são tratadas por um manipulador de exceção genérico fornecido pelo sistema que exibe uma caixa de diálogo.  
  
 As exceções são representadas por classes derivadas de <xref:System.Exception>. Essa classe identifica o tipo de exceção e contém propriedades que têm detalhes sobre a exceção. Gerar uma exceção envolve criar uma instância de uma classe derivada de exceção, opcionalmente configurar propriedades da exceção e, em seguida, gerar o objeto usando a palavra-chave `throw`. Por exemplo:  
  
 [!code-csharp[csProgGuideExceptions#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#1)]  
  
 Depois que uma exceção é gerada, o runtime verifica a instrução atual para ver se ela está dentro de um bloco `try`. Se estiver, todos os blocos `catch` associados ao bloco `try` serão verificados para ver se eles podem capturar a exceção. Os blocos `Catch` normalmente especificam os tipos de exceção. Se o tipo do bloco `catch` for do mesmo tipo que a exceção ou uma classe base da exceção, o bloco `catch` poderá manipular o método. Por exemplo:  
  
 [!code-csharp[csProgGuideExceptions#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#2)]  
  
 Se a instrução que gera uma exceção não estiver dentro de um bloco `try` ou se o bloco `try` que o contém não tiver um bloco `catch` correspondente, o runtime verificará o método de chamada quanto a uma instrução `try` e blocos `catch`. O runtime continuará acima na pilha de chamada, pesquisando um bloco `catch` compatível. Depois que o bloco `catch` for localizado e executado, o controle será passado para a próxima instrução após aquele bloco `catch`.  
  
 Uma instrução `try` pode conter mais de um bloco `catch`. A primeira instrução `catch` que pode manipular a exceção é executado, todas as instruções `catch` posteriores, mesmo se forem compatíveis, são ignoradas. Portanto, os blocos de captura devem sempre ser ordenados do mais específico (ou mais derivado) para o menos específico. Por exemplo:  
  
 [!code-csharp[csProgGuideExceptions#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#3)]  
  
 Antes de o bloco `catch` ser executado, o runtime verifica se há blocos `finally`. Os blocos `Finally` permitem que o programador limpe qualquer estado ambíguo que pode ser deixado de um bloco `try` cancelado ou libere quaisquer recursos externos (como identificadores de gráfico, conexões de banco de dados ou fluxos de arquivo) sem esperar o coletor de lixo no runtime finalizar os objetos. Por exemplo:  
  
 [!code-csharp[csProgGuideExceptions#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#4)]  
  
 Se `WriteByte()` gerou uma exceção, o código no segundo bloco `try` que tentar reabrir o arquivo falhará se `file.Close()` não for chamado e o arquivo permanecerá bloqueado. Como os blocos `finally` são executados mesmo se uma exceção for gerada, o bloco `finally` no exemplo anterior permite que o arquivo seja fechado corretamente e ajuda a evitar um erro.  
  
 Se não for encontrado nenhum bloco `catch` compatível na pilha de chamadas após uma exceção ser gerada, ocorrerá uma das três coisas:  
  
- Se a exceção estiver em um finalizador, o finalizador será anulado e o finalizador base, se houver, será chamado.  
  
- Se a pilha de chamadas contiver um construtor estático ou um inicializador de campo estático, uma <xref:System.TypeInitializationException> será gerada, com a exceção original atribuída à propriedade <xref:System.Exception.InnerException%2A> da nova exceção.  
  
- Se o início do thread for atingido, o thread será encerrado.  
  
## <a name="see-also"></a>Confira também

- [Guia de programação C#](../index.md)
- [Exceções e tratamento de exceções](./index.md)
