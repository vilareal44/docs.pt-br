---
title: Configurando associações para serviços do Windows Communication Foundation
description: Saiba mais sobre como configurar associações no momento da implantação para um aplicativo WCF editando itens no sistema. Elemento ServiceModel.
ms.date: 03/30/2017
helpviewer_keywords:
- binding configuration [WCF]
ms.assetid: 99a85fd8-f7eb-4a84-a93e-7721b37d415c
ms.openlocfilehash: a2bb396e65722726e54cd315e931eea933386659
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247622"
---
# <a name="configuring-bindings-for-windows-communication-foundation-services"></a>Configurando associações para serviços do Windows Communication Foundation
Ao criar um aplicativo, muitas vezes você desejará adiar decisões para o administrador após a implantação do aplicativo. Por exemplo, geralmente não há como saber com antecedência o que é um endereço de serviço, ou Uniform Resource Identifier (URI), será. Em vez de embutir em código um endereço, é preferível permitir que um administrador faça isso depois de criar um serviço. Essa flexibilidade é realizada por meio da configuração.  
  
> [!NOTE]
> Use a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) com a `/config` opção para criar rapidamente os arquivos de configuração.  
  
## <a name="major-sections"></a>Seções principais  
 O esquema de configuração do Windows Communication Foundation (WCF) inclui as três seções principais a seguir ( `serviceModel` , `bindings` e `services` ):  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <bindings>  
        </bindings>  
        <services>  
        </services>  
        <behaviors>  
        </behaviors>  
    </system.serviceModel>  
</configuration>  
```  
  
### <a name="servicemodel-elements"></a>Elementos ServiceModel  
 Você pode usar a seção vinculada pelo `system.ServiceModel` elemento para configurar um tipo de serviço com um ou mais pontos de extremidade, bem como configurações para um serviço. Cada ponto de extremidade pode ser configurado com um endereço, um contrato e uma associação. Para obter mais informações sobre pontos de extremidade, consulte [visão geral da criação de ponto de extremidades](endpoint-creation-overview.md). Se nenhum ponto de extremidade for especificado, o tempo de execução adicionará pontos de extremidade padrão. Para obter mais informações sobre pontos de extremidade, associações e comportamentos padrão, confira [Configuração simplificada](simplified-configuration.md) e [Configuração simplificada para serviços WCF](./samples/simplified-configuration-for-wcf-services.md).  
  
 Uma associação especifica transportes (HTTP, TCP, Pipes, Enfileiramento de mensagens) e protocolos (segurança, confiabilidade, fluxos de transação) e consiste em elementos de associação, cada um deles especificando um aspecto de como um ponto de extremidade se comunica com o mundo.  
  
 Por exemplo, especificar o [\<basicHttpBinding>](../configure-apps/file-schema/wcf/basichttpbinding.md) elemento indica usar http como o transporte de um ponto de extremidade. Isso é usado para conectar o ponto de extremidade em tempo de execução quando o serviço que usa esse ponto de extremidade é aberto.  
  
 Há dois tipos de associações: predefinidos e personalizados. As associações predefinidas contêm combinações úteis de elementos que são usadas em cenários comuns. Para obter uma lista de tipos de associação predefinidos que o WCF fornece, consulte [associações fornecidas pelo sistema](system-provided-bindings.md). Se nenhuma coleção de associação predefinida tiver a combinação correta de recursos de que um aplicativo de serviço precisa, você poderá construir associações personalizadas para atender aos requisitos do aplicativo. Para obter mais informações sobre associações personalizadas, consulte [\<customBinding>](../configure-apps/file-schema/wcf/custombinding.md) .  
  
 Os quatro exemplos a seguir ilustram as configurações de associação mais comuns usadas para configurar um serviço WCF.  
  
#### <a name="specifying-an-endpoint-to-use-a-binding-type"></a>Especificando um ponto de extremidade para usar um tipo de associação  
 O primeiro exemplo ilustra como especificar um ponto de extremidade configurado com um endereço, um contrato e uma associação.  
  
```xml  
<service name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">  
  <!-- This section is optional with the default configuration introduced  
       in .NET Framework 4. -->  
  <endpoint
      address="/HelloWorld2/"  
      contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
      binding="basicHttpBinding" />
