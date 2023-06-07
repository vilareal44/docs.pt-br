---
title: ASP.NET Web API e WCF
description: Saiba se o WCF ou o ASP.NET Web API é mais adequado para suas necessidades, comparando os principais recursos de cada tecnologia.
ms.date: 03/30/2017
ms.assetid: 08ceded3-fd9a-4467-9715-c4cbd9c7228e
ms.openlocfilehash: de8d1905866c860da96983c2f3d52599e3342403
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245953"
---
# <a name="wcf-and-aspnet-web-api"></a>ASP.NET Web API e WCF
WCF é o modelo de programação unificada da Microsoft para a criação de aplicativos orientados a serviços. Ele permite aos desenvolvedores criarem soluções seguras, confiáveis e transacionadas que se integram nas plataformas e interoperam com os investimentos existentes. [ASP.NET Web API](https://www.asp.net/web-api) é uma estrutura que facilita a criação de serviços http que atingem uma ampla variedade de clientes, incluindo navegadores e dispositivos móveis. O ASP.NET Web API é uma plataforma ideal para o desenvolvimento de aplicativos RESTful no .NET Framework. Este tópico apresenta algumas diretrizes para ajudá-lo a decidir qual tecnologia atenderá melhor suas necessidades.  
  
## <a name="choosing-which-technology-to-use"></a>Escolhendo a tecnologia a ser usada  
 A tabela a seguir descreve os principais recursos de cada tecnologia.  
  
|WCF|ASP.NET Web API|  
|---------|---------------------|  
|Permite serviços de compilação que dão suporte a vários protocolos de transporte (HTTP, TCP, UDP e transportes personalizados) e permite a alternância entre eles.|Somente HTTP. Modelo de programação de primeira classe para HTTP. Mais adequado para o acesso de vários navegadores, dispositivos móveis, etc. habilitando o alcance amplo.|  
|Permite criar serviços que dão suporte a várias codificações (Text, MTOM e Binary) do mesmo tipo de mensagem e permite a alternância entre eles.|Permite criar as APIs da Web que dão suporte a ampla variedade de tipos de mídia que incluem XML, JSON etc.|  
|Dá suporte a serviços de compilação com padrões WS-* como Mensagens Confiáveis, Transações, Segurança da Mensagem.|Usa protocolo básico e formatos como HTTP, WebSockets, SSL, JSON e XML. Não há suporte para protocolos de nível mais alto como Mensagens Confiáveis ou Transações.|  
|Dá suporte a padrões de troca de mensagens solicitação-resposta, unidirecional e duplex.|O HTTP é uma solicitação/resposta, mas padrões adicionais podem ser suportados por meio da integração de [signalr](https://github.com/SignalR/SignalR) e WebSockets.|  
|Os serviços de WCF SOAP podem ser descritos no WSDL permitindo que as ferramentas automatizadas gerem proxies de cliente mesmo para os serviços com esquemas complexos.|Há várias maneiras de descrever uma API da Web variando de página de ajuda HTML gerada automaticamente descrevendo snippets para metadados estruturados para APIs integradas de OData.|  
|É fornecido com o .NET Framework.|O é fornecido com .NET Framework, mas é de software livre e também está disponível fora de banda como download independente.|  
  
 Use o WCF para criar serviços Web confiáveis e seguros que podem ser acessados por uma variedade de transportes. Use ASP.NET Web API para criar serviços baseados em HTTP que estejam acessíveis a partir de uma ampla variedade de clientes. Use o ASP.NET Web API se você estiver criando novos serviços de estilo REST. Embora o WCF forneça algum suporte para escrever serviços de estilo REST, o suporte para REST no ASP.NET Web API é mais completo e todas as melhorias futuras do recurso REST serão feitas no ASP.NET Web API. Se você tiver um serviço WCF existente e quiser expor pontos de extremidade adicionais REST, use WCF e o <xref:System.ServiceModel.WebHttpBinding>.  
  
## <a name="see-also"></a>Veja também

- [O que é o Windows Communication Foundation](whats-wcf.md)
- [Conceitos fundamentais do Windows Communication Foundation](fundamental-concepts.md)
