---
title: Endereços do ponto de extremidade
ms.date: 03/30/2017
helpviewer_keywords:
- addresses [WCF]
- Windows Communication Foundation [WCF], addresses
- WCF [WCF], addresses
ms.assetid: 13f269e3-ebb1-433c-86cf-54fbd866a627
ms.openlocfilehash: 71358ed1c16c3c9b490a41f74dd6319af8eb2e7b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84593523"
---
# <a name="endpoint-addresses"></a>Endereços do ponto de extremidade
Cada ponto de extremidade tem um endereço associado a ele, que é usado para localizar e identificar o ponto de extremidade. Esse endereço consiste principalmente em um Uniform Resource Identifier (URI), que especifica o local do ponto de extremidade. O endereço do ponto de extremidade é representado no modelo de programação Windows Communication Foundation (WCF) pela <xref:System.ServiceModel.EndpointAddress> classe, que contém uma <xref:System.ServiceModel.EndpointAddress.Identity%2A> propriedade opcional que permite a autenticação do ponto de extremidade por outros pontos de extremidades que trocam mensagens com ele e um conjunto de propriedades opcionais <xref:System.ServiceModel.EndpointAddress.Headers%2A> , que definem quaisquer outros cabeçalhos SOAP necessários para alcançar o serviço. Os cabeçalhos opcionais fornecem informações de endereçamento adicionais e mais detalhadas para identificar ou interagir com o ponto de extremidade de serviço. O endereço de um ponto de extremidade é representado na transmissão como uma referência de ponto de extremidade do WS-Addressing (EPR).  
  
## <a name="uri-structure-of-an-address"></a>Estrutura de URI de um endereço  
 O URI de endereço para a maioria dos transportes tem quatro partes. Por exemplo, as quatro partes do URI `http://www.fabrikam.com:322/mathservice.svc/secureEndpoint` podem ser discriminadas da seguinte maneira:  
  
- Esquema`http:`
  
- Tradução`www.fabrikam.com`  
  
- adicional Porta: 322  
  
- Caminho:/mathservice.svc/secureEndpoint  
  
## <a name="defining-an-address-for-a-service"></a>Definindo um endereço para um serviço  
 O endereço do ponto de extremidade para um serviço pode ser especificado de forma imperativa usando código ou declarativamente por meio da configuração. A definição de pontos de extremidade no código geralmente não é prática porque as associações e os endereços para um serviço implantado são normalmente diferentes daqueles usados enquanto o serviço está sendo desenvolvido. Em geral, é mais prático definir pontos de extremidade de serviço usando a configuração em vez de código. Manter a ligação e as informações de endereçamento do código permite que elas sejam alteradas sem a necessidade de recompilar ou reimplantar o aplicativo.  
  
### <a name="defining-an-address-in-configuration"></a>Definindo um endereço na configuração  
 Para definir um ponto de extremidade em um arquivo de configuração, use o [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) elemento. Para obter detalhes e um exemplo, consulte [especificando um endereço de ponto de extremidade](../specifying-an-endpoint-address.md).  
  
### <a name="defining-an-address-in-code"></a>Definindo um endereço no código  
 Um endereço de ponto de extremidade pode ser criado no código com a <xref:System.ServiceModel.EndpointAddress> classe. Para obter detalhes e um exemplo, consulte [especificando um endereço de ponto de extremidade](../specifying-an-endpoint-address.md).  
  
### <a name="endpoints-in-wsdl"></a>Pontos de extremidade em WSDL  
 Um endereço de ponto de extremidade também pode ser representado em WSDL como um elemento EPR do WS-Addressing dentro do elemento do ponto de extremidade correspondente `wsdl:port` . O EPR contém o endereço do ponto de extremidade, bem como qualquer propriedade de endereço. Para obter detalhes e um exemplo, consulte [especificando um endereço de ponto de extremidade](../specifying-an-endpoint-address.md).  
  
## <a name="multiple-iis-binding-support-in-net-framework-35"></a>Suporte a várias associações do IIS no .NET Framework 3,5  
 Os provedores de serviços de Internet geralmente hospedam muitos aplicativos no mesmo servidor e site para aumentar a densidade do site e reduzir o custo total de propriedade. Esses aplicativos normalmente são associados a endereços de base diferentes. Um site da Serviços de Informações da Internet (IIS) pode conter vários aplicativos. Os aplicativos em um site podem ser acessados por meio de uma ou mais associações do IIS.  
  
 As associações do IIS fornecem duas partes de informação: um protocolo de associação e informações de associação. O protocolo de associação define o esquema sobre o qual ocorre a comunicação e informações de associação são as informações usadas para acessar o site.  
  
 O exemplo a seguir mostra os componentes que podem estar presentes em uma associação do IIS:  
  
