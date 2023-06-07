---
title: Mensagens de erro
ms.date: 07/20/2015
helpviewer_keywords:
- errors [Visual Basic]
- error messages
- trappable errors
- errors [Visual Basic], trappable
ms.assetid: f2dda05b-baef-41f5-8bb1-598bd7cf239f
ms.openlocfilehash: c2d9974f41efdd321af800e6270586d9b18ba6f7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402844"
---
# <a name="error-messages-visual-basic"></a>Mensagens de erro (Visual Basic)
Quando você escreve, compila ou executa um aplicativo do Visual Basic, os seguintes tipos de erros podem ocorrer:  
  
1. Erros de tempo de design, que ocorrem quando você escreve um aplicativo no Visual Studio.  
  
2. Erros em tempo de compilação, que ocorrem quando você compila um aplicativo no Visual Studio ou no prompt de comando.  
  
3. Erros em tempo de execução, que ocorrem quando você executa um aplicativo no Visual Studio ou como um arquivo executável autônomo.  
  
 Para obter informações sobre como solucionar um erro específico, consulte [Recursos adicionais para programadores do Visual Basic](../../getting-started/additional-resources.md).  
  
## <a name="run-time-errors"></a>Erros em tempo de execução  
 Se um aplicativo Visual Basic tentar executar uma ação que o sistema não pode executar, ocorrerá um erro em tempo de execução e Visual Basic lançará um `Exception` objeto. Visual Basic pode gerar erros personalizados de qualquer tipo de dados, incluindo `Exception` objetos, usando a `Throw` instrução. Um aplicativo pode identificar o erro exibindo o número do erro e a mensagem de uma exceção capturada. Se não for detectado um erro, o aplicativo será encerrado.  
  
 O código pode interceptar e examine os erros em tempo de execução. Se você colocar o código que produz o erro em um bloco `Try`, você poderá capturar qualquer erro gerado em um bloco `Catch` correspondente. Para obter informações sobre como interceptar erros em tempo de execução e responder a eles em seu código, consulte [Instrução Try... Catch... Finally](../statements/try-catch-finally-statement.md).  
  
## <a name="compile-time-errors"></a>Erros no tempo de compilação  
 Se o compilador do Visual Basic encontrar um problema no código, ocorrerá um erro em tempo de compilação. No Editor de Código, você pode identificar facilmente qual linha de código causou o erro, pois uma linha ondulada aparece sob essa linha de código. A mensagem de erro será exibida se você apontar para a linha ondulada ou abrir a **Lista de Erros**, que também mostra outras mensagens.  
  
 Se um identificador tiver uma linha ondulada e um sublinhado curto exibidos abaixo do caractere mais à direita, você poderá gerar um stub para a classe, construtor, método, propriedade, campo ou enum. Para obter mais informações, consulte [Gerar do uso](/visualstudio/ide/visual-csharp-intellisense#generate-from-usage).
  
 Ao resolver os avisos do compilador do Visual Basic, você poderá escrever um código que é executado mais rápido e com menos erros. Esses avisos identificam códigos que podem causar erros quando o aplicativo é executado. Por exemplo, o compilador avisará se você tentar invocar um membro de uma variável de objeto não atribuída, retornar de uma função sem definir o valor retornado ou executar um bloco `Try` com erros na lógica para capturar exceções. Para obter mais informações sobre avisos, incluindo como ativar e desativar, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).
