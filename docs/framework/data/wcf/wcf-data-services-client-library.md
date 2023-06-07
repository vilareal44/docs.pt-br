---
title: Biblioteca de cliente do WCF Data Services
description: Saiba como usar WCF Data Services bibliotecas de cliente para acessar e alterar dados de um aplicativo cliente .NET Framework.
ms.date: 03/30/2017
helpviewer_keywords:
- WCF Data Services, client library
- DataServiceQuery class, about DataServiceQuery class
- DataServiceContext class, about DataServiceContext class
ms.assetid: 21075e50-8917-413e-a8ea-35a0f6e65aa5
ms.openlocfilehash: 58d038d5c2ac4973c2b41f4d49c1746f48f2a2fb
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247734"
---
# <a name="wcf-data-services-client-library"></a>Biblioteca de cliente do WCF Data Services
Qualquer aplicativo pode interagir com um serviço de dados baseado em Protocolo Open Data (OData) se ele puder enviar uma solicitação HTTP e processar o feed OData que um serviço de dados retorna. Essa interoperabilidade permite acessar serviços baseados em OData de uma ampla variedade de aplicativos habilitados para a Web. A WCF Data Services inclui bibliotecas de cliente que fornecem uma experiência de programação mais rica quando você consome feeds OData de aplicativos baseados em .NET Framework ou Silverlight.  
  
 As duas principais classes de biblioteca de cliente são as classes <xref:System.Data.Services.Client.DataServiceContext> e <xref:System.Data.Services.Client.DataServiceQuery%601>. A classe <xref:System.Data.Services.Client.DataServiceContext> encapsula as operações que têm suporte em um serviço de dados especificado. Embora os serviços OData sejam sem estado, o contexto não é. Portanto, você pode usar a <xref:System.Data.Services.Client.DataServiceContext> classe para manter o estado no cliente entre as interações com o serviço de dados para dar suporte a recursos como o gerenciamento de alterações. Essa classe também gerencia identidades e rastreia alterações. A classe <xref:System.Data.Services.Client.DataServiceQuery%601> representa uma consulta em um conjunto de entidades específico.  
  
 Esta seção descreve como usar bibliotecas de cliente para acessar e modificar dados de um aplicativo cliente do .NET Framework. Para obter mais informações sobre como usar a biblioteca de cliente do WCF Data Services com um aplicativo baseado no Silverlight, consulte [WCF Data Services (Silverlight)](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc838234(v%3dvs.95)). Outras bibliotecas de cliente estão disponíveis para permitir que você consuma um feed OData em outros tipos de aplicativos. Para obter mais informações sobre o SDK do OData, consulte [SDK do OData – código de exemplo](https://www.odata.org/ecosystem/#sdk).
  
## <a name="in-this-section"></a>Nesta seção  
 [Gerando a biblioteca do cliente de serviço de dados](generating-the-data-service-client-library-wcf-data-services.md)  
 Descreve como gerar uma biblioteca de cliente e classes de serviço de dados do cliente baseadas em feeds OData.  
  
 [Consultar o serviço de dados](querying-the-data-service-wcf-data-services.md)  
 Descreve como consultar um serviço de dados de um aplicativo com base no .NET Framework usando bibliotecas de cliente.  
  
 [Carregar conteúdo adiado](loading-deferred-content-wcf-data-services.md)  
 Descreve como carregar o conteúdo adicional não incluído na resposta da consulta inicial.  
  
 [Atualizar o serviço de dados](updating-the-data-service-wcf-data-services.md)  
 Descreve como criar, modificar e excluir entidades e relações usando as bibliotecas de cliente.  
  
 [Operações assíncronas](asynchronous-operations-wcf-data-services.md)  
 Descreve os recursos fornecidos pelas bibliotecas de cliente para trabalhar com um serviço de dados de uma maneira assíncrona.  
  
 [Operações de envio em lote](batching-operations-wcf-data-services.md)  
 Descreve como enviar várias solicitações para o serviço de dados em um único lote usando as bibliotecas de cliente.  
  
 [Associando dados a controles](binding-data-to-controls-wcf-data-services.md)  
 Descreve como associar controles a um feed OData retornado por um serviço de dados.  
  
 [Chamar operações de serviço](calling-service-operations-wcf-data-services.md)  
 Descreve como usar o proxy do cliente de biblioteca para chamar operações de serviço.  
  
 [Gerenciar o contexto do serviço de dados](managing-the-data-service-context-wcf-data-services.md)  
 Descreve as opções para gerenciar o comportamento da biblioteca de cliente.  
  
 [Trabalhando com dados binários](working-with-binary-data-wcf-data-services.md)  
 Descreve como acessar e alterar os dados binários retornados pelo serviço de dados como um fluxo de dados.  
  
## <a name="see-also"></a>Veja também

- [Configurando WCF Data Services](defining-wcf-data-services.md)
- [Introdução](getting-started-with-wcf-data-services.md)