- Protocolo de associação: HTTP  
  
- Informações de associação: endereço IP, porta, cabeçalho de host  
  
 O IIS pode especificar várias associações para cada site, o que resulta em vários endereços base para cada esquema. Antes do .NET Framework 3,5, o WCF não oferecia suporte a vários endereços para um esquema e, se eles foram especificados, lançou um <xref:System.ArgumentException> durante a ativação.  
  
 O .NET Framework 3,5 permite que os provedores de serviços de Internet hospedem vários aplicativos com endereços base diferentes para o mesmo esquema no mesmo site.  
  
 Por exemplo, um site pode conter os seguintes endereços base:  
  
- `http://payroll.myorg.com/Service.svc`
  
- `http://shipping.myorg.com/Service.svc`
  
 Com o .NET Framework 3,5, você especifica um filtro de prefixo no nível do AppDomain no arquivo de configuração. Você faz isso com o [\<baseAddressPrefixFilters>](../../configure-apps/file-schema/wcf/baseaddressprefixfilters.md) elemento, que contém uma lista de prefixos. Os endereços base de entrada, fornecidos pelo IIS, são filtrados com base na lista de prefixos opcionais. Por padrão, quando um prefixo não é especificado, todos os endereços são passados. A especificação do prefixo resulta apenas no endereço base correspondente para o qual o esquema será passado.  
  
 Veja a seguir um exemplo de código de configuração que usa os filtros de prefixo.  
  
```xml  
<system.serviceModel>  
  <serviceHostingEnvironment>  
     <baseAddressPrefixFilters>  
        <add prefix="net.tcp://payroll.myorg.com:8000"/>  
        <add prefix="http://shipping.myorg.com:8000"/>  
    </baseAddressPrefixFilters>  
  </serviceHostingEnvironment>  
</system.serviceModel>  
```  
  
 No exemplo anterior, `net.tcp://payroll.myorg.com:8000` e `http://shipping.myorg.com:8000` são os únicos endereços base, para seus respectivos esquemas, que são passados.  
  
 O não `baseAddressPrefixFilter` oferece suporte a curingas.  
  
 Os endereços base fornecidos pelo IIS podem ter endereços associados a outros esquemas que não estão presentes na `baseAddressPrefixFilters` lista. Esses endereços não são filtrados.  
  
## <a name="multiple-iis-binding-support-in-net-framework-4-and-later"></a>Suporte a várias associações IIS no .NET Framework 4 e posterior  
 A partir do .NET 4, você pode habilitar o suporte para várias associações no IIS sem a necessidade de escolher um único endereço base, <xref:System.ServiceModel.ServiceHostingEnvironment> definindo <xref:System.ServiceModel.ServiceHostingEnvironment.MultipleSiteBindingsEnabled%2A> a configuração como true. Esse suporte é limitado aos esquemas de protocolo HTTP.  
  
 Veja a seguir um exemplo de código de configuração que usa multipleSiteBindingsEnabled em [\<serviceHostingEnvironment>](../../configure-apps/file-schema/wcf/servicehostingenvironment.md) .  
  
```xml  
<system.serviceModel>  
  <serviceHostingEnvironment multipleSiteBindingsEnabled="true" >  
  </serviceHostingEnvironment>  
</system.serviceModel>  
```  
  
 Todas as configurações de baseAddressPrefixFilters são ignoradas, para protocolos HTTP e não HTTP, quando várias associações de site são habilitadas usando essa configuração.  
  
 Para obter detalhes e exemplos, consulte [suporte a várias associações de site do IIS](supporting-multiple-iis-site-bindings.md) e <xref:System.ServiceModel.ServiceHostingEnvironment.MultipleSiteBindingsEnabled%2A> .  
  
## <a name="extending-addressing-in-wcf-services"></a>Estendendo o endereçamento nos serviços WCF  
 O modelo de endereçamento padrão dos serviços WCF usa o URI do endereço do ponto de extremidade para as seguintes finalidades:  
  
- Para especificar o endereço de escuta do serviço, o local no qual o ponto de extremidade escuta mensagens,  
  
