---
title: Como particionar dados de serviço
ms.date: 03/30/2017
ms.assetid: 1ccff72e-d76b-4e36-93a2-e51f7b32dc83
ms.openlocfilehash: 3b2f86ee6a4dea25fb5c972d4cecb1b9ed411b29
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601186"
---
# <a name="how-to-service-data-partitioning"></a>Como particionar dados de serviço
Este tópico descreve as etapas básicas necessárias para particionar mensagens em várias instâncias do mesmo serviço de destino. O particionamento de dados de serviço normalmente é usado quando você precisa dimensionar um serviço para fornecer melhor qualidade de serviço, ou quando você precisa lidar com solicitações de clientes diferentes de uma maneira específica. Por exemplo, as mensagens de clientes de alto valor ou "ouro" talvez precisem ser processadas em uma prioridade mais alta do que as mensagens de um cliente padrão.  
  
 Neste exemplo, as mensagens são roteadas para uma das duas instâncias do serviço regularCalc. Ambas as instâncias do serviço são idênticas; no entanto, o serviço representado pelo ponto de extremidade Calculator1 processa mensagens recebidas de clientes de alto valor, o ponto de extremidade da calculadora 2 processa mensagens de outros clientes  
  
 A mensagem enviada do cliente não tem dados exclusivos que possam ser usados para identificar a qual instância de serviço a mensagem deve ser roteada. Para permitir que cada cliente encaminhe dados para um serviço de destino específico, implementaremos dois pontos de extremidade de serviço que serão usados para receber mensagens.  
  
> [!NOTE]
> Embora este exemplo use pontos de extremidade específicos para particionar dados, isso também pode ser feito usando as informações contidas na própria mensagem, como cabeçalho ou dados de corpo.  
  
### <a name="implement-service-data-partitioning"></a>Implementar o particionamento de dados de serviço  
  
1. Crie a configuração básica do serviço de roteamento especificando os pontos de extremidade de serviço expostos pelo serviço. O exemplo a seguir define dois pontos de extremidade, que serão usados para receber mensagens. Ele também define os pontos de extremidade do cliente, que são usados para enviar mensagens para as instâncias do serviço regularCalc.  
  
    ```xml  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/router" />  
          </baseAddresses>  
        </host>  
        <!--Set up the inbound endpoints for the Routing Service-->  
        <!--create the endpoints for the calculator service-->  
        <endpoint address="calculator1"  
                  binding="wsHttpBinding"  
                  name="calculator1Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <endpoint address="calculator2"  
                  binding="wsHttpBinding"  
                  name="calculator2Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
       </service>  
    </services>  
    <client>  
    <!--set up the destination endpoints-->  
        <endpoint name="CalcEndpoint1"  
                  address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                  binding="netTcpBinding"  
                  contract="*" />  
  
        <endpoint name="CalcEndpoint2"  
                  address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                  binding="netTcpBinding"  
                  contract="*" />  
     </client>  
    ```  
  
2. Defina os filtros usados para rotear mensagens para os pontos de extremidade de destino.  Para este exemplo, o filtro EndpointName é usado para determinar qual ponto de extremidade de serviço recebeu a mensagem. O exemplo a seguir define a seção de roteamento e os filtros necessários.  
  
    ```xml  
    <filters>  
      <!--define the different message filters-->  
      <!--define endpoint name filters looking for messages that show up on the virtual endpoints-->  
      <filter name="HighPriority" filterType="EndpointName"  
              filterData="calculator1Endpoint"/>  
      <filter name="NormalPriority" filterType="EndpointName"  
              filterData="calculator2Endpoint"/>  
    </filters>  
    ```  
  
3. Defina a tabela de filtros, que associa cada filtro a um ponto de extremidade do cliente. Neste exemplo, a mensagem será roteada com base no ponto de extremidade específico em que foi recebido. Como a mensagem pode corresponder apenas a um dos dois filtros possíveis, não há necessidade de usar a prioridade de filtro para controlar a ordem em que os filtros são avaliados.  
  
     O seguinte define a tabela de filtros e adiciona os filtros definidos anteriormente.  
  
    ```xml  
    <filterTables>  
       <filterTable name="filterTable1">  
         <!--add the filters to the message filter table-->  
         <add filterName="HighPriority" endpointName="CalcEndpoint1"/>  
         <add filterName="NormalPriority" endpointName="CalcEndpoint2"/>  
       </filterTable>  
    </filterTables>  
    ```  
  
4. Para avaliar as mensagens de entrada em relação aos filtros contidos na tabela, você deve associar a tabela de filtros aos pontos de extremidade de serviço usando o comportamento de roteamento. O exemplo a seguir demonstra como associar "filterTable1" aos pontos de extremidade de serviço:  
  
    ```xml  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
## <a name="example"></a>Exemplo  
 A seguir está uma lista completa do arquivo de configuração.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright (c) Microsoft Corporation. All rights reserved -->  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/router" />  
          </baseAddresses>  
        </host>  
        <!--Set up the inbound endpoints for the Routing Service-->  
        <!--create the endpoints for the calculator service-->  
        <endpoint address="calculator1"  
                  binding="wsHttpBinding"  
                  name="calculator1Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <endpoint address="calculator2"  
                  binding="wsHttpBinding"  
                  name="calculator2Endpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
  
      </service>  
    </services>  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
    <client>  
<!--set up the destination endpoints-->  
      <endpoint name="CalcEndpoint1"  
                address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <endpoint name="CalcEndpoint2"  
                address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
    </client>  
    <routing>  
      <!-- use the namespace table element to define a prefix for our custom namespace-->  
  
      <filters>  
        <!--define the different message filters-->  
        <!--define endpoint name filters looking for messages that show up on the virtual endpoints-->  
        <filter name="HighPriority" filterType="EndpointName"  
                filterData="calculator1Endpoint"/>  
        <filter name="NormalPriority" filterType="EndpointName"  
                filterData="calculator2Endpoint"/>  
      </filters>  
  
      <filterTables>  
        <filterTable name="filterTable1">  
          <!--add the filters to the message filter table-->  
          <add filterName="HighPriority" endpointName="CalcEndpoint1"/>  
          <add filterName="NormalPriority" endpointName="CalcEndpoint2"/>  
        </filterTable>  
      </filterTables>  
  
    </routing>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- [Serviços de roteamento](../samples/routing-services.md)
