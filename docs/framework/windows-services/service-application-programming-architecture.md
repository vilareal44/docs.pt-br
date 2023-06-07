---
title: Arquitetura de programação do aplicativo de serviço
description: Entenda a arquitetura de programação do aplicativo de serviço. Os aplicativos de serviço do Windows são baseados em uma classe que herda de System. ServiceProcess. ServiceBase.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ServiceController components, programming architecture
- ServiceBase class, service states
- Windows Service applications, code model
- services, programming architecture
- ServiceController class
- services, states
- ServiceProcessInstaller class, service application code model
- Windows Service applications, states
ms.assetid: 83230026-d068-4174-97ff-e264c896eb2f
author: ghogen
ms.openlocfilehash: c59ccc5a8b2f11fda9c4734487092c1aabb74908
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925573"
---
# <a name="service-application-programming-architecture"></a>Arquitetura de programação do aplicativo de serviço
Os aplicativos de Serviço Windows baseiam-se em uma classe que é herdada da classe <xref:System.ServiceProcess.ServiceBase?displayProperty=nameWithType>. Você substitui os métodos dessa classe e define funcionalidades para eles para determinar o comportamento do seu serviço.  
  
 As classes principais envolvidas na criação do serviço são:  
  
- <xref:System.ServiceProcess.ServiceBase?displayProperty=nameWithType> – você substitui os métodos da classe <xref:System.ServiceProcess.ServiceBase> ao criar um serviço e define o código para determinar como o seu serviço funciona nessa classe herdada.  
  
- <xref:System.ServiceProcess.ServiceProcessInstaller?displayProperty=nameWithType> e <xref:System.ServiceProcess.ServiceInstaller?displayProperty=nameWithType> – você usa essas classes para instalar e desinstalar o serviço.  
  
 Além disso, uma classe chamada <xref:System.ServiceProcess.ServiceController> pode ser usada para manipular o serviço sozinha. Essa classe não é envolvida na criação de um serviço, mas pode ser usada para iniciar e parar o serviço, passar comandos para ele e retornar uma série de enumerações.  
  
## <a name="defining-your-services-behavior"></a>Definindo o comportamento do serviço  
 Em sua classe de serviço, você substitui as funções de classe base que determinam o que acontece quando o estado do serviço é alterado no Gerenciador de Controle de Serviços. A classe <xref:System.ServiceProcess.ServiceBase> expõe os métodos a seguir, que você pode substituir para adicionar o comportamento personalizado.  
  
|Método|Substituir para|  
|------------|-----------------|  
|<xref:System.ServiceProcess.ServiceBase.OnStart%2A>|Indica quais ações devem ser tomadas quando o serviço é iniciado. Você precisa escrever o código neste procedimento para que o serviço realize um trabalho útil.|  
|<xref:System.ServiceProcess.ServiceBase.OnPause%2A>|Indica o que deve acontecer quando o serviço está em pausa.|  
|<xref:System.ServiceProcess.ServiceBase.OnStop%2A>|Indica o que deve acontecer quando a execução do serviço é interrompida.|  
|<xref:System.ServiceProcess.ServiceBase.OnContinue%2A>|Indica o que deve acontecer quando o serviço continua o funcionamento normal depois de ser colocado em pausa.|  
|<xref:System.ServiceProcess.ServiceBase.OnShutdown%2A>|Indica o que deverá acontecer antes do sistema ser desligado, se o serviço estiver em execução no momento.|  
|<xref:System.ServiceProcess.ServiceBase.OnCustomCommand%2A>|Indica o que deve acontecer quando o serviço recebe um comando personalizado. Para obter mais informações sobre comandos personalizados, consulte o MSDN online.|  
|<xref:System.ServiceProcess.ServiceBase.OnPowerEvent%2A>|Indica como o serviço deve responder quando um evento de gerenciamento de energia é recebido, como bateria fraca ou operação de suspensão.|  
  
> [!NOTE]
> Esses métodos representam os estados pelos quais o serviço passa em seu tempo de vida. O serviço faz a transição de um estado para o próximo. Por exemplo, você nunca fará com que o serviço responda a um comando <xref:System.ServiceProcess.ServiceBase.OnContinue%2A> antes que <xref:System.ServiceProcess.ServiceBase.OnStart%2A> seja chamado.  
  
 Há várias outras propriedades e métodos interessantes. Elas incluem:  
  
- O método <xref:System.ServiceProcess.ServiceBase.Run%2A> na classe <xref:System.ServiceProcess.ServiceBase>. Este é o ponto de entrada principal para o serviço. Quando você cria um serviço usando o modelo de Serviço Windows, o código é inserido no método `Main` do aplicativo para executar o serviço. Esse código tem esta aparência:  
  
     [!code-csharp[VbRadconService#6](../../../samples/snippets/csharp/VS_Snippets_VBCSharp/VbRadconService/CS/MyNewService.cs#6)]
     [!code-vb[VbRadconService#6](../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbRadconService/VB/MyNewService.vb#6)]  
  
    > [!NOTE]
    > Esses exemplos usam uma matriz do tipo <xref:System.ServiceProcess.ServiceBase>, na qual cada serviço que o aplicativo contém pode ser adicionado e, em seguida, todos os serviços podem ser executados juntos. No entanto, se você estiver criando apenas um único serviço, poderá decidir não usar a matriz e simplesmente declarar um novo objeto herdado de <xref:System.ServiceProcess.ServiceBase> e, em seguida, executá-lo. Para obter um exemplo, confira [Como escrever serviços de forma programática](how-to-write-services-programmatically.md).  
  
- Uma série de propriedades na classe <xref:System.ServiceProcess.ServiceBase>. Elas determinam quais métodos podem ser chamados no serviço. Por exemplo, quando a propriedade <xref:System.ServiceProcess.ServiceBase.CanStop%2A> está definida como `true`, o método <xref:System.ServiceProcess.ServiceBase.OnStop%2A> no serviço pode ser chamado. Quando a propriedade <xref:System.ServiceProcess.ServiceBase.CanPauseAndContinue%2A> está definida como `true`, os métodos <xref:System.ServiceProcess.ServiceBase.OnPause%2A> e <xref:System.ServiceProcess.ServiceBase.OnContinue%2A> podem ser chamados. Ao definir uma dessas propriedades para `true`, você deverá substituir e definir o processamento dos métodos associados.  
  
    > [!NOTE]
    > O serviço precisará substituir pelo menos <xref:System.ServiceProcess.ServiceBase.OnStart%2A> e <xref:System.ServiceProcess.ServiceBase.OnStop%2A> para ser útil.  
  
 Você também pode usar um componente chamado <xref:System.ServiceProcess.ServiceController> para comunicar-se com um serviço existente e controlar seu comportamento.  
  
## <a name="see-also"></a>Veja também

- [Introdução a aplicativos do Serviço Windows](introduction-to-windows-service-applications.md)
- [Como: Criar serviços Windows](how-to-create-windows-services.md)
