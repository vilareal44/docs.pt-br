---
title: Configurando serviços usando arquivos de configuração
description: Saiba como um arquivo de configuração para um serviço WCF oferece a flexibilidade de fornecer dados de comportamento de ponto de extremidade e serviço durante a implantação.
ms.date: 03/30/2017
helpviewer_keywords:
- configuring services [WCF]
ms.assetid: c9c8cd32-2c9d-4541-ad0d-16dff6bd2a00
ms.openlocfilehash: 1a3266ad8890436c9be9d0f2b231aeaca0f9236e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245421"
---
# <a name="configuring-services-using-configuration-files"></a>Configurando serviços usando arquivos de configuração
Configurar um serviço de Windows Communication Foundation (WCF) com um arquivo de configuração oferece a flexibilidade de fornecer dados de comportamento de ponto de extremidade e serviço no ponto de implantação, em vez de em tempo de design. Este tópico descreve as principais técnicas disponíveis.  
  
 Um serviço WCF é configurável usando a tecnologia de configuração .NET Framework. Normalmente, os elementos XML são adicionados ao arquivo de Web.config para um site Serviços de Informações da Internet (IIS) que hospeda um serviço WCF. Os elementos permitem que você altere detalhes, como os endereços de ponto de extremidade (endereços reais usados para comunicação com o serviço) de cada computador. Além disso, o WCF inclui vários elementos fornecidos pelo sistema que permitem que você selecione rapidamente os recursos mais básicos para um serviço. A partir do .NET Framework 4, o WCF vem com um novo modelo de configuração padrão que simplifica os requisitos de configuração do WCF. Se você não fornecer nenhuma configuração WCF para um serviço específico, o tempo de execução configurará automaticamente seu serviço com alguns pontos de extremidade padrão e Associação/comportamento padrão. Na prática, escrever a configuração é uma parte importante da programação de aplicativos WCF.  
  
 Para obter mais informações, consulte [Configurando associações para serviços](configuring-bindings-for-wcf-services.md). Para obter uma lista dos elementos usados com mais frequência, consulte [associações fornecidas pelo sistema](system-provided-bindings.md). Para obter mais informações sobre pontos de extremidade, associações e comportamentos padrão, confira [Configuração simplificada](simplified-configuration.md) e [Configuração simplificada para serviços WCF](./samples/simplified-configuration-for-wcf-services.md).  
  
> [!IMPORTANT]
> Ao implantar os cenários lado a lado onde duas versões diferentes de um serviço são implantadas, é necessário especificar nomes parciais de assemblies referenciados em arquivos de configuração. Isso ocorre porque o arquivo de configuração é compartilhado entre todas as versões de um serviço e pode estar em execução em versões diferentes do .NET Framework.  
  
## <a name="systemconfiguration-webconfig-and-appconfig"></a>System.Configuration: Web.config e App.config  
 O WCF usa o sistema de configuração System.Configuração do .NET Framework.  
  
 Ao configurar um serviço no Visual Studio, use um arquivo de Web.config ou um arquivo de App.config para especificar as configurações. A escolha do nome do arquivo de configuração é determinada pelo ambiente de hospedagem que você escolhe para o serviço. Se você estiver usando o IIS para hospedar o serviço, use um arquivo Web.config. Se você estiver usando qualquer outro ambiente de hospedagem, use um arquivo App.config.  
  
 No Visual Studio, o arquivo chamado App.config é usado para criar o arquivo de configuração final. O nome final realmente usado para a configuração depende do nome do assembly. Por exemplo, um assembly denominado "Cohowinery.exe" tem um nome de arquivo de configuração final de "Cohowinery.exe.config". Entretanto, você precisa modificar somente o arquivo App.config. As alterações feitas nesse arquivo são automaticamente feitas no arquivo de configuração final do aplicativo em tempo de compilação.  
  
 Ao usar um arquivo App.config, o sistema de configuração mescla o arquivo App.config com o conteúdo do arquivo Machine.config quando o aplicativo é iniciado e a configuração é aplicada. Esse mecanismo permite que as configurações de todo o computador sejam definidas no arquivo Machine.config. O arquivo App.config pode ser usado para substituir as configurações do arquivo Machine.config. Você também pode bloquear as configurações no arquivo Machine.config para que sejam usadas. No caso do Web.config, o sistema de configuração mescla os arquivos Web.config em todos os diretórios que levam ao diretório do aplicativo na configuração que é aplicada. Para obter mais informações sobre a configuração e as prioridades de configuração, consulte os tópicos no <xref:System.Configuration> namespace.  
  
