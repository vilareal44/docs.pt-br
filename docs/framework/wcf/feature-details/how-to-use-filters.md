---
title: Como usar filtros
ms.date: 03/30/2017
ms.assetid: f2c7255f-c376-460e-aa20-14071f1666e5
ms.openlocfilehash: 434171138e75a0f4c336cd80cc2beb574b10001e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598886"
---
# <a name="how-to-use-filters"></a>Como usar filtros
Este tópico descreve as etapas básicas necessárias para criar uma configuração de roteamento que usa vários filtros. Neste exemplo, as mensagens são roteadas para duas implementações de um serviço de calculadora, regularCalc e roundingCalc. Ambas as implementações dão suporte às mesmas operações; no entanto, um serviço arredonda todos os cálculos para o valor inteiro mais próximo antes de retornar. Um aplicativo cliente deve ser capaz de indicar se deve usar a versão de arredondamento do serviço; Se nenhuma preferência de serviço for expressa, a mensagem terá o balanceamento de carga entre os dois serviços. As operações expostas por ambos os serviços são:  
  
- Adicionar  
  
- Subtrair  
  
- Multiplicar  
  
- Dividir  
  
 Como os dois serviços implementam as mesmas operações, você não pode usar o filtro de ação, pois a ação especificada na mensagem não será exclusiva. Em vez disso, você deve fazer trabalho adicional para garantir que as mensagens sejam roteadas para os pontos de extremidade apropriados.  
  
### <a name="determine-unique-data"></a>Determinar dados exclusivos  
  
1. Como ambas as implementações de serviço manipulam as mesmas operações e são, essencialmente, idênticas além dos dados que elas retornam, os dados base contidos nas mensagens enviadas de aplicativos cliente não são exclusivos o suficiente para permitir que você determine como encaminhar a solicitação. Mas se o aplicativo cliente adicionar um valor de cabeçalho exclusivo à mensagem, você poderá usar esse valor para determinar como a mensagem deve ser roteada.  
  
     Para este exemplo, se o aplicativo cliente precisar que a mensagem seja processada pela calculadora de arredondamento, ele adicionará um cabeçalho personalizado usando o seguinte código:  
  
    ```csharp  
    messageHeadersElement.Add(MessageHeader.CreateHeader("RoundingCalculator",
                                   "http://my.custom.namespace/", "rounding"));  
    ```  
  
     Agora você pode usar o filtro XPath para inspecionar mensagens para esse cabeçalho e rotear mensagens que contêm o cabeçalho para o serviço roundCalc.  
  
2. Além disso, o serviço de roteamento expõe dois pontos de extremidade de serviço virtual que podem ser usados com os filtros EndpointName, EndpointAddress ou PrefixEndpointAddress para rotear exclusivamente as mensagens de entrada para uma implementação de calculadora específica com base no ponto de extremidade para o qual o aplicativo cliente envia a solicitação.  
  
### <a name="define-endpoints"></a>Definir pontos de extremidade  
  
