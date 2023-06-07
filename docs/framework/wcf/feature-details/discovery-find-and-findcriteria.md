---
title: FindCriteria e descoberta achada
ms.date: 03/30/2017
ms.assetid: 99016fa4-1778-495b-b4cc-0e22fbec42c6
ms.openlocfilehash: 1d6a0e3fcca45c3fe57aab84b0f2b6b86fabb404
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599172"
---
# <a name="discovery-find-and-findcriteria"></a>FindCriteria e descoberta achada

Uma operação de localização de descoberta é iniciada por um cliente para descobrir um ou mais serviços e é uma das principais ações na descoberta. A execução de uma localização envia uma mensagem de teste do WS-Discovery pela rede. Os serviços que correspondem aos critérios especificados respondem com mensagens ProbeMatch WS-Discovery. Para obter mais informações sobre mensagens de descoberta, consulte a [especificação do WS-Discovery](http://schemas.xmlsoap.org/ws/2004/10/discovery/ws-discovery.pdf).

## <a name="discoveryclient"></a>DiscoveryClient

A <xref:System.ServiceModel.Discovery.DiscoveryClient> classe fornece o mecanismo para executar operações de localização e facilita a execução de operações do cliente de descoberta. Ele contém um <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> método, que executa uma localização síncrona (de bloqueio) e um <xref:System.ServiceModel.Discovery.DiscoveryClient.FindAsync%2A> método, que inicia uma localização assíncrona sem bloqueio. Os dois métodos usam um <xref:System.ServiceModel.Discovery.FindCriteria> parâmetro e fornecem resultados ao usuário por meio de um <xref:System.ServiceModel.Discovery.FindResponse> objeto.

## <a name="findcriteria"></a>FindCriteria

<xref:System.ServiceModel.Discovery.FindCriteria>tem várias propriedades, que podem ser agrupadas em critérios de pesquisa, que especificam quais serviços você está procurando e localizam os critérios de encerramento (por quanto tempo a pesquisa deve durar). Um <xref:System.ServiceModel.Discovery.FindCriteria> pode conter vários critérios de pesquisa. Por padrão, o serviço precisa corresponder a todos os componentes, caso contrário, ele não considera um serviço correspondente. Se você quiser encontrar serviços que correspondam apenas a alguns dos critérios, você pode implementar a lógica de localização personalizada no serviço ou pode usar várias consultas.

Os critérios de pesquisa incluem:

- <xref:System.ServiceModel.Discovery.Configuration.ContractTypeNameElement>Adicional. O nome do contrato do serviço que está sendo pesquisado e os critérios normalmente usados ao pesquisar um serviço. Se mais de um nome de contrato for especificado, somente os pontos de extremidade de serviço correspondentes a todos os contratos serão respondidos. Observe que no WCF um ponto de extremidade só pode dar suporte a um contrato.

- <xref:System.ServiceModel.Discovery.Configuration.ScopeElement>Adicional. Os escopos são URIs absolutos que são usados para categorizar pontos de extremidade de serviço individuais. Talvez você queira usar isso em cenários em que vários pontos de extremidade expõem o mesmo contrato e você deseja uma maneira de procurar um subconjunto dos pontos de extremidade. Se mais de um escopo for especificado, somente os pontos de extremidade de serviço correspondentes a todos os escopos serão respondidos.

- <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchBy%2A>-Especifica o algoritmo de correspondência a ser usado ao corresponder os escopos na mensagem de investigação com o do ponto de extremidade. Há cinco regras de correspondência de escopo com suporte:

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByExact?displayProperty=nameWithType>faz uma comparação básica de cadeia de caracteres que diferencia maiúsculas de minúsculas.

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByPrefix?displayProperty=nameWithType>corresponde por segmentos separados por "/". Uma pesquisa por `http://contoso/building1` corresponde a um serviço com escopo `http://contoso/building/floor1` . Observe que ele não corresponde `http://contoso/building100` porque os dois últimos segmentos não correspondem.

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByLdap?displayProperty=nameWithType>faz a correspondência de escopos por segmentos usando uma URL LDAP.

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByUuid?displayProperty=nameWithType>faz a correspondência de escopos usando exatamente uma cadeia de caracteres UUID.

  - <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByNone?displayProperty=nameWithType>corresponde apenas aos serviços que não especificam um escopo.

  Se uma regra de correspondência de escopo não for especificada, <xref:System.ServiceModel.Discovery.FindCriteria.ScopeMatchByPrefix> será usada.

Os critérios de encerramento incluem:

1. <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A>-O tempo máximo de espera por respostas de serviços na rede. A duração padrão é de 20 segundos.

2. <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A>-O número máximo de respostas a aguardar. Se <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> as respostas forem recebidas antes <xref:System.ServiceModel.Discovery.FindCriteria.Duration%2A> de ter decorrido, a operação Localizar terminará.

## <a name="findresponse"></a>FindResponse

<xref:System.ServiceModel.Discovery.FindResponse>tem uma <xref:System.ServiceModel.Discovery.FindResponse.Endpoints%2A> propriedade de coleção que contém todas as respostas enviadas por serviços correspondentes na rede. Se nenhum serviço tiver respondido, a coleção estará vazia. Se um ou mais serviços tiverem respondido, cada resposta será armazenada em um <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> objeto, que contém o endereço, o contrato e algumas informações adicionais sobre o serviço.

O exemplo a seguir mostra como executar uma operação Localizar no código.

```csharp
// Create DiscoveryClient
DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());

// Create FindCriteria
FindCriteria findCriteria = new FindCriteria(typeof(IPrinterService));
findCriteria.Scopes.Add(new Uri("http://www.contoso.com/building1/floor1"));
findCriteria.Duration = TimeSpan.FromSeconds(10);

// Find ICalculatorService endpoints
FindResponse findResponse = discoveryClient.Find(findCriteria);

Console.WriteLine("Found {0} ICalculatorService endpoint(s).", findResponse.Endpoints.Count)
```

## <a name="see-also"></a>Consulte também

- [Visão geral de descoberta do WCF](wcf-discovery-overview.md)
- [Usando o canal cliente Discovery](using-the-discovery-client-channel.md)
- [Descoberta com escopos](../samples/discovery-with-scopes-sample.md)
- [Basic](../samples/basic-sample.md)
