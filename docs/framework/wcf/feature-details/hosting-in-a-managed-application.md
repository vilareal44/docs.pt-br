---
title: Hospedagem em um aplicativo gerenciado
ms.date: 03/30/2017
ms.assetid: af70132d-e9e1-4f32-b20f-f0014629758a
ms.openlocfilehash: 759257edf89d1af5c453bb98f41361b54cf113ae
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597339"
---
# <a name="hosting-in-a-managed-application"></a>Hospedagem em um aplicativo gerenciado
Os serviços do Windows Communication Foundation (WCF) podem ser hospedados em qualquer aplicativo .NET Framework. Os serviços de hospedagem interna são a opção de hospedagem mais flexível, pois exigem a implantação mínima da infraestrutura. No entanto, ela também é a opção de hospedagem menos robusta, porque os aplicativos gerenciados não fornecem recursos avançados de hospedagem e gerenciamento de outras opções de hospedagem no WCF, como Serviços de Informações da Internet (IIS) e serviços do Windows.  
  
 Para criar um serviço auto-hospedado, crie e abra uma instância do <xref:System.ServiceModel.ServiceHost> , que inicia um serviço de escuta de mensagens. Para obter mais informações, consulte [como hospedar um serviço WCF em um aplicativo gerenciado](../how-to-host-a-wcf-service-in-a-managed-application.md).  
  
 Para obter um exemplo completo sobre como definir um contrato, implementar o contrato e hospedar um serviço dentro de um aplicativo gerenciado, consulte o [Tutorial introdução](../getting-started-tutorial.md) e o [autohost](../samples/self-host.md).  
  
 As seções a seguir descrevem cenários comuns que usam essa opção de hospedagem.  
  
## <a name="console-applications"></a>Aplicativos de console  
 Cenários comuns que o auto-Hosting permite são serviços WCF em execução dentro de aplicativos de console. Hospedar um serviço WCF dentro de um aplicativo de console normalmente é útil durante a fase de desenvolvimento do serviço. Isso facilita a depuração, fácil de obter informações de rastreamento do para descobrir o que está acontecendo dentro do aplicativo e é fácil de mover ao copiá-los para novos locais.  
  
## <a name="rich-client-applications"></a>Aplicativos cliente avançados  
 Outros cenários comuns que a hospedagem interna habilita são aplicativos cliente avançados, como aqueles baseados em Windows Presentation Foundation (WPF) ou Windows Forms (WinForms). Essa opção de hospedagem também torna mais fácil para aplicativos cliente avançados, como aplicativos do WPF e WinForms, para se comunicar com o mundo exterior. Por exemplo, um cliente de colaboração ponto a ponto que usa o WPF para sua interface do usuário e também hospeda um serviço WCF que permite que outros clientes se conectem a ele e compartilhem informações.  
  
## <a name="see-also"></a>Consulte também

- [Hospedando serviços](../hosting-services.md)
- [Tutorial de Introdução](../getting-started-tutorial.md)