</service>  
```  
  
 Neste exemplo, o `name` atributo indica a qual tipo de serviço é a configuração. Quando você cria um serviço em seu código com o `HelloWorld` contrato, ele é inicializado com todos os pontos de extremidade definidos na configuração de exemplo. Se o assembly implementar apenas um contrato de serviço, o `name` atributo poderá ser omitido porque o serviço usa o único tipo disponível. O atributo usa uma cadeia de caracteres, que deve estar no formato`Namespace.Class, AssemblyName, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null`  
  
 O `address` atributo especifica o URI que outros pontos de extremidade usam para se comunicar com o serviço. O URI pode ser um caminho absoluto ou relativo. Se um endereço relativo for fornecido, espera-se que o host forneça um endereço base apropriado para o esquema de transporte usado na associação. Se um endereço não estiver configurado, presume-se que o endereço base seja o endereço para esse ponto de extremidade.  
  
 O `contract` atributo especifica o contrato que esse ponto de extremidade está expondo. O tipo de implementação do serviço deve implementar o tipo de contrato. Se uma implementação de serviço implementar um único tipo de contrato, essa propriedade poderá ser omitida.  
  
 O `binding` atributo seleciona uma associação personalizada ou predefinida a ser usada para esse ponto de extremidade específico. Um ponto de extremidade que não seleciona explicitamente uma associação usa a seleção de associação padrão, que é `BasicHttpBinding` .  
  
#### <a name="modifying-a-predefined-binding"></a>Modificando uma associação predefinida  
 No exemplo a seguir, uma associação predefinida é modificada. Em seguida, ele pode ser usado para configurar qualquer ponto de extremidade no serviço. A associação é modificada definindo o <xref:System.ServiceModel.Configuration.IBindingConfigurationElement.ReceiveTimeout%2A> valor como 1 segundo. Observe que a propriedade retorna um <xref:System.TimeSpan> objeto.  
  
 Essa associação alterada é encontrada na seção associações. Essa associação alterada agora pode ser usada ao criar qualquer ponto de extremidade, definindo o `binding` atributo no `endpoint` elemento.  
  
> [!NOTE]
> Se você fornecer um nome específico para a associação, o `bindingConfiguration` especificado no ponto de extremidade de serviço deverá corresponder a ele.  
  
```xml  
<service name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">  
  <endpoint
      address="/HelloWorld2/"  
      contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
      binding="basicHttpBinding" />
</service>  
<bindings>  
    <basicHttpBinding
        receiveTimeout="00:00:01"  
    />  
</bindings>  
```  
  
## <a name="configuring-a-behavior-to-apply-to-a-service"></a>Configurando um comportamento para aplicar a um serviço  
 No exemplo a seguir, um comportamento específico é configurado para o tipo de serviço. O `ServiceMetadataBehavior` elemento é usado para habilitar a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) para consultar o serviço e gerar documentos WSDL (Web Services Description Language) a partir dos metadados.  
  
> [!NOTE]
> Se você fornecer um nome específico para o comportamento, o `behaviorConfiguration` especificado na seção serviço ou ponto de extremidade deverá corresponder a ele.  
  
```xml  
<behaviors>  
    <behavior>  
        <ServiceMetadata httpGetEnabled="true" />
    </behavior>  
</behaviors>  
<services>  
    <service
       name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">
       <endpoint
          address="http://computer:8080/Hello"  
          contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
          binding="basicHttpBinding" />
    </service>  
</services>  
```  
  
 A configuração anterior permite que um cliente chame e obtenha os metadados do serviço digitado "HelloWorld".  
  
 `svcutil /config:Client.exe.config http://computer:8080/Hello?wsdl`  
  
## <a name="specifying-a-service-with-two-endpoints-using-different-binding-values"></a>Especificando um serviço com dois pontos de extremidade usando valores de associação diferentes  
 Neste último exemplo, dois pontos de extremidade são configurados para o `HelloWorld` tipo de serviço. Cada ponto de extremidade usa um `bindingConfiguration` atributo personalizado diferente do mesmo tipo de associação (cada um modifica o `basicHttpBinding` ).  
  
```xml  
<service name="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null">  
    <endpoint  
        address="http://computer:8080/Hello1"  
        contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
        binding="basicHttpBinding"  
        bindingConfiguration="shortTimeout" />
    <endpoint  
        address="http://computer:8080/Hello2"  
        contract="HelloWorld, IndigoConfig, Version=2.0.0.0, Culture=neutral, PublicKeyToken=null"  
        binding="basicHttpBinding"  
        bindingConfiguration="Secure" />
</service>  
<bindings>  
    <basicHttpBinding
        name="shortTimeout"  
        timeout="00:00:00:01"
     />  
     <basicHttpBinding
        name="Secure">  
        <Security mode="Transport" />  
     </basicHttpBinding>
</bindings>  
```  
  
 Você pode obter o mesmo comportamento usando a configuração padrão adicionando uma `protocolMapping` seção e configurando as associações, conforme demonstrado no exemplo a seguir.  
  
```xml  
<protocolMapping>  
    <add scheme="http" binding="basicHttpBinding" bindingConfiguration="shortTimeout" />  
    <add scheme="https" binding="basicHttpBinding" bindingConfiguration="Secure" />  
</protocolMapping>  
<bindings>  
    <basicHttpBinding
        name="shortTimeout"  
        timeout="00:00:00:01"
     />  
     <basicHttpBinding
        name="Secure" />  
        <Security mode="Transport" />  
</bindings>  
```  
  
## <a name="see-also"></a>Veja também

- [Configuração simplificada](simplified-configuration.md)
- [Associações fornecidas pelo sistema](system-provided-bindings.md)
- [Visão geral de criação de ponto de extremidade](endpoint-creation-overview.md)
- [Usando associações para configurar serviços e clientes](using-bindings-to-configure-services-and-clients.md)
