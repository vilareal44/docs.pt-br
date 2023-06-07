---
title: Hospedagem no serviço de ativação do processo do Windows
description: Saiba como o WAS gerencia a ativação e o tempo de vida dos processos de trabalho que contêm aplicativos que hospedam serviços WCF.
ms.date: 03/30/2017
helpviewer_keywords:
- hosting services [WCF], WAS
ms.assetid: d2b9d226-15b7-41fc-8c9a-cb651ac20ecd
ms.openlocfilehash: 6b0b23c21762009341fd62c029431824dd26d6c3
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247254"
---
# <a name="hosting-in-windows-process-activation-service"></a>Hospedagem no serviço de ativação do processo do Windows
O Serviço de Ativação de Processos do Windows (WAS) gerencia a ativação e a vida útil dos processos de trabalho que contêm aplicativos que hospedam os serviços do Windows Communication Foundation (WCF). O modelo de processo WAS generaliza o modelo de processo IIS 6.0 para o servidor HTTP ao remover a dependência do HTTP. Isso permite que os serviços WCF usem protocolos HTTP e não HTTP, como net. TCP, em um ambiente de hospedagem que ofereça suporte à ativação baseada em mensagem e ofereça a capacidade de hospedar um grande número de aplicativos em um determinado computador.  
  
 Para obter mais informações sobre como criar um serviço WCF que é executado no ambiente de hospedagem WAS, consulte [como hospedar um serviço WCF no was](how-to-host-a-wcf-service-in-was.md).  
  
 O modelo de processo WAS fornece vários recursos que permitem que os aplicativos sejam hospedados de forma mais robusta, mais gerenciável e que usa recursos com eficiência:  
  
- A ativação baseada em mensagem de aplicativos e aplicativos de processo de trabalho iniciam e param dinamicamente em resposta a itens de trabalho recebidos que chegam usando protocolos de rede HTTP e não HTTP.  
  
- Reciclagem de processo de trabalho e aplicativo robusto para manter a integridade dos aplicativos em execução.  
  
- Configuração e gerenciamento de aplicativos centralizados.  
  
- Permite que os aplicativos aproveitem o modelo de processo do IIS sem exigir a superfície de implantação de uma instalação completa do IIS.  
O [Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ff384253(v=azure.10)) funciona com o IIS 7,0 e o WAS (serviço de ativação de processos do Windows) para fornecer um ambiente de Hospedagem de aplicativos avançado para serviços WCF e WF do NET4. Esses benefícios incluem gerenciamento do ciclo de vida do processo, reciclagem de processo, hospedagem compartilhada, proteção rápida de falhas, órfão de processo, ativação sob demanda e monitoramento de integridade. Para obter informações detalhadas, consulte [recursos de hospedagem do AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10)) e conceitos de hospedagem do [AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677371(v=azure.10)).  
  
## <a name="elements-of-the-was-addressing-model"></a>Elementos do modelo de endereçamento WAS  
 Os aplicativos têm endereços Uniform Resource Identifier (URI), que são as unidades de código cujo tempo de vida e o ambiente de execução são gerenciados pelo servidor. Uma única instância do servidor WAS pode ser doméstica a vários aplicativos diferentes. Os servidores organizam aplicativos em grupos chamados *sites*. Dentro de um site, os aplicativos são organizados de maneira hierárquica que reflete a estrutura dos URIs que servem como seus endereços externos.  
  
 Os endereços de aplicativo têm duas partes: um prefixo de URI de base e um endereço relativo (caminho) específico do aplicativo, que fornece o endereço externo para um aplicativo quando Unido. O prefixo URI de base é construído a partir da associação do site e é usado para todos os aplicativos no site. Em seguida, os endereços de aplicativo são construídos por meio de fragmentos de caminho específicos do aplicativo (como "/applicationOne") e acrescentando-os ao prefixo de URI de base (por exemplo, "net. TCP://localhost") para chegar ao URI completo do aplicativo.  
  
 A tabela a seguir ilustra vários cenários de endereçamento possíveis para sites do WAS com associações de site HTTP e não HTTP.  
  
|Cenário|Associações de site|Caminho do aplicativo|URIs de aplicativo base|  
|--------------|-------------------|----------------------|---------------------------|  
|Somente HTTP|http: *: 80:\*|/appTwo|`http://localhost/appTwo/`|  
|HTTP e não HTTP|http: *: 80:\*<br /><br /> NET. TCP: 808:\*|/appTwo|`http://localhost/appTwo/`<br />`net.tcp://localhost/appTwo/`|  
|Não somente HTTP|NET. pipe: *|/appThree|`net.pipe://appThree/`|  
  
 Os serviços e recursos dentro de um aplicativo também podem ser resolvidos. Em um aplicativo, os recursos do aplicativo são endereçados em relação ao caminho do aplicativo base. Por exemplo, suponha que um site em um nome de computador contoso.com tenha associações de site para os protocolos HTTP e net. TCP. Além disso, suponha que o site contenha um aplicativo localizado em/Billing, que expõe um serviço em GetOrders. svc. Em seguida, se o serviço GetOrders. svc tiver exposto um ponto de extremidade com um endereço relativo de SecureEndpoint, o ponto de extremidade de serviço será exposto nos dois URIs a seguir:  
  
- `http://contoso.com/Billing/GetOrders.svc/SecureEndpoint`
- `net.tcp://contoso.com/Billing/GetOrders.svc/SecureEndpoint`
  
## <a name="the-was-runtime"></a>O tempo de execução do WAS  
 Os aplicativos são organizados em sites para fins de endereçamento e gerenciamento. Em tempo de execução, os aplicativos também são agrupados em pools de aplicativos. Um pool de aplicativos pode alojar vários aplicativos diferentes de vários sites diferentes. Todos os aplicativos dentro de um pool de aplicativos compartilham um conjunto comum de características de tempo de execução. Por exemplo, todos eles são executados na mesma versão do Common Language Runtime (CLR) e todos compartilham uma identidade de processo comum. Cada pool de aplicativos corresponde a uma instância de um processo de trabalho (w3wp.exe). Cada aplicativo gerenciado em execução dentro de um pool de aplicativos compartilhado é isolado de outros aplicativos por meio de um AppDomain CLR.  
  
## <a name="see-also"></a>Veja também

- [Arquitetura de ativação do WAS](was-activation-architecture.md)
- [Configurar o WAS para uso com o WCF](configuring-the-wpa--service-for-use-with-wcf.md)
- [Como instalar e configurar os componentes de ativação do WCF](how-to-install-and-configure-wcf-activation-components.md)
- [Como hospedar um serviço do WCF no WAS](how-to-host-a-wcf-service-in-was.md)
- [Recursos de hospedagem do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677189(v=azure.10))
