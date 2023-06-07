---
title: Configuração do cliente
description: Saiba como usar a configuração do cliente WCF para especificar o endereço, a associação, o comportamento e o contrato de um ponto de extremidade, que é usado para se conectar a um serviço.
ms.date: 03/30/2017
ms.assetid: 5da5bd3b-65d9-43b7-91b9-cc9e989b1350
ms.openlocfilehash: c3e3d4904bad39e951e8ba69013ac95894130489
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245369"
---
# <a name="client-configuration"></a>Configuração do cliente
Você pode usar a configuração de cliente do Windows Communication Foundation (WCF) para especificar o endereço, a associação, o comportamento e o contrato, as propriedades "ABC" do ponto de extremidade do cliente, que os clientes usam para se conectar a pontos de extremidade de serviço. O [\<client>](../../configure-apps/file-schema/wcf/client.md) elemento tem um [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-of-client.md) elemento cujos atributos são usados para configurar o ponto de extremidade ABCs. Esses atributos são discutidos na seção [Configurando pontos de extremidade](#configuring-endpoints) .  
  
 O [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-of-client.md) elemento também contém um [\<metadata>](../../configure-apps/file-schema/wcf/metadata.md) elemento que é usado para especificar as configurações de importação e exportação de metadados, um [\<headers>](../../configure-apps/file-schema/wcf/headers.md) elemento que contém uma coleção de cabeçalhos de endereço personalizados e um [\<identity>](../../configure-apps/file-schema/wcf/identity.md) elemento que permite a autenticação de um ponto de extremidade por outros pontos de extremidades que trocam mensagens com ele. Os [\<headers>](../../configure-apps/file-schema/wcf/headers.md) [\<identity>](../../configure-apps/file-schema/wcf/identity.md) elementos e são parte do <xref:System.ServiceModel.EndpointAddress> e são discutidos no artigo [endereços](endpoint-addresses.md) . Links para tópicos que explicam o uso de extensões de metadados são fornecidos na seção [Configurando metadados](#configuring-metadata) .  
  
## <a name="configuring-endpoints"></a>Configurando pontos de extremidade  
 A configuração do cliente foi criada para permitir que o cliente especifique um ou mais pontos de extremidade, cada um com seu próprio nome, endereço e contrato, com cada um referenciando os [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) elementos e na configuração do cliente a ser usada para configurar esse ponto de extremidade. O arquivo de configuração do cliente deve ser nomeado "App.config" porque esse é o nome esperado pelo tempo de execução do WCF. O exemplo a seguir mostra um arquivo de configuração de cliente.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
        <client>  
          <endpoint  
            name="endpoint1"  
            address="http://localhost/ServiceModelSamples/service.svc"  
            binding="wsHttpBinding"  
            bindingConfiguration="WSHttpBinding_IHello"  
            behaviorConfiguration="IHello_Behavior"  
            contract="IHello" >  
  
            <metadata>  
              <wsdlImporters>  
                <extension  
                  type="Microsoft.ServiceModel.Samples.WsdlDocumentationImporter, WsdlDocumentation"/>  
              </wsdlImporters>  
            </metadata>  
  
            <identity>  
              <servicePrincipalName value="host/localhost" />  
            </identity>  
          </endpoint>  
            <!-- Add another endpoint by adding another <endpoint> element. -->
          <endpoint  
            name="endpoint2">  
           //Configure another endpoint here.  
          </endpoint>  
        </client>  
  
//The bindings section references by the bindingConfiguration endpoint attribute.  
    <bindings>  
      <wsHttpBinding>  
        <binding name="WSHttpBinding_IHello"
                 bypassProxyOnLocal="false"
                 hostNameComparisonMode="StrongWildcard">  
          <readerQuotas maxDepth="32"/>  
          <reliableSession ordered="true"
                           enabled="false" />  
          <security mode="Message">  
           //Security settings go here.  
          </security>  
        </binding>  
        <binding name="Another Binding"  
          <!-- Configure this binding here. -->  
        </binding>  
          </wsHttpBinding>  
     </bindings>  
  
//The behavior section references by the behaviorConfiguration endpoint attribute.  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name=" IHello_Behavior ">  
                    <clientVia />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
  
    </system.serviceModel>  
</configuration>  
```  
  
 O `name` atributo opcional identifica exclusivamente um ponto de extremidade para um determinado contrato. Ele é usado pelo <xref:System.ServiceModel.ChannelFactory%601.%23ctor%2A> ou pelo <xref:System.ServiceModel.ClientBase%601.%23ctor%2A> para especificar qual ponto de extremidade na configuração do cliente está sendo direcionado e deve ser carregado quando um canal é criado para o serviço. Um nome de configuração de ponto de extremidade curinga " \* " está disponível e indica ao <xref:System.ServiceModel.ChannelFactory.ApplyConfiguration%2A> método que ele deve carregar qualquer configuração de ponto de extremidade no arquivo, desde que haja exatamente um disponível e, de outra forma, gere uma exceção. Se esse atributo for omitido, o ponto de extremidade correspondente será usado como o ponto de extremidade padrão associado ao tipo de contrato especificado. O valor padrão para o `name` atributo é uma cadeia de caracteres vazia que é igual a qualquer outro nome.  
  
 Cada ponto de extremidade deve ter um endereço associado a ele para localizar e identificar o ponto de extremidade. O `address` atributo pode ser usado para especificar a URL que fornece o local do ponto de extremidade. Mas o endereço para um ponto de extremidade de serviço também pode ser especificado no código criando um Uniform Resource Identifier (URI) e é adicionado ao <xref:System.ServiceModel.ServiceHost> usando um dos <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> métodos. Para obter mais informações, consulte [endereços](endpoint-addresses.md). Como a introdução indica, os [\<headers>](../../configure-apps/file-schema/wcf/headers.md) [\<identity>](../../configure-apps/file-schema/wcf/identity.md) elementos e são parte do <xref:System.ServiceModel.EndpointAddress> e também são discutidos no tópico [endereços](endpoint-addresses.md) .  
  
 O `binding` atributo indica o tipo de associação que o ponto de extremidade espera usar ao se conectar a um serviço. O tipo deve ter uma seção de configuração registrada se for para ser referenciado. No exemplo anterior, esta é a [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) seção, que indica que o ponto de extremidade usa um <xref:System.ServiceModel.WSHttpBinding> . Mas pode haver mais de uma associação de um determinado tipo que o ponto de extremidade pode usar. Cada um deles tem seu próprio [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento dentro do elemento do tipo (Associação). O `bindingconfiguration` atributo é usado para distinguir entre associações do mesmo tipo. Seu valor é correspondido com o `name` atributo do [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) elemento. Para obter mais informações sobre como configurar uma associação de cliente usando a configuração [do, consulte como especificar uma associação de cliente na configuração](../how-to-specify-a-client-binding-in-configuration.md).  
  
 O `behaviorConfiguration` atributo é usado para especificar qual o [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) [\<endpointBehaviors>](../../configure-apps/file-schema/wcf/endpointbehaviors.md) ponto de extremidade deve usar. Seu valor é correspondido com o `name` atributo do [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) elemento. Para obter um exemplo de como usar a configuração para especificar os comportamentos do cliente, consulte [Configurando comportamentos do cliente](../configuring-client-behaviors.md).  
  
 O `contract` atributo especifica qual contrato o ponto de extremidade está expondo. Esse valor é mapeado para o <xref:System.ServiceModel.ServiceContractAttribute.ConfigurationName%2A> do <xref:System.ServiceModel.ServiceContractAttribute> . O valor padrão é o nome completo do tipo da classe que implementa o serviço.  
  
### <a name="configuring-metadata"></a>Configurando metadados  
 O [\<metadata>](../../configure-apps/file-schema/wcf/metadata.md) elemento é usado para especificar as configurações usadas para registrar extensões de importação de metadados. Para obter mais informações sobre como estender o sistema de metadados, consulte [estendendo o sistema de metadados](../extending/extending-the-metadata-system.md).  
  
## <a name="see-also"></a>Veja também

- [Pontos de extremidade: endereços, associações e contratos](endpoints-addresses-bindings-and-contracts.md)
- [Configurando comportamentos do cliente](../configuring-client-behaviors.md)
