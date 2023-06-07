---
title: <system.serviceModel>
description: Saiba mais sobre os elementos de configuração do WCF ServiceModel, que permitem configurar aplicativos cliente e serviço WCF.
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#system.ServiceModel
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.ServiceModel
helpviewer_keywords:
- <system.serviceModel> element
- system.serviceModel element
ms.assetid: 78519531-ad7a-40d3-b3e7-42f1103d8854
ms.openlocfilehash: 567cbd2cc07ee82e795daa067b9034b2b8dc1974
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85243952"
---
# \<system.serviceModel>
Esta seção de configuração contém todos os elementos de configuração de ServiceModel Windows Communication Foundation (WCF).  

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;**\<system.serviceModel>**  
  
## <a name="syntax"></a>Syntax  
  
```xml  
<system.serviceModel>
  <behaviors>
  </behaviors>
  <bindings>
  </bindings>
  <client>
  </client>
  <comContracts>
  </comContracts>
  <commonBehaviors>
  </commonBehaviors>
  <diagnostics>
  </diagnostics>
  <extensions>
  </extensions>
  <protocolMapping>
  </protocolMapping>
  <routing>
  </routing>
  <serviceHostingEnvironment>
  </serviceHostingEnvironment>
  <services>
  </services>
  <standardEndpoints>
  </standardEndpoints>
  <tracking>
  </tracking>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behaviors>](behaviors.md)|Esta seção define duas coleções filho chamadas `endpointBehaviors` e `serviceBehaviors` .  Cada coleção define elementos de comportamento consumidos por pontos de extremidade e serviços, respectivamente. Cada elemento do comportamento é identificado por seu exclusivo `name` atributo.|  
|[\<bindings>](bindings.md)|Esta seção contém uma coleção de associações padrão e personalizadas. Cada entrada é identificada por seu exclusivo `name` . Os serviços usam associações vinculando-os usando o `name` .|  
|[\<client>](client.md)|Esta seção contém uma lista de pontos de extremidade que um cliente usa para se conectar a um serviço.|  
|[\<comContracts>](comcontracts.md)|Esta seção define contratos COM habilitados para WCF e interoperabilidade COM.|  
|[\<commonBehaviors>](commonbehaviors.md)|Esta seção só pode ser definida no arquivo de machine.config. Ele define duas coleções filho chamadas `endpointBehaviors` e `serviceBehaviors` .  Cada coleção define elementos de comportamento consumidos por todos os pontos de extremidade do WCF e serviços no computador, respectivamente.  Se um comportamento for definido em ambas `<commonBehaviors>` as `<behaviors>` seções e, o comportamento na \<behaviors> seção terá preferência.|  
|[\<diagnostics>](diagnostics.md)|Esta seção contém configurações para os recursos de diagnóstico do WCF. O usuário pode habilitar/desabilitar rastreamento, contadores de desempenho e o provedor WMI e pode adicionar filtros de mensagem personalizados.|  
|[\<extensions>](extensions-section.md)|Esta seção contém uma coleção de extensões, que permitem ao usuário criar associações definidas pelo usuário, comportamentos e outros aspectos das extensões.|  
|[\<protocolMapping>](protocolmapping.md)|Esta seção define um conjunto de mapeamento de protocolo padrão entre esquemas de protocolo de transporte (por exemplo, http, net. TCP, net. pipe, etc.) e associações do WCF.|  
|[\<routing>](routing.md)|Esta seção define um conjunto de filtros de roteamento, que determinam o tipo de Windows Communication Foundation (WCF) <xref:System.ServiceModel.Dispatcher.MessageFilter> a ser usado ao avaliar mensagens recebidas, bem como tabelas de roteamento que definem os pontos de extremidade de destino para envio de mensagens quando um filtro é correspondente.|  
|[\<serviceHostingEnvironment>](servicehostingenvironment.md)|Esta seção define o tipo que o ambiente de Hospedagem de serviço instancia para um determinado transporte. Se esta seção estiver vazia, o tipo padrão será usado.|  
|[\<services>](services.md)|A seção contém uma coleção de serviços. Para cada serviço definido no assembly, esse elemento contém um `service` elemento que especifica as configurações para o serviço.|  
|[\<standardEndpoints>](standardendpoints.md)|Esta seção define uma coleção de pontos de extremidade padrão, que são pontos de extremidade pré-configurados reutilizáveis. Um ponto de extremidade padrão terá um ou mais dos atributos de endereço, associação e contrato definidos como um valor fixo. Por exemplo, no ponto de extremidade de descoberta, o contrato é fixo. Você também pode usar pontos de extremidade padrão para estender o ponto final de serviço com novas propriedades semelhantes à definição de associações personalizadas.|
|[\<tracking>](tracking-of-wcf.md)|Esta seção define as configurações de controle para um serviço de fluxo de trabalho.|

### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|\<configuration>|O elemento raiz para todos os elementos de configuração em um arquivo de configuração do .NET.|  
  
## <a name="remarks"></a>Comentários  
 O WCF não adiciona elementos às seções de configuração de outros produtos.  
  
 Os serviços WCF são definidos na `services` seção do arquivo de configuração. Um assembly pode conter qualquer número de serviços. Cada serviço tem sua própria `service` seção de configuração. A seção e seu conteúdo definem o contrato de serviço, o comportamento e os pontos de extremidade do serviço específico.  
  
 Somente um atributo de serviço `name` é necessário.  Por padrão, o nome de um serviço descreve o tipo CLR subjacente usado para implementar um serviço; no entanto, você pode alterar a propriedade ConfigurationName em um <xref:System.ServiceModel.ServiceContractAttribute> para substituir o requisito de tipo CLR.  
  
 O atributo `behaviorConfiguration` é opcional. Ele identifica o comportamento do serviço usado por um serviço. O comportamento especificado por esse atributo deve ser vinculado a um comportamento de serviço definido no escopo do mesmo arquivo de configuração (ou seja, o mesmo arquivo ou um arquivo pai).  
  
 Cada serviço expõe um ou mais pontos de extremidade definidos em um `endpoint` elemento. Cada ponto de extremidade tem seu próprio endereço e associação. Todas as associações usadas no arquivo de configuração devem ser definidas no escopo do arquivo.  
  
 As associações são vinculadas a pontos de extremidade por meio da combinação dos atributos `name` e `bindingConfiguration` . O `binding` atributo define em qual seção a associação é definida. O `bindingConfiguration` atributo define qual associação configurada dentro da seção de associação é usada. Uma seção de associação pode definir várias associações configuradas.  
  
## <a name="example"></a>Exemplo  
 Este é um exemplo de um arquivo de configuração do WCF.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <system.serviceModel>
    <behaviors>
      <!-- List of Behaviors -->
    </behaviors>
    <client>
      <!-- List of Endpoints -->
    </client>
    <diagnostics wmiProviderEnabled="false"
                 performanceCountersEnabled="false"
                 tracingEnabled="false">
    </diagnostics>
    <serviceHostingEnvironment>
      <!-- List of entries -->
    </serviceHostingEnvironment>
    <comContracts>
      <!-- List of COM+ Contracts -->
    </comContracts>
    <services>
      <!-- List of Services -->
    </services>
    <bindings>
      <!-- List of Bindings -->
    </bindings>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceModelSectionGroup>
