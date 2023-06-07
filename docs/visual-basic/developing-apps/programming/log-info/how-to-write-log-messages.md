---
title: 'Como: gravar mensagens de log'
ms.date: 07/20/2015
helpviewer_keywords:
- My.Application.Log object, writing log messages
ms.assetid: 972a3e0c-2996-4623-a7a9-d7ebc4d207f8
ms.openlocfilehash: 28c5fe77e2711dfcac96e621a90fb9506eb74775
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410043"
---
# <a name="how-to-write-log-messages-visual-basic"></a>Como gravar mensagens de log (Visual Basic)

É possível usar os objetos `My.Application.Log` e `My.Log` para registrar em log informações sobre seu aplicativo. Este exemplo mostra como usar o método `My.Application.Log.WriteEntry` para registrar em log informações de rastreamento.

Para registrar informações de exceção em log, use o método `My.Application.Log.WriteException`; consulte [How to: Log Exceptions (Como registrar em log as exceções)](how-to-log-exceptions.md).

## <a name="example"></a>Exemplo

Este exemplo usa o método `My.Application.Log.WriteEntry` para gravar as informações de rastreamento.

[!code-vb[VbVbalrMyApplicationLog#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#11)]

## <a name="net-framework-security"></a>Segurança do .NET Framework

Certifique-se de que os dados gravados no log não incluem informações confidenciais como senhas de usuário. Para obter mais informações, consulte [Working with Application Logs (Trabalhando com logs de aplicativo)](working-with-application-logs.md).

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A>
- [Trabalhar com logs do aplicativo](working-with-application-logs.md)
- [Como: registrar exceções em log](how-to-log-exceptions.md)
- [Passo a passo: determinar o local no qual My.Application.Log grava informações](walkthrough-determining-where-my-application-log-writes-information.md)
- [Passo a passo: alterar o local no qual My.Application.Log grava informações](walkthrough-changing-where-my-application-log-writes-information.md)
- [Passo a passo: filtrar a saída de My.Application.Log](walkthrough-filtering-my-application-log-output.md)
