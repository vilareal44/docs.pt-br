---
title: Modelo de objeto de programação HTTP Web do WCF
ms.date: 03/30/2017
ms.assetid: ed96b5fc-ca2c-4b0d-bdba-d06b77c3cb2a
ms.openlocfilehash: ba3bca8950037a185b76deae4713170db3f4a75d
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600147"
---
# <a name="wcf-web-http-programming-object-model"></a>Modelo de objeto de programação HTTP Web do WCF
O modelo de programação WEB HTTP do WCF permite que os desenvolvedores exponham serviços da Web Windows Communication Foundation (WCF) por meio de solicitações HTTP básicas sem exigir SOAP. O modelo de programação WEB HTTP do WCF é criado sobre o modelo de extensibilidade do WCF existente. Ele define as seguintes classes:  
  
 **Modelo de programação:**  
  
- <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute>  
  
- <xref:System.ServiceModel.Web.WebGetAttribute>  
  
- <xref:System.ServiceModel.Web.WebInvokeAttribute>  
  
- <xref:System.ServiceModel.Web.WebServiceHost>  
  
 **Infraestrutura de canais e Dispatcher:**  
  
- <xref:System.ServiceModel.WebHttpBinding>  
  
- <xref:System.ServiceModel.Description.WebHttpBehavior>  
  
 **Classes utilitárias e pontos de extensibilidade:**  
  
- <xref:System.UriTemplate>  
  
- <xref:System.UriTemplateTable>  
  
- <xref:System.ServiceModel.Dispatcher.QueryStringConverter>  
  
- <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector>  
  
## <a name="aspnetcacheprofileattribute"></a>AspNetCacheProfileAttribute  
 O <xref:System.ServiceModel.Web.AspNetCacheProfileAttribute> , quando aplicado a uma operação de serviço, indica o perfil de cache de saída ASP.net no arquivo de configuração que deve ser usado pelo para armazenar em cache as respostas da operação no cache de saída do ASP .net. Essa propriedade usa apenas um parâmetro, o nome do perfil de cache que especifica as configurações de cache no arquivo de configuração.  
  
## <a name="webgetattribute"></a>WebGetAttribute  
 O <xref:System.ServiceModel.Web.WebGetAttribute> atributo é usado para marcar uma operação de serviço como uma que responde a solicitações HTTP Get. É um comportamento de operação passiva (os <xref:System.ServiceModel.Description.IOperationBehavior> métodos não fazem nada) que adiciona metadados à descrição da operação. Aplicar o <xref:System.ServiceModel.Web.WebGetAttribute> não tem nenhum efeito, a menos que um comportamento que procure esses metadados na descrição da operação (especificamente, o <xref:System.ServiceModel.Description.WebHttpBehavior> ) seja adicionado à coleção de comportamento do serviço. O <xref:System.ServiceModel.Web.WebGetAttribute> atributo usa os parâmetros opcionais mostrados na tabela a seguir.  
  
|Parâmetro|Descrição|  
|---------------|-----------------|  
|`BodyStyle`|Controla se as solicitações são encapsuledas e as respostas enviadas e recebidas da operação de serviço à qual o atributo é aplicado.|  
|`RequestFormat`|Controla como as mensagens de solicitação são formatadas.|  
|`ResponseFormat`|Controla como as mensagens de resposta são formatadas.|  
|`UriTemplate`|Especifica o modelo de URI que controla quais solicitações HTTP são mapeadas para a operação de serviço à qual o atributo é aplicado.|  
  
## <a name="webhttpbinding"></a>WebHttpBinding  
 A <xref:System.ServiceModel.WebHttpBinding> classe incorpora suporte para XML, JSON e dados binários brutos usando o <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement> . Ele é composto de um <xref:System.ServiceModel.Channels.HttpsTransportBindingElement> <xref:System.ServiceModel.Channels.HttpTransportBindingElement> e de um <xref:System.ServiceModel.WebHttpSecurity> objeto. O <xref:System.ServiceModel.WebHttpBinding> é projetado para ser usado em conjunto com o <xref:System.ServiceModel.Description.WebHttpBehavior> .  
  