1. Ao definir os pontos de extremidade usados pelo serviço de roteamento, primeiro você deve determinar a forma do canal usado por seus clientes e serviços. Nesse cenário, os serviços de destino usam um padrão de solicitação-resposta, portanto, o <xref:System.ServiceModel.Routing.IRequestReplyRouter> é usado. O exemplo a seguir define os pontos de extremidade de serviço expostos pelo serviço de roteamento.  
  
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
            <!--first create the general router endpoint-->  
            <endpoint address="general"  
                      binding="wsHttpBinding"  
                      name="routerEndpoint"  
                      contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
            <!--create a virtual endpoint for the regular calculator service-->  
            <endpoint address="regular/calculator"  
                      binding="wsHttpBinding"  
                      name="calculatorEndpoint"  
                      contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
            <!--now create a virtual endpoint for the rounding calculator-->  
            <endpoint address="rounding/calculator"  
                      binding="wsHttpBinding"  
                      name="roundingEndpoint"  
                      contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
  
          </service>  
    </services>  
    ```  
  
     Com essa configuração, o serviço de roteamento expõe três pontos de extremidade separados. Dependendo das opções de tempo de execução, o aplicativo cliente envia mensagens para um desses endereços. As mensagens que chegam em um dos pontos de extremidade de serviço "virtual" ("arredondamento/calculadora" ou "regular/calculadora") são encaminhadas para a implementação da calculadora correspondente. Se o aplicativo cliente não enviar a solicitação para um ponto de extremidade específico, a mensagem será endereçada ao ponto de extremidade geral. Independentemente do ponto de extremidade escolhido, o aplicativo cliente também pode optar por incluir o cabeçalho personalizado para indicar que a mensagem deve ser encaminhada à implementação da calculadora de arredondamento.  
  
2. O exemplo a seguir define os pontos de extremidade do cliente (destino) para os quais o serviço de roteamento roteia as mensagens.  
  
    ```xml  
    <client>  
          <endpoint name="regularCalcEndpoint"  
                    address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                    binding="netTcpBinding"  
                    contract="*" />  
  
          <endpoint name="roundingCalcEndpoint"  
                    address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                    binding="netTcpBinding"  
                    contract="*" />  
    </client>  
    ```  
  
     Esses pontos de extremidade são usados na tabela de filtro para indicar o ponto de final de destino para o qual a mensagem é enviada quando corresponde a um filtro específico.  
  
### <a name="define-filters"></a>Definir Filtros  
  
1. Para rotear mensagens com base no cabeçalho personalizado "RoundingCalculator" que o aplicativo cliente adiciona à mensagem, defina um filtro que usa uma consulta XPath para verificar a presença desse cabeçalho. Como esse cabeçalho é definido usando um namespace personalizado, adicione também uma entrada de namespace que define um prefixo de namespace personalizado de "Custom" que é usado na consulta XPath. O exemplo a seguir define a seção de roteamento, a tabela de namespace e o filtro XPath necessários.  
  
    ```xml  
    <routing>  
          <!-- use the namespace table element to define a prefix for our custom namespace-->  
          <namespaceTable>  
            <add prefix="custom" namespace="http://my.custom.namespace/"/>  
          </namespaceTable>  
          <filters>  
            <!--define the different message filters-->  
            <!--define an xpath message filter to look for the custom header coming from the client-->  
            <filter name="XPathFilter" filterType="XPath"
                    filterData="/s12:Envelope/s12:Header/custom:RoundingCalculator = 'rounding'"/>  
          </filters>  
    </routing>  
    ```  
  
     Esse **MessageFilter** procura um cabeçalho RoundingCalculator na mensagem que contém um valor de "arredondamento". Esse cabeçalho é definido pelo cliente para indicar que a mensagem deve ser roteada para o serviço roundingCalc.  
  
    > [!NOTE]
    > O prefixo do namespace S12 é definido por padrão na tabela de namespace e representa o namespace `http://www.w3.org/2003/05/soap-envelope` .
  
2. Você também deve definir filtros que procuram mensagens recebidas nos dois pontos de extremidade virtuais. O primeiro ponto de extremidade virtual é o ponto de extremidade "regular/Calculator". O cliente pode enviar solicitações para esse ponto de extremidade para indicar que a mensagem deve ser roteada para o serviço regularCalc. A configuração a seguir define um filtro que usa o <xref:System.ServiceModel.Dispatcher.EndpointNameMessageFilter> para determinar se a mensagem chegou por um ponto de extremidade com o nome especificado em FilterData.  
  
    ```xml  
    <!--define an endpoint name filter looking for messages that show up on the virtual regular calculator endpoint-->  
    <filter name="EndpointNameFilter" filterType="EndpointName" filterData="calculatorEndpoint"/>  
    ```  
  
     Se uma mensagem for recebida pelo ponto de extremidade de serviço chamado "calculatorEndpoint", esse filtro será avaliado como `true` .  
  
3. Em seguida, defina um filtro que procura mensagens enviadas ao endereço do roundingEndpoint. O cliente pode enviar solicitações para esse ponto de extremidade para indicar que a mensagem deve ser roteada para o serviço roundingCalc. A configuração a seguir define um filtro que usa o <xref:System.ServiceModel.Dispatcher.PrefixEndpointAddressMessageFilter> para determinar se a mensagem chegou no ponto de extremidade "arredondamento/calculadora".  
  
    ```xml  
    <!--define a filter looking for messages that show up with the address prefix.  The corresponds to the rounding calc virtual endpoint-->  
    <filter name="PrefixAddressFilter" filterType="PrefixEndpointAddress"  
            filterData="http://localhost/routingservice/router/rounding/"/>  
    ```  
  
     Se uma mensagem for recebida em um endereço que começa com `http://localhost/routingservice/router/rounding/` , esse filtro será avaliado como **true**. Como o endereço base usado por essa configuração é `http://localhost/routingservice/router` e o endereço especificado para o roundingEndpoint é "arredondamento/calculadora", o endereço completo usado para se comunicar com esse ponto de extremidade é `http://localhost/routingservice/router/rounding/calculator` , que corresponde a esse filtro.  
  
    > [!NOTE]
    > O filtro PrefixEndpointAddress não avalia o nome do host ao executar uma correspondência, pois um único host pode ser referenciado usando uma variedade de nomes de host que podem ser de todas as maneiras válidas de se referir ao host do aplicativo cliente. Por exemplo, todas as seguintes opções podem se referir ao mesmo host:  
    >
    > - localhost  
    > - 127.0.0.1  
    > - `www.contoso.com`  
    > - ContosoWeb01  
  