## <a name="major-sections-of-the-configuration-file"></a>Seções principais do arquivo de configuração  
 As seções principais do arquivo de configuração incluem os seguintes elementos.  
  
```xml  
<system.ServiceModel>  
  
   <services>  
   <!-- Define the service endpoints. This section is optional in the new  
    default configuration model in .NET Framework 4. -->  
      <service>  
         <endpoint/>  
      </service>  
   </services>  
  
   <bindings>  
   <!-- Specify one or more of the system-provided binding elements,  
    for example, <basicHttpBinding> -->
   <!-- Alternatively, <customBinding> elements. -->  
      <binding>  
      <!-- For example, a <BasicHttpBinding> element. -->  
      </binding>  
   </bindings>  
  
   <behaviors>  
   <!-- One or more of the system-provided or custom behavior elements. -->  
      <behavior>  
      <!-- For example, a <throttling> element. -->  
      </behavior>  
   </behaviors>  
  
</system.ServiceModel>  
```  
  
> [!NOTE]
> As seções de associações e de comportamentos são opcionais e são incluídas apenas se necessário.  
  
### <a name="the-services-element"></a>O \<services> elemento  
 O elemento `services` contém as especificações de todos os serviços que o aplicativo hospeda. Começando com o modelo de configuração simplificado no .NET Framework 4, esta seção é opcional.  
  
 [\<services>](../configure-apps/file-schema/wcf/services.md)  
  
### <a name="the-service-element"></a>O \<service> elemento  
 Cada serviço tem os seguintes atributos:  
  
- `name`. Especifica o tipo que fornece uma implementação de um contrato de serviço. É um nome totalmente qualificado que consiste no namespace, em um ponto e no nome do tipo. Por exemplo, `"MyNameSpace.myServiceType"`.  
  
- `behaviorConfiguration`. Especifica o nome de um dos elementos `behavior` localizados no elemento `behaviors`. O comportamento especificado governa as ações, como, por exemplo, se o serviço permite representação. Se seu valor for o nome vazio ou se nenhum `behaviorConfiguration` for fornecido, o conjunto padrão de comportamentos de serviço será adicionado ao serviço.  
  
- [\<service>](../configure-apps/file-schema/wcf/service.md)  
  
### <a name="the-endpoint-element"></a>O \<endpoint> elemento  
 Cada ponto de extremidade requer um endereço, uma associação e um contrato, que são representados pelos seguintes atributos:  
  
- `address`. Especifica o URI (Uniform Resource Identifier) do serviço, que pode ser um endereço absoluto ou um endereço fornecido em relação ao endereço básico do serviço. Se definido como uma cadeia de caracteres vazia, indicará que o ponto de extremidade está disponível no endereço básico especificado ao criar <xref:System.ServiceModel.ServiceHost> do serviço.  
  
- `binding`. Normalmente especifica uma associação fornecida pelo sistema, como <xref:System.ServiceModel.WSHttpBinding>, mas também pode especificar uma associação definida pelo usuário. A associação especificada determina o tipo de transporte, de segurança e de codificação usado, e se sessões confiáveis, transações ou streaming são suportados ou se estão habilitados.  
  
- `bindingConfiguration`. Se os valores padrão de uma associação precisarem ser modificados, isso poderá ser feito configurando o elemento `binding` apropriado no elemento `bindings`. Esse atributo deve receber o mesmo valor que o atributo `name` do elemento `binding` que é usado para alterar os padrões. Se nenhum nome for fornecido, ou se nenhum `bindingConfiguration` estiver especificado na associação, a associação padrão do tipo de associação será usada no ponto de extremidade.  
  