## <a name="webinvokeattribute"></a>Invoke  
 O <xref:System.ServiceModel.Web.WebInvokeAttribute> atributo é semelhante a <xref:System.ServiceModel.Web.WebGetAttribute> , mas é usado para marcar uma operação de serviço como uma que responde a solicitações HTTP que não sejam Get. É um comportamento de operação passiva (os <xref:System.ServiceModel.Description.IOperationBehavior> métodos não fazem nada) que adiciona metadados à descrição da operação. Aplicar o <xref:System.ServiceModel.Web.WebInvokeAttribute> não tem nenhum efeito, a menos que um comportamento que procure esses metadados na descrição da operação (especificamente, o <xref:System.ServiceModel.Description.WebHttpBehavior> ) seja adicionado à coleção de comportamento do serviço.  
  
 O <xref:System.ServiceModel.Web.WebInvokeAttribute> atributo usa os parâmetros opcionais mostrados na tabela a seguir.  
  
|Parâmetro|Descrição|  
|---------------|-----------------|  
|`BodyStyle`|Controla se as solicitações são encapsuledas e as respostas enviadas e recebidas da operação de serviço à qual o atributo é aplicado.|  
|`Method`|Especifica o método HTTP ao qual a operação de serviço está mapeada.|  
|`RequestFormat`|Controla como as mensagens de solicitação são formatadas.|  
|`ResponseFormat`|Controla como as mensagens de resposta são formatadas.|  
|`UriTemplate`|Especifica o modelo de URI que controla o que as solicitações GET são mapeadas para a operação de serviço à qual o atributo é aplicado.|  
  
## <a name="uritemplate"></a>UriTemplate  
 A <xref:System.UriTemplate> classe permite que você defina um conjunto de URIs estruturalmente semelhantes. Os modelos são compostos de duas partes, um caminho e uma consulta. Um caminho consiste em uma série de segmentos delimitados por uma barra (/). Cada segmento pode ter um valor literal, um valor de variável (gravado entre chaves [{}], restrito para corresponder ao conteúdo de exatamente um segmento) ou um curinga (gravado como um asterisco [ \* ], que corresponde a "o restante do caminho"), que deve aparecer no final do caminho. A expressão de consulta pode ser omitida inteiramente. Se estiver presente, ele especificará uma série não ordenada de pares de nome/valor. Os elementos da expressão de consulta podem ser pares literais (? x = 2) ou pares de variáveis (? x = {*Value*}). Valores não emparelhados não são permitidos. <xref:System.UriTemplate>é usado internamente pelo modelo de programação do WCF WEB HTTP para mapear URIs ou grupos de URIs específicos para operações de serviço.  
  
## <a name="uritemplatetable"></a>UriTemplateTable  
 A <xref:System.UriTemplateTable> classe representa um conjunto associativo de <xref:System.UriTemplate> objetos vinculados a um objeto da escolha do desenvolvedor. Ele permite que você corresponda a URIs (identificadores de recursos uniformes) candidatos em relação aos modelos no conjunto e recupere os dados associados aos modelos correspondentes. <xref:System.UriTemplateTable>é usado internamente pelo modelo de programação do WCF WEB HTTP para mapear URIs ou grupos de URIs específicos para operações de serviço.  
  
## <a name="webservicehost"></a>WebServiceHost  
 <xref:System.ServiceModel.Web.WebServiceHost>estende o <xref:System.ServiceModel.ServiceHost> para facilitar o host de um serviço de estilo da Web não SOAP. Se <xref:System.ServiceModel.Web.WebServiceHost> o não encontrar nenhum ponto de extremidade na descrição do serviço, ele criará automaticamente um ponto de extremidades padrão no endereço base do serviço. Ao criar um ponto de extremidade HTTP padrão, o <xref:System.ServiceModel.Web.WebServiceHost> também desabilita a página de ajuda http e a funcionalidade WSDL (linguagem de descrição de serviços Web) Get para que o ponto de extremidade de metadados não interfira no ponto de extremidade http padrão. <xref:System.ServiceModel.Web.WebServiceHost>também garante que todos os pontos de extremidade que usam <xref:System.ServiceModel.WebHttpBinding> têm o <xref:System.ServiceModel.Description.WebHttpBehavior> anexo necessário anexado. Por fim, <xref:System.ServiceModel.Web.WebServiceHost> o configura automaticamente a associação do ponto de extremidade para trabalhar com as configurações de segurança serviços de informações da Internet (IIS) associadas quando usadas em um diretório virtual seguro.  
  
