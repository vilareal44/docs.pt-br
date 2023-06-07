---
title: Tipos de erro
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions, types
- errors [Visual Basic], types
- errors [Visual Basic], logic
- errors [Visual Basic], syntax
- logic errors [Visual Basic], Visual Basic
- run-time errors [Visual Basic], types of errors
- syntax errors [Visual Basic], Visual Basic
ms.assetid: 3048aabf-8c97-4e13-9150-853769cb5f6f
ms.openlocfilehash: 107d93429ad0440ed18169bc6b6ca7b2e21cb77a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405138"
---
# <a name="error-types-visual-basic"></a>Tipos de erro (Visual Basic)
No Visual Basic, os erros se enquadram em uma das três categorias: erros de sintaxe, erros de tempo de execução e erros de lógica.

## <a name="syntax-errors"></a>Erros de sintaxe
 Os *erros de sintaxe* são aqueles que aparecem enquanto você escreve o código. Se você estiver usando o Visual Studio, Visual Basic verificará seu código ao digitá-lo na janela **Editor de código** e o alertará se cometer um erro, como grafar uma palavra incorreta ou usando um elemento de linguagem incorretamente. Se você compilar a partir da linha de comando, Visual Basic exibirá um erro do compilador com informações sobre o erro de sintaxe. Os erros de sintaxe são o tipo mais comum de erros. Você pode corrigi-los facilmente no ambiente de codificação assim que eles ocorrerem.

> [!NOTE]
> A `Option Explicit` instrução é um meio de evitar erros de sintaxe. Ele força você a declarar, com antecedência, todas as variáveis a serem usadas no aplicativo. Portanto, quando essas variáveis são usadas no código, quaisquer erros tipográficos são capturados imediatamente e podem ser corrigidos.

## <a name="run-time-errors"></a>Erros de tempo de execução
 Os *erros de tempo de execução* são aqueles que aparecem somente depois que você compila e executa seu código. Eles envolvem código que podem parecer corretos para que não haja erros de sintaxe, mas que não serão executados. Por exemplo, você pode escrever corretamente uma linha de código para abrir um arquivo. Mas se o arquivo não existir, o aplicativo não poderá abrir o arquivo e lançar uma exceção. Você pode corrigir a maioria dos erros de tempo de execução reescrevendo o código com falha ou usando a [manipulação de exceções](../../language-reference/statements/try-catch-finally-statement.md)e, em seguida, recompilando-o e executando-o novamente.
  
## <a name="logic-errors"></a>Erros lógicos
 Os *erros lógicos* são aqueles que aparecem quando o aplicativo está em uso. Eles são geralmente suposições com falhas feitas pelo desenvolvedor ou resultados indesejados ou inesperados em resposta às ações do usuário. Por exemplo, uma chave digitada incorretamente pode fornecer informações incorretas para um método, ou você pode pressupor que um valor válido sempre é fornecido para um método quando esse não for o caso. Embora os erros de lógica possam ser tratados usando a [manipulação de exceção](../../language-reference/statements/try-catch-finally-statement.md) (por exemplo, testando se um argumento é `Nothing` e gerando um <xref:System.ArgumentNullException> ), mais comumente eles devem ser resolvidos corrigindo o erro em lógica e recompilando o aplicativo.

## <a name="see-also"></a>Confira também

- [Instrução Try...Catch...Finally](../../language-reference/statements/try-catch-finally-statement.md)
- [Noções básicas do depurador](/visualstudio/debugger/debugger-feature-tour)
