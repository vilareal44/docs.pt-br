---
title: 'Como: Receber notificações de exceção de primeira tentativa'
description: Obtenha notificações de exceção de primeira chance no .NET por meio do evento FirstChanceException da classe AppDomain, antes que o CLR pesquise manipuladores de exceção.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- first-chance exception notifications
- exceptions, first chance notifications
ms.assetid: 66f002b8-a97d-4a6e-a503-2cec01689113
ms.openlocfilehash: e8b5ae5fb69c7befd329316aee11523f79d73fcd
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85104735"
---
# <a name="how-to-receive-first-chance-exception-notifications"></a>Como: Receber notificações de exceção de primeira tentativa
O evento <xref:System.AppDomain.FirstChanceException> da classe <xref:System.AppDomain> permite que você receba uma notificação de que uma exceção foi lançada, antes de o Common Language Runtime começar a procurar por manipuladores de exceção.

 O evento é gerado no nível de domínio do aplicativo. Um thread de execução pode passar por vários domínios de aplicativo, assim, uma exceção não tratada em um domínio de aplicativo poderia ser tratada em outro domínio de aplicativo. A notificação ocorre em cada domínio de aplicativo que tenha adicionado um manipulador para o evento, até que um domínio de aplicativo lide com a exceção.

 Os procedimentos e os exemplos neste artigo mostram como receber notificações de exceções de primeira tentativa em um programa simples que tem um domínio de aplicativo, e em um domínio de aplicativo que você criar.

 Para obter um exemplo mais complexo que abrange vários domínios de aplicativo, veja o exemplo para o evento <xref:System.AppDomain.FirstChanceException>.

## <a name="receiving-first-chance-exception-notifications-in-the-default-application-domain"></a>Receber notificações de exceção de primeira tentativa no Domínio do aplicativo padrão
 No procedimento a seguir, o ponto de entrada para o aplicativo, o método `Main()`, é executado no domínio do aplicativo padrão.

#### <a name="to-demonstrate-first-chance-exception-notifications-in-the-default-application-domain"></a>Para demonstrar notificações de exceção de primeira tentativa no domínio do aplicativo padrão

1. Defina um manipulador de eventos para o evento <xref:System.AppDomain.FirstChanceException> usando uma função lambda e anexe-o ao evento. Neste exemplo, o manipulador de eventos imprime o nome do domínio do aplicativo no qual o evento foi tratado e a propriedade <xref:System.Exception.Message%2A> da exceção.

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#2)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#2)]

2. Gere uma exceção e capture-a. Antes de o runtime localizar o manipulador de exceção, o evento <xref:System.AppDomain.FirstChanceException> é gerado e exibe uma mensagem. Essa mensagem é seguida pela mensagem exibida pelo manipulador de exceção.

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#3)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#3)]

3. Gere uma exceção, mas não a capture. Antes de o runtime procurar o manipulador de exceção, o evento <xref:System.AppDomain.FirstChanceException> é gerado e exibe uma mensagem. Não há um manipulador de exceções, portanto, o aplicativo será encerrado.

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#4)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#4)]

     O código que é mostrado nas três primeiras etapas deste procedimento forma um aplicativo de console completo. A saída do aplicativo varia, dependendo do nome do arquivo .exe, pois o nome do domínio do aplicativo padrão é formado pelo nome e pela extensão do arquivo .exe. Veja o seguinte para a saída de exemplo.

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#5)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#5)]

## <a name="receiving-first-chance-exception-notifications-in-another-application-domain"></a>Receber notificações de exceção de primeira tentativa em outro Domínio do aplicativo
 Se seu programa contiver mais de um domínio de aplicativo, escolha quais domínios de aplicativo recebem notificações.

#### <a name="to-receive-first-chance-exception-notifications-in-an-application-domain-that-you-create"></a>Para receber notificações de exceção de primeira tentativa em outro Domínio do aplicativo criado por você

