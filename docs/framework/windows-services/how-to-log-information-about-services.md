---
title: 'Como: Registrar em log informações sobre serviços'
description: Saiba como registrar em log informações sobre serviços. Defina a propriedade AutoLog se desejar que o projeto de serviço do Windows interaja com o log de eventos do aplicativo.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- AutoLog property
- services, logging information
- Windows Service applications, logging information about
- event logs, service applications
- application event logs, service applications
- logs, service applications
ms.assetid: c0d8140f-c055-4d8e-a2e0-37358a550116
author: ghogen
ms.openlocfilehash: 6ebce5464dc25ba4101b3898ee791714f8efc5d6
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925742"
---
# <a name="how-to-log-information-about-services"></a>Como: Registrar em log informações sobre serviços
Por padrão, todos os projetos de Serviço Windows têm a capacidade de interagir com o log de eventos do aplicativo e gravar informações e exceções. A propriedade <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> é usada para indicar se você deseja essa funcionalidade em seu aplicativo. Por padrão, o registro em log é habilitado para qualquer serviço criado com o modelo de projeto de Serviço Windows. Você pode usar uma forma estática da classe <xref:System.Diagnostics.EventLog> para gravar informações de serviço em um log sem precisar criar uma instância de um componente <xref:System.Diagnostics.EventLog> nem registrar uma fonte manualmente.  
  
 O instalador do serviço registra automaticamente cada serviço no projeto como uma fonte de eventos válida no log do aplicativo no computador no qual o serviço está instalado, quando o registro em log está habilitado. O serviço registra informações sempre que é iniciado, interrompido, colocado em pausa, retomado, instalado ou desinstalado. Ele também registra as falhas que ocorrem. Você não precisa escrever nenhum código para gravar entradas no log ao usar o comportamento padrão. O serviço faz isso automaticamente.  
  
 Se você quiser gravar em um log de eventos que não seja o log do aplicativo, será necessário definir a propriedade <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> como `false`, criar seu próprio log de eventos personalizado no código de serviços e registrar o serviço como uma fonte válida de entradas para esse log. Em seguida, você precisará escrever o código para registrar entradas no log sempre que ocorrer uma ação do seu interesse.  
  
> [!NOTE]
> Se você usar um log de eventos personalizado e configurar o aplicativo de serviço para gravar nele, não tente acessar o log de eventos antes de configurar a propriedade <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> do serviço no código. O log de eventos precisa do valor dessa propriedade para registrar o serviço como uma origem válida de eventos.  
  
### <a name="to-enable-default-event-logging-for-your-service"></a>Para habilitar o registro em log de eventos padrão para o serviço  
  
- Defina a propriedade <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> do componente para `true`.  
  
    > [!NOTE]
    > Por padrão, essa propriedade é definida como `true`. Você não precisa definir isso explicitamente, a menos que esteja criando um processamento mais complexo, como avaliar uma condição e, em seguida, definir a propriedade <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> com base no resultado dessa condição.  
  
### <a name="to-disable-event-logging-for-your-service"></a>Para desabilitar o registro em log de eventos para o serviço  
  
- Defina a propriedade <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> do componente para `false`.  
  
     [!code-csharp[VbRadconService#17](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#17)]
     [!code-vb[VbRadconService#17](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#17)]  
  
### <a name="to-set-up-logging-to-a-custom-log"></a>Para configurar o registro em um log personalizado  
  
1. Defina a propriedade <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> como `false`.  
  
    > [!NOTE]
    > Você precisa definir <xref:System.ServiceProcess.ServiceBase.AutoLog%2A> como false para usar um log personalizado.  
  
2. Configure uma instância de um componente <xref:System.Diagnostics.EventLog> no aplicativo de Serviço Windows.  
  
3. Crie um log personalizado chamando o método <xref:System.Diagnostics.EventLog.CreateEventSource%2A> e especificando a cadeia de caracteres de origem e o nome do arquivo de log que você deseja criar.  
  
4. Defina a propriedade <xref:System.Diagnostics.EventLog.Source%2A> na instância do componente <xref:System.Diagnostics.EventLog> para a cadeia de caracteres de origem que você criou na etapa 3.  
  
5. Grave as entradas, acessando o método <xref:System.Diagnostics.EventLog.WriteEntry%2A> na instância do componente <xref:System.Diagnostics.EventLog>.  
  
     O código a seguir mostra como configurar o registro em um log personalizado.  
  
    > [!NOTE]
    > Neste exemplo de código, uma instância de um componente <xref:System.Diagnostics.EventLog> é chamada de `eventLog1` (`EventLog1` em Visual Basic). Se você criar uma instância com outro nome na etapa 2, altere o código adequadamente.  
  
     [!code-csharp[VbRadconService#14](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#14)]
     [!code-vb[VbRadconService#14](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#14)]  
    [!code-csharp[VbRadconService#15](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#15)]
    [!code-vb[VbRadconService#15](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#15)]  
  
## <a name="see-also"></a>Veja também

- [Introdução a aplicativos do Serviço Windows](introduction-to-windows-service-applications.md)