## <a name="webservicehostfactory"></a>WebServiceHostFactory  
 A <xref:System.ServiceModel.Activation.WebServiceHostFactory> classe é usada para criar dinamicamente um <xref:System.ServiceModel.Web.WebServiceHost> quando um serviço é hospedado em serviços de informações da Internet (IIS) ou no was (serviço de ativação de processos do Windows). Ao contrário de um serviço hospedado internamente em que o aplicativo de hospedagem instancia o <xref:System.ServiceModel.Web.WebServiceHost> , os serviços hospedados no IIS ou o was usam essa classe para criar o <xref:System.ServiceModel.Web.WebServiceHost> para o serviço. O <xref:System.ServiceModel.Activation.WebServiceHostFactory.CreateServiceHost%28System.Type%2CSystem.Uri%5B%5D%29> método é chamado quando uma solicitação de entrada para o serviço é recebida.  
  
## <a name="webhttpbehavior"></a>WebHttpBehavior  
 A <xref:System.ServiceModel.Description.WebHttpBehavior> classe fornece os formatadores necessários, seletores de operação e assim por diante, necessários para o suporte ao serviço de estilo da Web na camada de modelo de serviço. Isso é implementado como um comportamento de ponto de extremidade (usado em conjunto com o <xref:System.ServiceModel.WebHttpBinding> ) e permite que os formatadores e seletores de operação sejam especificados para cada ponto de extremidade, o que permite que a mesma implementação de serviço exponha os pontos de extremidade SOAP e POX.  
  
### <a name="extending-webhttpbehavior"></a>Estendendo o WebHttpBehavior  
 <xref:System.ServiceModel.Description.WebHttpBehavior>é extensível usando um número de métodos virtuais: <xref:System.ServiceModel.Description.WebHttpBehavior.GetOperationSelector%28System.ServiceModel.Description.ServiceEndpoint%29> ,,, <xref:System.ServiceModel.Description.WebHttpBehavior.GetReplyClientFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29> <xref:System.ServiceModel.Description.WebHttpBehavior.GetRequestClientFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29> <xref:System.ServiceModel.Description.WebHttpBehavior.GetReplyDispatchFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29> e <xref:System.ServiceModel.Description.WebHttpBehavior.GetRequestDispatchFormatter%28System.ServiceModel.Description.OperationDescription%2CSystem.ServiceModel.Description.ServiceEndpoint%29> . Os desenvolvedores podem derivar uma classe de <xref:System.ServiceModel.Description.WebHttpBehavior> e substituir esses métodos para personalizar o comportamento padrão.  
  
 O <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> é um exemplo de extensão <xref:System.ServiceModel.Description.WebHttpBehavior> . <xref:System.ServiceModel.Description.WebScriptEnablingBehavior>permite que os pontos de extremidade de Windows Communication Foundation (WCF) recebam solicitações HTTP de um cliente ASP.NET AJAX baseado em navegador. O [serviço AJAX usando http post](../samples/ajax-service-using-http-post.md) é um exemplo de uso desse ponto de extensibilidade.  
  
> [!WARNING]
> Ao usar o <xref:System.ServiceModel.Description.WebScriptEnablingBehavior> , o <xref:System.UriTemplate> não tem suporte <xref:System.ServiceModel.Web.WebGetAttribute> nos <xref:System.ServiceModel.Web.WebInvokeAttribute> atributos ou.  
  
## <a name="webhttpdispatchoperationselector"></a>WebHttpDispatchOperationSelector  
 A <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector> classe usa <xref:System.UriTemplate> e <xref:System.UriTemplateTable> classes para distribuir chamadas para operações de serviço.  
  
## <a name="compatibility"></a>Compatibilidade  
 O modelo de programação do WCF WEB HTTP não usa mensagens baseadas em SOAP e, portanto, não oferece suporte aos protocolos WS-*. No entanto, você pode expor o mesmo contrato por dois pontos de extremidade diferentes: um usando SOAP e o outro não usando SOAP. Consulte [como: expor um contrato ao SOAP e a clientes Web](how-to-expose-a-contract-to-soap-and-web-clients.md) para obter um exemplo.  
  
## <a name="security"></a>Segurança  

Como o modelo de programação do WCF WEB HTTP não dá suporte aos protocolos WS-*, a única maneira de proteger um serviço Web criado no modelo de programação do WCF WEB HTTP é expor seu serviço usando SSL. Para obter mais informações sobre como configurar o SSL com o IIS 7,0, consulte [como implementar o SSL no IIS](https://support.microsoft.com/help/299875/how-to-implement-ssl-in-iis).
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.WebHttpBinding>
- <xref:System.ServiceModel.Web.WebGetAttribute>
- <xref:System.ServiceModel.Web.WebInvokeAttribute>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- <xref:System.ServiceModel.Dispatcher.WebHttpDispatchOperationSelector>
- [Visão geral do modelo de programação HTTP Web do WCF](wcf-web-http-programming-model-overview.md)