1. Defina um manipulador de eventos para o evento <xref:System.AppDomain.FirstChanceException>. Este exemplo usa um método `static` (método `Shared` no Visual Basic) que imprime o nome do domínio do aplicativo no qual o evento foi tratado e a propriedade <xref:System.Exception.Message%2A> da exceção.

     [!code-csharp[System.AppDomain.FirstChanceException_howto#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#3)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#3)]

2. Crie um domínio do aplicativo e adicione o manipulador de eventos ao evento <xref:System.AppDomain.FirstChanceException> para esse domínio do aplicativo. Neste exemplo, o domínio de aplicativo é chamado `AD1`.

     [!code-csharp[System.AppDomain.FirstChanceException_howto#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#2)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#2)]

     Você pode manipular esse evento no domínio do aplicativo padrão da mesma maneira. Use a propriedade <xref:System.AppDomain.CurrentDomain%2A?displayProperty=nameWithType>`static` (`Shared` no Visual Basic) no `Main()` para obter uma referência para o domínio do aplicativo padrão.

#### <a name="to-demonstrate-first-chance-exception-notifications-in-the-application-domain"></a>Para demonstrar notificações de exceção de primeira tentativa no domínio do aplicativo

1. Crie um objeto `Worker` no domínio do aplicativo que você criou no procedimento anterior. A classe `Worker` deve ser pública e deve derivar de <xref:System.MarshalByRefObject>, conforme mostra o exemplo completo no final deste artigo.

     [!code-csharp[System.AppDomain.FirstChanceException_howto#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#4)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#4)]

2. Chame um método do objeto `Worker` que gera uma exceção. Nesse exemplo, o método `Thrower` é chamado duas vezes. Na primeira vez, o argumento do método é `true`, que faz com que o método capture sua própria exceção. Na segunda vez, o argumento é `false`, e o método `Main()` captura a exceção no domínio de aplicativo padrão.

     [!code-csharp[System.AppDomain.FirstChanceException_howto#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#6)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#6)]

3. Coloque o código no método `Thrower` para controlar se o método lida com sua própria exceção.

     [!code-csharp[System.AppDomain.FirstChanceException_howto#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#5)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#5)]

## <a name="example"></a>Exemplo
 O exemplo a seguir cria um domínio do aplicativo chamado `AD1` e adiciona o manipulador de eventos ao evento <xref:System.AppDomain.FirstChanceException> do domínio do aplicativo. O exemplo cria uma instância da classe `Worker` no domínio do aplicativo e chama um método chamado `Thrower`, que gera uma <xref:System.ArgumentException>. Dependendo do valor do argumento, o método captura a exceção ou não consegue lidar com ela.

 Cada vez que o método `Thrower` gera uma exceção no `AD1`, o evento <xref:System.AppDomain.FirstChanceException> é gerado no `AD1` e o manipulador de eventos exibe uma mensagem. Em seguida, o runtime procura um manipulador de exceção. No primeiro caso, o manipulador de exceção é encontrado em `AD1`. No segundo caso, a exceção é tratada no `AD1` e, em vez disso, é capturada no domínio do aplicativo padrão.

> [!NOTE]
> O nome do domínio do aplicativo padrão é igual ao nome do executável.

 Se você adicionar um manipulador ao evento <xref:System.AppDomain.FirstChanceException> para o domínio do aplicativo padrão, o evento será disparado e tratado antes de o domínio de aplicativo padrão lidar com a exceção. Para ver isso, adicione o código C# `AppDomain.CurrentDomain.FirstChanceException += FirstChanceException;` (no Visual Basic, `AddHandler AppDomain.CurrentDomain.FirstChanceException, FirstChanceException`) no início de `Main()`.

 [!code-csharp[System.AppDomain.FirstChanceException_howto#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#1)]
 [!code-vb[System.AppDomain.FirstChanceException_howto#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.AppDomain.FirstChanceException>