- `contract`. Especifica a interface que define o contrato. Essa é a interface implementada no tipo CLR (Common Language Runtime) especificado pelo atributo `name` do elemento `service`.  
  
- [\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md)  
  
### <a name="the-bindings-element"></a>O \<bindings> elemento  
 O elemento `bindings` contém as especificações de todas as associações que podem ser usadas por qualquer ponto de extremidade definido em qualquer serviço.  
  
 [\<bindings>](../configure-apps/file-schema/wcf/bindings.md)  
  
### <a name="the-binding-element"></a>O \<binding> elemento  
 Os `binding` elementos contidos no `bindings` elemento podem ser uma das associações fornecidas pelo sistema (consulte [associações fornecidas pelo sistema](system-provided-bindings.md)) ou uma associação personalizada (consulte [associações personalizadas](./extending/custom-bindings.md)). O elemento `binding` tem um atributo `name` que correlaciona a associação ao ponto de extremidade especificado no atributo `bindingConfiguration` do elemento `endpoint`. Se nenhum nome for especificado, a associação corresponderá ao padrão desse tipo de associação.  
  
Para obter mais informações sobre como configurar serviços e clientes, consulte [Configurando serviços WCF](configuring-services.md).
  
 [\<binding>](../configure-apps/file-schema/wcf/bindings.md)  
  
### <a name="the-behaviors-element"></a>O \<behaviors> elemento  
 Esse é um elemento contêiner dos elementos `behavior` que definem os comportamentos de um serviço.  
  
 [\<behaviors>](../configure-apps/file-schema/wcf/behaviors.md)  
  
### <a name="the-behavior-element"></a>O \<behavior> elemento  
 Cada `behavior` elemento é identificado por um `name` atributo e fornece um comportamento fornecido pelo sistema, como <`throttling`> ou um comportamento personalizado. Se nenhum nome for fornecido, esse elemento de comportamento corresponderá ao comportamento padrão do serviço ou do ponto de extremidade.  
  
 [\<behavior>](../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md)  
  
## <a name="how-to-use-binding-and-behavior-configurations"></a>Como usar as configurações de associação e de comportamento  
 O WCF facilita o compartilhamento de configurações entre pontos de extremidade usando um sistema de referência na configuração. Em vez de atribuir valores de configuração diretamente a um ponto de extremidade, os valores da configuração relacionados à associação são agrupados em elementos `bindingConfiguration` na seção `<binding>`. A configuração de uma associação é um grupo nomeado de configurações em uma associação. Os pontos de extremidade podem fazer referência a `bindingConfiguration` por nome.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
 <system.serviceModel>  
  <bindings>  
    <basicHttpBinding>  
     <binding name="myBindingConfiguration1" closeTimeout="00:01:00" />  
     <binding name="myBindingConfiguration2" closeTimeout="00:02:00" />  
     <binding closeTimeout="00:03:00" />  <!-- Default binding for basicHttpBinding -->  
    </basicHttpBinding>  
     </bindings>  
     <services>  
      <service name="MyNamespace.myServiceType">  
       <endpoint
          address="myAddress" binding="basicHttpBinding"
          bindingConfiguration="myBindingConfiguration1"  
          contract="MyContract"  />  
       <endpoint
          address="myAddress2" binding="basicHttpBinding"
          bindingConfiguration="myBindingConfiguration2"  
          contract="MyContract" />  
       <endpoint
          address="myAddress3" binding="basicHttpBinding"
          contract="MyContract" />  
       </service>  
      </services>  
    </system.serviceModel>  