- Para especificar o filtro de endereço SOAP, o endereço que um ponto de extremidade espera como um cabeçalho SOAP.  
  
 Os valores para cada uma dessas finalidades podem ser especificados separadamente, permitindo várias extensões de endereçamento que abrangem cenários úteis:  
  
- Intermediários SOAP: uma mensagem enviada por um cliente atravessa um ou mais serviços adicionais que processam a mensagem antes de atingir seu destino final. Os intermediários SOAP podem executar várias tarefas, como cache, roteamento, balanceamento de carga ou validação de esquema nas mensagens. Esse cenário é realizado enviando mensagens para um endereço físico separado ( `via` ) que tem como alvo o intermediário, em vez de apenas um endereço lógico ( `wsa:To` ) que se destina ao destino final.  
  
- O endereço de escuta do ponto de extremidade é um URI privado e é definido com um valor diferente de sua `listenURI` propriedade.  
  
 O endereço de transporte que o `via` especifica é o local para o qual uma mensagem deve ser enviada inicialmente ao seu caminho para algum outro endereço remoto especificado pelo `to` parâmetro no qual o serviço está localizado. Na maioria dos cenários de Internet, o `via` URI é o mesmo que a <xref:System.ServiceModel.EndpointAddress.Uri%2A> Propriedade do `to` endereço final do serviço. Você só distingue entre esses dois endereços quando deve fazer o roteamento manual.  
  
### <a name="addressing-headers"></a>Cabeçalhos de endereçamento  
 Um ponto de extremidade pode ser resolvido por um ou mais cabeçalhos SOAP além de seu URI básico. Um conjunto de cenários em que isso é útil é um conjunto de cenários intermediários de SOAP em que um ponto de extremidade exige que clientes desse ponto de extremidade incluam cabeçalhos SOAP direcionados a intermediários.  
  
 Você pode definir cabeçalhos de endereço personalizados de duas maneiras — usando o código ou a configuração:  
  
- No código, crie cabeçalhos de endereço personalizados usando a <xref:System.ServiceModel.Channels.AddressHeader> classe e, em seguida, usado na construção de um <xref:System.ServiceModel.EndpointAddress> .  
  
- Em configuração, personalizado [\<headers>](../../configure-apps/file-schema/wcf/headers.md) são especificados como filhos do [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-of-client.md) elemento.  
  
 A configuração é geralmente preferível ao código, pois permite que você altere os cabeçalhos após a implantação.  
  
### <a name="custom-listening-addresses"></a>Endereços de escuta personalizados  
 Você pode definir o endereço de escuta para um valor diferente do URI do ponto de extremidade. Isso é útil em cenários intermediários em que o endereço SOAP a ser exposto é o de um intermediário de SOAP público, enquanto o endereço onde o ponto de extremidade realmente escuta é um endereço de rede privada.  
  
 Você pode especificar um endereço de escuta personalizado usando o código ou a configuração:  
  
- No código, especifique um endereço de escuta personalizado adicionando uma <xref:System.ServiceModel.Description.ClientViaBehavior> classe à coleção de comportamentos do ponto de extremidade.  
  
- Em configuração, especifique um endereço de escuta personalizado com o `ListenUri` atributo do elemento de serviço [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) .  
  
### <a name="custom-soap-address-filter"></a>Filtro de endereço SOAP personalizado  
 O <xref:System.ServiceModel.EndpointAddress.Uri%2A> é usado em conjunto com qualquer <xref:System.ServiceModel.EndpointAddress.Headers%2A> propriedade para definir o filtro de endereço SOAP de um ponto de extremidade ( <xref:System.ServiceModel.Dispatcher.EndpointDispatcher.AddressFilter%2A> ). Por padrão, esse filtro verifica se uma mensagem de entrada tem um `To` cabeçalho de mensagem que corresponde ao URI do ponto de extremidade e que todos os cabeçalhos de ponto de extremidade necessários estão presentes na mensagem.  
  
 Em alguns cenários, um ponto de extremidade recebe todas as mensagens que chegam no transporte subjacente, e não apenas aquelas com o `To` cabeçalho apropriado. Para habilitar isso, o usuário pode usar a <xref:System.ServiceModel.Dispatcher.MatchAllMessageFilter> classe.  
  
## <a name="see-also"></a>Consulte também

- [Especificando um endereço do ponto de extremidade](../specifying-an-endpoint-address.md)
- [Identidade e autenticação de serviço](service-identity-and-authentication.md)
