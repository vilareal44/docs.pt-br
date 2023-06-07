---
title: Hospedagem no Internet Information Services
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF], IIS
ms.assetid: ddae14e8-143c-442d-b660-2046809b2d43
ms.openlocfilehash: baf13af39fe575a75f1304b21f3b4ad70dd370ab
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597313"
---
# <a name="host-in-internet-information-services"></a>Host no Serviços de Informações da Internet

Uma opção para hospedar serviços de Windows Communication Foundation (WCF) está dentro de um aplicativo de Serviços de Informações da Internet (IIS). Esse modelo de hospedagem é semelhante ao modelo usado pelos serviços Web ASP.NET e ASP.NET Web Services (ASMX).

## <a name="versions-of-iis"></a>Versões do IIS

O WCF pode ser hospedado nas seguintes versões do IIS nos seguintes sistemas operacionais:

- IIS 5,1 no Windows XP SP2. Esse ambiente é útil para o design e o desenvolvimento de aplicativos hospedados no IIS que são implantados posteriormente em um sistema operacional de servidor, como o Windows Server 2003.

- IIS 6,0 no Windows Server 2003. O IIS 6,0 fornece um modelo de processo avançado que oferece escalabilidade, confiabilidade e isolamento de aplicativos aprimorados. Esse ambiente é adequado para a implantação de produção de serviços WCF que usam a comunicação HTTP exclusivamente.

- IIS 7,0 no Windows Vista e no Windows Server 2008. O IIS 7,0 fornece o mesmo modelo de processo avançado que o IIS 6,0, mas usa o WAS (serviço de ativação de processos do Windows) para permitir a ativação e a comunicação de rede por meio de protocolos diferentes de HTTP. Esse ambiente é adequado para o desenvolvimento de serviços WCF que se comunicam em qualquer protocolo de rede com suporte do WCF (incluindo HTTP, net. TCP, net. pipe e net. MSMQ). Para obter mais informações sobre o WAS, consulte [hospedagem no serviço de ativação de processos do Windows](hosting-in-windows-process-activation-service.md).

- O [Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10)) funciona com o IIS 7,0 e o WAS (serviço de ativação de processos do Windows) para fornecer um ambiente de Hospedagem de aplicativos avançado para serviços WCF e WF do NET4. Esses benefícios incluem gerenciamento do ciclo de vida do processo, reciclagem de processo, hospedagem compartilhada, proteção rápida de falhas, órfão de processo, ativação sob demanda e monitoramento de integridade. Para obter informações detalhadas, consulte [recursos de hospedagem do AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10)) e conceitos de hospedagem do [AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677371(v=azure.10)).

## <a name="benefits-of-iis-hosting"></a>Benefícios da hospedagem do IIS

Hospedar serviços WCF no IIS tem vários benefícios:

- Os serviços WCF hospedados no IIS são implantados e gerenciados como qualquer outro tipo de aplicativo do IIS, incluindo aplicativos ASP.NET e ASMX.

- O IIS fornece ativação de processos, gerenciamento de integridade e recursos de reciclagem para aumentar a confiabilidade dos aplicativos hospedados.

- Como ASP.NET, os serviços WCF hospedados no ASP.NET podem aproveitar o modelo de hospedagem compartilhado ASP.NET, em que vários aplicativos residem em um processo de trabalho comum para aumentar a densidade e a escalabilidade do servidor.

- Os serviços WCF hospedados no IIS usam o mesmo modelo de compilação dinâmica que o ASP.NET 2,0, que simplifica o desenvolvimento e a implantação de serviços hospedados.

Ao decidir hospedar serviços WCF no IIS, é importante lembrar que o IIS 5,1 e o IIS 6,0 são limitados apenas à comunicação HTTP. Para obter mais informações sobre como escolher um ambiente de hospedagem, consulte [serviços de hospedagem](../hosting-services.md).

## <a name="deploy-an-iis-hosted-wcf-service"></a>Implantar um serviço WCF hospedado pelo IIS

O desenvolvimento e a implantação de um serviço WCF hospedado pelo IIS consiste nas seguintes tarefas:

- Verifique se o IIS, o ASP.NET, o WCF e o componente de ativação HTTP do WCF estão corretamente instalados e registrados.

- Crie um novo aplicativo do IIS ou reutilize um aplicativo ASP.NET existente.

- Crie um arquivo. svc para o serviço WCF.

- Implantar a implementação do serviço para o aplicativo do IIS.

- Configure o serviço WCF.

Para obter uma discussão sobre cada uma dessas tarefas, consulte [implantando um serviço WCF hospedado por serviços de informações da Internet](deploying-an-internet-information-services-hosted-wcf-service.md).

## <a name="wcf-services-and-aspnet"></a>Serviços WCF e ASP.NET

Os serviços WCF podem ser hospedados lado a lado com o ASP.NET ou no modo de compatibilidade ASP.NET, no qual os serviços podem aproveitar ao máximo os recursos fornecidos pela plataforma de aplicativo Web ASP.NET. Para obter uma discussão sobre esses recursos, consulte [Serviços WCF e ASP.net](wcf-services-and-aspnet.md).

## <a name="see-also"></a>Consulte também

- [Estendendo a hospedagem usando ServiceHostFactory](../extending/extending-hosting-using-servicehostfactory.md)
- [Implantando um serviço WCF hospedado em Serviços de Informações da Internet](deploying-an-internet-information-services-hosted-wcf-service.md)
- [Serviços WCF e ASP.NET](wcf-services-and-aspnet.md)
- [Práticas recomendadas de hospedagem de Serviços de Informações da Internet](internet-information-services-hosting-best-practices.md)
- [Configurando os Serviços de Informações da Internet 7.0 para Windows Communication Foundation](configuring-iis-for-wcf.md)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
