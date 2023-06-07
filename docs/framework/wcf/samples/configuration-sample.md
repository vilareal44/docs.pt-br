---
title: Exemplo de configuração
ms.date: 03/30/2017
ms.assetid: 75515b4a-8d70-44c8-99e0-7423df41380e
ms.openlocfilehash: 6d84085d06da117ebf13fa4bb714513aacc3abd6
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594719"
---
# <a name="configuration-sample"></a>Exemplo de configuração
Este exemplo demonstra o uso de um arquivo de configuração para tornar um serviço detectável.  
  
> [!NOTE]
> Este exemplo implementa a descoberta na configuração. Para obter um exemplo que implementa a descoberta no código, consulte [básico](basic-sample.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Discovery\Configuration`  
  
## <a name="service-configuration"></a>Configuração de Serviço  
 O arquivo de configuração neste exemplo demonstra dois recursos:  
  
- Tornando o serviço detectável por meio de um padrão <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> .  
  
- Ajuste das informações relacionadas à descoberta para o ponto de extremidade do aplicativo do serviço e ajuste algumas das configurações relacionadas à descoberta no ponto de extremidade padrão.  
  
 Para habilitar a descoberta, algumas alterações devem ser feitas no arquivo de configuração do aplicativo para o serviço:  
  
- Um ponto de extremidade de descoberta deve ser adicionado ao `<service>` elemento. Este é um <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> ponto de extremidade padrão. Esse é um ponto de extremidade do sistema que o tempo de execução associa ao serviço de descoberta. O serviço de descoberta escuta mensagens neste ponto de extremidade.  
  
- Um `<serviceDiscovery>` comportamento é adicionado à `<serviceBehaviors>` seção. Isso permite que o serviço seja descoberto em tempo de execução e use o ponto de extremidade de descoberta mencionado anteriormente para escutar descoberta `Probe` e `Resolve` mensagens. Com essas duas adições, o serviço é detectável no ponto de extremidade de descoberta especificado.  
  
 O trecho de configuração a seguir mostra um serviço com um ponto de extremidade de aplicativo e um ponto de extremidade de descoberta definidos:  
  
```xml
<services>  
        <service name="Microsoft.Samples.Discovery.CalculatorService"  
                 behaviorConfiguration="calculatorServiceBehavior">  
          <endpoint address=""  
                    binding="wsHttpBinding"  
                    contract="Microsoft.Samples.Discovery.ICalculatorService"  
                    behaviorConfiguration="endpointBehaviorConfiguration" />  
          <endpoint name="udpDiscovery"
                    kind="udpDiscoveryEndpoint"
                endpointConfiguration="adhocDiscoveryEndpointConfiguration"/>        </service>  
      </services>  
```  
  
 Para aproveitar os anúncios, você precisará adicionar um ponto de extremidade de anúncio. Para fazer isso, modifique o arquivo de configuração, conforme mostrado no código a seguir.  
  
```xml  
<serviceDiscovery>  
            <announcementEndpoints>  
              <endpoint kind="udpAnnouncementEndpoint"/>  
            </announcementEndpoints>  
          </serviceDiscovery>  
```  
  
 Adicionar um ponto de extremidade de anúncio ao comportamento do serviço de descoberta cria um cliente de anúncio padrão para o serviço. Isso garante que o serviço enviará um comunicado online e offline quando o serviço for aberto e fechado, respectivamente.  
  
 Esse arquivo de configuração vai além dessas etapas simples, modificando comportamentos adicionais. É possível controlar as informações relacionadas à descoberta usando pontos de extremidade específicos. Ou seja, um usuário pode controlar se um ponto de extremidade pode ser descoberto e o usuário também pode marcar esse ponto de extremidade com <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior.Scopes%2A> e os metadados XML personalizados. Para fazer isso, o usuário deve adicionar uma `behaviorConfiguration` propriedade ao ponto de extremidade do aplicativo. Nesse caso, a propriedade a seguir é adicionada ao ponto de extremidade do aplicativo.  
  
`behaviorConfiguration="endpointBehaviorConfiguration"`  
  
 Agora, por meio do elemento de configuração de comportamento, você pode controlar os atributos relacionados à descoberta. Nesse caso, dois escopos são adicionados ao ponto de extremidade do aplicativo.  
  
```xml  
<endpointBehaviors>  
          <behavior name="endpointBehaviorConfiguration">  
            <endpointDiscovery>  
              <scopes>  
                <add scope="http://www.example.com/calculator"/>  
                <add scope="ldap:///ou=engineering,o=examplecom,c=us"/>  
              </scopes>  
            </endpointDiscovery>  
  
          </behavior>
        </endpointBehaviors>  
```  
  
 Para obter mais informações sobre escopos, consulte [descoberta e FindCriteria](../feature-details/discovery-find-and-findcriteria.md).  
  
 Você também pode controlar detalhes específicos do ponto de extremidade de descoberta. Isso é feito por meio do <xref:System.ServiceModel.Configuration.StandardEndpointsSection> . Neste exemplo, a versão do protocolo usado é modificada, bem como a adição de um `maxResponseDelay` atributo, conforme mostrado no exemplo de código a seguir.  
  
```xml  
<standardEndpoints>  
   <udpDiscoveryEndpoint>  
      <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11" maxResponseDelay="00:00:00.600" />
   </udpDiscoveryEndpoint>  
</standardEndpoints>  
```  
  
 A seguir está o arquivo de configuração completo usado neste exemplo:  
  
```xml  
<configuration>  
    <system.serviceModel>  
  
      <services>  
        <service name="Microsoft.Samples.Discovery.CalculatorService"  
                 behaviorConfiguration="calculatorServiceBehavior">  
          <endpoint address=""  
                    binding="wsHttpBinding"  
                    contract="Microsoft.Samples.Discovery.ICalculatorService"  
                    behaviorConfiguration="endpointBehaviorConfiguration" />  
         <!-- Define the discovery endpoint -->
<endpoint name="udpDiscovery" kind="udpDiscoveryEndpoint" endpointConfiguration="adhocDiscoveryEndpointConfiguration"/>        </service>  
      </services>  
  
      <behaviors>  
  
        <serviceBehaviors>  
          <behavior name="calculatorServiceBehavior">  
  
            <!-- Add an announcement endpoint -->  
            <serviceDiscovery>  
              <announcementEndpoints>  
                <endpoint kind="udpAnnouncementEndpoint"/>  
              </announcementEndpoints>  
            </serviceDiscovery>  
          </behavior>  
        </serviceBehaviors>  
  
        <endpointBehaviors>  
          <behavior name="endpointBehaviorConfiguration">  
            <!-- Add scopes used to identify the service -->  
            <endpointDiscovery>  
              <scopes>  
                <add scope="http://www.example.com/calculator"/>  
                <add scope="ldap:///ou=engineering,o=examplecom,c=us"/>  
              </scopes>  
            </endpointDiscovery>  
  
          </behavior>
        </endpointBehaviors>  
  
      </behaviors>  
  
      <standardEndpoints>  
        <udpDiscoveryEndpoint>  
         <!-- Configure the UDP discovery endpoint -->  
          <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11" maxResponseDelay="00:00:00.600" />
        </udpDiscoveryEndpoint>  
      </standardEndpoints>  
  
    </system.serviceModel>  
</configuration>  
```  
  
## <a name="client-configuration"></a>Configuração do cliente  
 No arquivo de configuração do aplicativo para o cliente, um `standardEndpoint` do tipo `dynamicEndpoint` é usado para utilizar a descoberta, conforme mostrado no trecho de configuração a seguir.  
  
```xml  
<client>  
   <!--  Create an endpoint, make kind="dynamicEndpoint" and use the endpointConfiguration to change settings of DynamicEndpoint -->  
   <endpoint name="calculatorEndpoint"  
             binding="wsHttpBinding"  
             contract="ICalculatorService"  
             kind ="dynamicEndpoint"  
             endpointConfiguration="dynamicEndpointConfiguration">  
   </endpoint>  
</client>  
```  
  
 Quando um cliente está usando um `dynamicEndpoint` , o tempo de execução executa a descoberta automaticamente. Várias configurações são usadas durante a descoberta, como aquelas definidas na `discoveryClientSettings` seção, que especifica o tipo de ponto de extremidade de descoberta a ser usado:  
  
```xml  
<endpoint kind="udpDiscoveryEndpoint" endpointConfiguration="adhocDiscoveryEndpointConfiguration" />  
```  
  
 Os critérios de localização usados para Pesquisar serviços:  
  
```xml  
<!-- Add Scopes, ScopeMatchBy, Extensions and termination criteria in FindCriteria -->  
<findCriteria scopeMatchBy="http://schemas.microsoft.com/ws/2008/06/discovery/rfc" duration="00:00:10" maxResults="1">  
   <scopes>  
      <add scope="http://www.microsoft.com/building42/floor1"/>  
   </scopes>  
   <!-- These extensions are sent from the client to the service as part of the probe message -->  
   <extensions>  
      <CustomMetadata>This is custom metadata that is sent to the service along with the client's find request.</CustomMetadata>  
   </extensions>  
</findCriteria>  
```  
  
 Este exemplo estende esse recurso e modifica o <xref:System.ServiceModel.Discovery.FindCriteria> usado pelo cliente, bem como algumas propriedades do padrão `updDiscoveryEndpoint` usadas para descoberta. O <xref:System.ServiceModel.Discovery.FindCriteria> é modificado para usar um escopo e um `scopeMatchBy` algoritmo específico, bem como critérios de encerramento personalizados. Além disso, o exemplo também mostra como um cliente pode enviar elementos XML usando `Probe` mensagens. Por fim, algumas alterações são feitas no <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> , como a versão do protocolo usado e as configurações específicas de UDP, conforme mostrado no arquivo de configuração a seguir.  
  
```xml  
<udpDiscoveryEndpoint>
        <!-- Specify the discovery protocol version and UDP transport settings. -->
        <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11">  
          <transportSettings duplicateMessageHistoryLength="2048"  
                             maxPendingMessageCount="5"  
                             maxReceivedMessageSize="8192"  
                             maxBufferPoolSize="262144"/>  
        </standardEndpoint>
      </udpDiscoveryEndpoint>  
```  
  
 A seguir está a configuração completa do cliente usada no exemplo.  
  
```xml  
<configuration>  
  <system.serviceModel>  
  
    <client>  
      <!--  Create an endpoint, make kind="dynamicEndpoint" and use the endpointConfiguration to change settings of DynamicEndpoint -->  
      <endpoint name="calculatorEndpoint"  
                binding="wsHttpBinding"  
                contract="ICalculatorService"  
                kind ="dynamicEndpoint"  
                endpointConfiguration="dynamicEndpointConfiguration">  
      </endpoint>  
    </client>  
  
    <standardEndpoints>  
  
      <dynamicEndpoint>
        <standardEndpoint name="dynamicEndpointConfiguration">  
          <discoveryClientSettings>  
            <!-- Controls where the discovery happens. In this case, Probe message is sent over UdpDiscoveryEndpoint. -->  
            <endpoint kind="udpDiscoveryEndpoint" endpointConfiguration="adhocDiscoveryEndpointConfiguration" />  
  
            <!-- Add Scopes, ScopeMatchBy, Extensions and termination criteria in FindCriteria -->  
            <findCriteria scopeMatchBy="http://schemas.microsoft.com/ws/2008/06/discovery/rfc" duration="00:00:10" maxResults="1">  
              <scopes>  
                <add scope="http://www.microsoft.com/building42/floor1"/>  
              </scopes>  
              <!-- These extensions are sent from the client to the service as part of the probe message -->  
              <extensions>  
                <CustomMetadata>This is custom metadata that is sent to the service along with the client's find request.</CustomMetadata>  
              </extensions>  
            </findCriteria>  
          </discoveryClientSettings>  
        </standardEndpoint>
      </dynamicEndpoint>  
  
      <udpDiscoveryEndpoint>
        <!-- Specify the discovery protocol version and UDP transport settings. -->
        <standardEndpoint name="adhocDiscoveryEndpointConfiguration" discoveryVersion="WSDiscovery11">  
          <transportSettings duplicateMessageHistoryLength="2048"  
                             maxPendingMessageCount="5"  
                             maxReceivedMessageSize="8192"  
                             maxBufferPoolSize="262144"/>  
        </standardEndpoint>
      </udpDiscoveryEndpoint>  
  
    </standardEndpoints>  
  
  </system.serviceModel>
</configuration>
```  
  
#### <a name="to-use-this-sample"></a>Para usar este exemplo  
  
1. Este exemplo usa pontos de extremidade HTTP e para executar este exemplo, as ACLs de URL adequadas devem ser adicionadas. Para obter mais informações, consulte [Configurando http e HTTPS](../feature-details/configuring-http-and-https.md). A execução do comando a seguir em um privilégio elevado deve adicionar as ACLs apropriadas. Talvez você queira substituir seu domínio e nome de usuário pelos argumentos a seguir se o comando não funcionar como está. `netsh http add urlacl url=http://+:8000/ user=%DOMAIN%\%UserName%`  
  
2. Compile a solução.  
  
3. Execute o executável do serviço no diretório de compilação.  
  
4. Execute o executável do cliente. Observe que o cliente é capaz de localizar o serviço.  