4. O filtro final deve dar suporte ao roteamento de mensagens que chegam ao ponto de extremidade geral sem o cabeçalho personalizado. Para esse cenário, as mensagens devem alternar entre os serviços regularCalc e roundingCalc. Para dar suporte ao roteamento "round robin" dessas mensagens, use um filtro personalizado que permita que uma instância de filtro corresponda a cada mensagem processada.  O seguinte define duas instâncias de um RoundRobinMessageFilter, que são agrupadas para indicar que elas devem ser alternadas entre si.  
  
    ```xml  
    <!-- Set up the custom message filters.  In this example,   
         we'll use the example round robin message filter,   
         which alternates between the references-->  
    <filter name="RoundRobinFilter1" filterType="Custom"  
                    customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly"  
                    filterData="group1"/>  
    <filter name="RoundRobinFilter2" filterType="Custom"  
                    customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly"  
                    filterData="group1"/>  
    ```  
  
     Durante o tempo de execução, esse tipo de filtro alterna entre todas as instâncias de filtro definidas deste tipo que são configuradas como o mesmo grupo em uma coleção. Isso faz com que as mensagens processadas por esse filtro personalizado sejam alternadas entre retornar `true` para `RoundRobinFilter1` e `RoundRobinFilter2` .  
  
### <a name="define-filter-tables"></a>Definir tabelas de filtro  
  
1. Para associar os filtros a pontos de extremidade específicos do cliente, você deve colocá-los em uma tabela de filtro. Esse cenário de exemplo também usa configurações de prioridade de filtro, que é uma configuração opcional que permite que você indique a ordem na qual os filtros são processados. Se nenhuma prioridade de filtro for especificada, todos os filtros serão avaliados simultaneamente.  
  
    > [!NOTE]
    > Embora a especificação de uma prioridade de filtro permita que você controle a ordem na qual os filtros são processados, isso pode afetar negativamente o desempenho do serviço de roteamento. Quando possível, construa a lógica de filtro para que o uso de prioridades de filtro não seja necessário.  
  
     O seguinte define a tabela de filtro e adiciona o "XPathFilter" definido anteriormente à tabela com uma prioridade de 2. Essa entrada também especifica que, se o `XPathFilter` corresponder à mensagem, a mensagem será roteada para o `roundingCalcEndpoint` .  
  
    ```xml  
    <routing>  
    ...      <filters>  
    ...      </filters>  
          <filterTables>  
            <table name="filterTable1">  
              <entries>  
                <!--add the filters to the message filter table-->  
                <!--first look for the custom header, and if we find it,  
                    send the message to the rounding calc endpoint-->  
                <add filterName="XPathFilter" endpointName="roundingCalcEndpoint" priority="2"/>  
              </entries>  
            </table>  
          </filterTables>  
    </routing>  
    ```  
  
     Ao especificar uma prioridade de filtro, os filtros de prioridade mais alta são avaliados primeiro. Se um ou mais filtros em um nível de prioridade específico corresponderem, nenhum filtro em níveis de prioridade mais baixos será avaliado. Para esse cenário, 2 é a prioridade mais alta especificada e esta é a única entrada de filtro nesse nível.  
  
2. As entradas de filtro foram definidas para verificar se uma mensagem é recebida em um ponto de extremidade específico inspecionando o nome do ponto de extremidade ou o prefixo do endereço. As entradas a seguir adicionam ambas as entradas de filtro à tabela de filtro e as associamos aos pontos de extremidade de destino para os quais a mensagem será roteada. Esses filtros são definidos com uma prioridade de 1 para indicar que eles só devem ser executados se o filtro XPath anterior não coincidir com a mensagem.  
  
    ```xml  
    <!--if the header wasn't there, send the message based on which virtual endpoint it arrived at-->  
    <!--we determine this through the endpoint name, or through the address prefix-->  
    <add filterName="EndpointNameFilter" endpointName="regularCalcEndpoint" priority="1"/>  
    <add filterName="PrefixAddressFilter" endpointName="roundingCalcEndpoint" priority="1"/>  
    ```  
  
     Como esses filtros têm uma prioridade de filtro igual a 1, eles só serão avaliados se o filtro no nível de prioridade 2 não corresponder à mensagem. Além disso, como os dois filtros têm o mesmo nível de prioridade, eles serão avaliados simultaneamente. Como ambos os filtros são mutuamente exclusivos, é possível apenas um ou o outro para corresponder a uma mensagem.  
  