</configuration>  
```  
  
 O `name` de `bindingConfiguration` é definido no elemento `<binding>`. O `name` deve ser uma cadeia de caracteres exclusiva dentro do escopo do tipo de associação — nesse caso, o [<BasicHttpBinding \> ](../configure-apps/file-schema/wcf/basichttpbinding.md)ou um valor vazio para se referir à associação padrão. O ponto de extremidade vincula-se à configuração definindo o atributo `bindingConfiguration` para essa cadeia de caracteres.  
  
 Um `behaviorConfiguration` é implementado da mesma forma, conforme ilustrado no exemplo a seguir.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="myBehavior">  
           <callbackDebug includeExceptionDetailInFaults="true" />  
         </behavior>  
      </endpointBehaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="true" />  
        </behavior>  
      </serviceBehaviors>  
  
    </behaviors>  
    <services>  
     <service name="NewServiceType">  
       <endpoint
          address="myAddress3" behaviorConfiguration="myBehavior"  
          binding="basicHttpBinding"  
          contract="MyContract" />  
      </service>  
    </services>  
   </system.serviceModel>  
</configuration>  
```  
  
 Observe que o conjunto padrão de comportamentos de serviço é adicionado ao serviço. Esse sistema permite que os pontos de extremidade compartilhem configurações comuns sem redefinir as configurações. Se o escopo de todo o computador for necessário, crie a configuração de associação ou comportamento no Machine.config. As definições de configuração estão disponíveis em todos os arquivos de App.config. A [ferramenta do editor de configuração (SvcConfigEditor.exe)](configuration-editor-tool-svcconfigeditor-exe.md) facilita a criação de configurações.  
  
## <a name="behavior-merge"></a>Mesclagem de comportamentos  
 O recurso de mesclagem de comportamentos facilita o gerenciamento dos comportamentos quando você deseja que um conjunto de comportamentos comuns sejam usados de maneira consistente. Esse recurso permite que você especifique comportamentos em diferentes níveis da hierarquia de configuração e faça com que os serviços herdem comportamentos de vários níveis da hierarquia de configuração. Para ilustrar como isso funciona, suponha que você tenha o seguinte layout de diretório virtual no IIS:  
  
 `~\Web.config~\Service.svc~\Child\Web.config~\Child\Service.svc`
  
 E seu `~\Web.config` arquivo tem o seguinte conteúdo:  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceDebug includeExceptionDetailInFaults="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 E você tem um Web.config filho localizado em ~\Child\Web.config com o seguinte conteúdo:  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 O serviço localizado em ~\Child\Service.svc se comportará como se tivesse os comportamentos de serviceDebug e de serviceMetadata. O serviço localizado em ~ \ Service.svc só terá o comportamento de serviceDebug. O que acontece é que as duas coleções de comportamentos com o mesmo nome (neste caso a cadeia de caracteres vazia) são mescladas.  
  
 Você também pode limpar as coleções de comportamento usando a \<clear> marca e removeu comportamentos individuais da coleção usando a \<remove> marca. Por exemplo, as duas configurações a seguir resultam no serviço filho que tem apenas o comportamento de serviceMetadata:  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <remove name="serviceDebug"/>  
          <serviceMetadata httpGetEnabled="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <clear/>  
          <serviceMetadata httpGetEnabled="True" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 A mesclagem de comportamentos é feita para coleções de comportamentos sem nome, conforme mostrado acima, e também para coleções de comportamentos nomeadas.  
  
 A mesclagem de comportamento funciona no ambiente de hospedagem do IIS, no qual Web.config arquivos são mesclados hierarquicamente com o arquivo de Web.config raiz e machine.config. Mas ele também funciona no ambiente de aplicativo, onde machine.config pode mesclar com o arquivo App.config.  
  
 A mesclagem de comportamentos se aplica aos comportamentos de ponto de extremidade e aos comportamentos de serviço na configuração.  
  
 Se uma coleção de comportamentos filho contiver um comportamento que já esteja presente na coleção de comportamentos pai, o comportamento filho substituirá o pai. Portanto, se uma coleção de comportamentos pai tivesse sido `<serviceMetadata httpGetEnabled="False" />` e uma coleção de comportamentos filho tivesse `<serviceMetadata httpGetEnabled="True" />` , o comportamento filho substituiria o comportamento pai na coleção de comportamentos e HttpGetEnabled seria "true".  
  
## <a name="see-also"></a>Veja também

- [Configuração simplificada](simplified-configuration.md)
- [Configurando serviços WCF](configuring-services.md)
- [\<service>](../configure-apps/file-schema/wcf/service.md)
- [\<binding>](../configure-apps/file-schema/wcf/bindings.md)