3. Se uma mensagem não corresponder a nenhum dos filtros anteriores, a mensagem foi recebida por meio do ponto de extremidade de serviço genérico e não continha informações de cabeçalho que indicam onde encaminhá-lo. Essas mensagens devem ser tratadas pelo filtro personalizado, que equilibra a carga entre os dois serviços de calculadora. O exemplo a seguir demonstra como adicionar as entradas de filtro à tabela de filtro; cada filtro é associado a um dos dois pontos de extremidade de destino.  
  
    ```xml  
    <!--if none of the other filters have matched,   
        this message showed up on the default router endpoint,   
        with no custom header-->  
    <!--round robin these requests between the two services-->  
    <add filterName="RoundRobinFilter1" endpointName="regularCalcEndpoint" priority="0"/>  
    <add filterName="RoundRobinFilter2" endpointName="roundingCalcEndpoint" priority="0"/>  
    ```  
  
     Como essas entradas especificam uma prioridade de 0, elas só serão avaliadas se nenhum filtro de prioridade mais alta corresponder à mensagem. Além disso, como ambas têm a mesma prioridade, elas são avaliadas simultaneamente.  
  
     Conforme mencionado anteriormente, o filtro personalizado usado por essas definições de filtro avalia apenas uma ou outra como `true` para cada mensagem recebida. Como apenas dois filtros são definidos usando esse filtro, com a mesma configuração de grupo especificada, o efeito é que o serviço de roteamento alterna entre o envio para o regularCalcEndpoint e o RoundingCalcEndpoint.  
  
4. Para avaliar as mensagens em relação aos filtros, a tabela de filtros deve primeiro ser associada aos pontos de extremidade de serviço que serão usados para receber mensagens.  O exemplo a seguir demonstra como associar a tabela de roteamento com os pontos de extremidade de serviço usando o comportamento de roteamento:  
  
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
        <!--first create the general router endpoint-->  
        <endpoint address="general"  
                  binding="wsHttpBinding"  
                  name="routerEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <!--create a virtual endpoint for the regular calculator service-->  
        <endpoint address="regular/calculator"  
                  binding="wsHttpBinding"  
                  name="calculatorEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        <!--now create a virtual endpoint for the rounding calculator-->  
        <endpoint address="rounding/calculator"  
                  binding="wsHttpBinding"  
                  name="roundingEndpoint"  
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
      <endpoint name="regularCalcEndpoint"  
                address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <endpoint name="roundingCalcEndpoint"  
                address="net.tcp://localhost:8080/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
    </client>  
    <routing>  
      <!-- use the namespace table element to define a prefix for our custom namespace-->  
      <namespaceTable>  
        <add prefix="custom" namespace="http://my.custom.namespace/"/>  
      </namespaceTable>  
      <filters>  
        <!--define the different message filters-->  
        <!--define an xpath message filter to look for the custom header coming from the client-->  
        <filter name="XPathFilter" filterType="XPath" filterData="/s12:Envelope/s12:Header/custom:RoundingCalculator = 'rounding'"/>  
  
        <!--define an endpoint name filter looking for messages that show up on the virtual regular calculator endpoint-->  
        <filter name="EndpointNameFilter" filterType="EndpointName" filterData="calculatorEndpoint"/>  
  
        <!--define a filter looking for messages that show up with the address prefix.  The corresponds to the rounding calc virtual endpoint-->  
        <filter name="PrefixAddressFilter" filterType="PrefixEndpointAddress" filterData="http://localhost/routingservice/router/rounding/"/>  
  
        <!--Set up the custom message filters.  In this example, we'll use the example round robin message filter, which alternates between the references-->  
        <filter name="RoundRobinFilter1" filterType="Custom" customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly" filterData="group1"/>  
        <filter name="RoundRobinFilter2" filterType="Custom" customType="CustomFilterAssembly.RoundRobinMessageFilter, CustomFilterAssembly" filterData="group1"/>  
      </filters>  
      <filterTables>  
        <table name="filterTable1">  
          <entries>  
            <!--add the filters to the message filter table-->  
            <!--first look for the custom header, and if we find it, send the message to the rounding calc endpoint-->  
            <add filterName="XPathFilter" endpointName="roundingCalcEndpoint" priority="2"/>  
  
            <!--if the header wasn't there, send the message based on which virtual endpoint it arrived at-->  
            <!--we determine this through the endpoint name, or through the address prefix-->  
            <add filterName="EndpointNameFilter" endpointName="regularCalcEndpoint" priority="1"/>  
            <add filterName="PrefixAddressFilter" endpointName="roundingCalcEndpoint" priority="1"/>  
  
            <!--if none of the other filters have matched, this message showed up on the default router endpoint, with no custom header-->  
            <!--round robin these requests between the two services-->  
            <add filterName="RoundRobinFilter1" endpointName="regularCalcEndpoint" priority="0"/>  
            <add filterName="RoundRobinFilter2" endpointName="roundingCalcEndpoint" priority="0"/>  
          </entries>  
        </table>  
      </filterTables>  
    </routing>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Consulte também

- [Serviços de roteamento](../samples/routing-services.md)
