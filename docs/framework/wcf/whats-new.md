---
title: Novidades no Windows Communication Foundation 4.5
description: Este artigo descreve os recursos novos para Windows Communication Foundation (WCF) versão 4,5 e links para recursos adicionais.
ms.date: 03/30/2017
helpviewer_keywords:
- WCF [WCF], what's new
- Windows Communication Foundation [WCF], what's new
ms.assetid: 7e93fe73-af93-46b5-9f63-32f761ee40cf
ms.openlocfilehash: b6ce7fe19a8d7cc00823502e322ee53a1bd0a931
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245616"
---
# <a name="whats-new-in-windows-communication-foundation-45"></a>Novidades no Windows Communication Foundation 4.5

Este tópico aborda os recursos novos para Windows Communication Foundation (WCF) versão 4,5.

## <a name="wcf-simplification-features"></a>funcionalidades de simplificação do WCF

Muito trabalho foi feito para facilitar o desenvolvimento e a manutenção de aplicativos do WCF 4.5. Para obter mais informações, consulte [recursos de simplificação do WCF](wcf-simplification-features.md).

### <a name="task-based-async-support"></a>Suporte assíncrono baseado em tarefas

Por padrão, Adicionar Referência de Serviço gera métodos de operação de serviço assíncrono de retorno de tarefas. Isso é feito para métodos síncronos e assíncronos. Isso permite que você chame as operações de serviço assincronamente usando o novo modelo de programação assíncrona baseado em tarefa. Quando você chama o método proxy gerado, o WCF constrói um objeto de tarefa para representar a operação assíncrona e retornar essa tarefa para você. A tarefa é concluída quando a operação é concluída. Ao implementar uma operação assíncrona, você pode implementá-la como uma operação assíncrona baseada em tarefa. Para obter mais informações, consulte [operações síncronas e assíncronas](synchronous-and-asynchronous-operations.md).

### <a name="simplified-generated-configuration-files"></a>Arquivos de configuração gerados simplificados

Quando você adiciona uma referência de serviço no Visual Studio ou usa a ferramenta SvcUtil.exe, um arquivo de configuração de cliente é gerado. Em versões anteriores do WCF, esses arquivos de configuração continham o valor de cada propriedade de associação mesmo se seu valor fosse o valor padrão. No WCF 4.5, os arquivos de configuração gerados contêm apenas as propriedades de associação que são definidas com um valor não padrão.

Para obter mais informações, consulte [recursos de simplificação do WCF](wcf-simplification-features.md).

### <a name="contract-first-development"></a>Desenvolvimento de primeiro contrato

O WCF agora tem suporte para desenvolvimento do primeiro contrato. O svcutil.exe tem um comutador/serviceContract que permite gerar contratos de serviço e de dados de um documento WSDL.

### <a name="add-service-reference-from-a-portable-subset-project"></a>Adicione uma referência de serviço de um projeto de subconjunto portátil

Os projetos de subconjuntos portáteis permitem que os programadores de assembly .NET mantenham uma única árvore de origem e um sistema de compilação enquanto ainda dão suporte a várias plataformas .NET (desktop, Silverlight, Windows Phone e Xbox). Os projetos de subconjuntos portáteis só fazem referência a bibliotecas portáteis .NET que são assemblies que podem ser usados em qualquer plataforma .NET. A experiência do desenvolvedor é a mesma que adicionar uma referência de serviço dentro de qualquer outro aplicativo cliente WCF. Para obter mais informações, consulte [Adicionar referência de serviço em um projeto de subconjunto portátil](add-service-reference-in-a-portable-subset-project.md).

### <a name="aspnet-compatibility-mode-default-changed"></a>Padrão do modo de compatibilidade do ASP.NET alterado

O WCF fornece o modo de compatibilidade do ASP.NET para conceder aos desenvolvedores acesso completo aos recursos no pipeline HTTP do ASP.NET ao escrever serviços do WCF. Para usar esse modo, você deve definir o `aspNetCompatibilityEnabled` atributo como true na [\<serviceHostingEnvironment>](../configure-apps/file-schema/wcf/servicehostingenvironment.md) seção de web.config. Além disso, qualquer serviço nesse appDomain precisa ter a `RequirementsMode` propriedade <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> definida como <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> or <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required> . Por padrão, <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> agora é definido como <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> . Para obter mais informações, consulte [Serviços WCF e ASP.net](./feature-details/wcf-services-and-aspnet.md).

### <a name="new-transport-default-values"></a>Novos valores padrão de transporte

Para simplificar a configuração, alguns valores padrão da propriedade de transporte foram alterados. Para obter mais informações, consulte [recursos de simplificação do WCF](wcf-simplification-features.md).

### <a name="xmldictionaryreaderquotas"></a>XmlDictionaryReaderQuotas

<xref:System.Xml.XmlDictionaryReaderQuotas> contém valores de cota configuráveis para os leitores de dicionário de XML que limitam a quantidade de memória utilizada por um codificador ao criar uma mensagem. Embora essas quotas sejam configuráveis, os valores padrão foram alterados para diminuir a possibilidade de um desenvolvedor precisar defini-las explicitamente. Para obter mais informações, consulte [recursos de simplificação do WCF](wcf-simplification-features.md).

### <a name="wcf-configuration-validation"></a>Validação da instalação do WCF

Como parte do processo de compilação dentro do Visual Studio, os arquivos de configuração do WCF agora são validados para os atributos definidos dentro do projeto. Uma lista de erros de validação ou avisos são exibidos no Visual Studio se a validação falhar.

### <a name="xml-editor-tooltips"></a>Dicas do editor XML

Para ajudar os desenvolvedores novos e existentes do serviço WCF a configurarem os serviços, o editor de XML do Visual Studio agora fornece dicas de ferramenta para cada elemento de configuração e suas propriedades que fazem parte do arquivo de configuração do serviço.

## <a name="streaming-improvements"></a>Melhorias de streaming

Suporte adicionado para streaming assíncrono verdadeiro onde o lado de envio agora não bloqueia threads se o lado de recebimento não está lendo ou lê lentamente, aumentando, portanto, a escalabilidade. Remove a limitação de armazenamento em buffer de mensagem quando um cliente envia uma mensagem transmitida por streaming para um serviço WCF hospedado no IIS. Para obter mais informações, consulte [recursos de simplificação do WCF](wcf-simplification-features.md).

## <a name="simplifying-exposing-an-endpoint-over-https-with-iis"></a>Simplificar a exposição de um ponto de extremidade em HTTPS com o IIS

Um mapeamento de protocolo de HTTPS foi adicionado para simplificar a exposição de um ponto de extremidade em HTTPS. Para habilitar um ponto de extremidade HTTPS, verifique se seu site com uma associação de HTTPS e o certificado de SSL configurado e, em seguida, basta habilitar o HTTPS para o diretório virtual que hospeda o serviço. Se os metadados estiverem habilitados para o serviço, isso será exposto por HTTPS também.

## <a name="generating-a-single-wsdl-document"></a>Gerando um único documento WSDL

Pilhas de processamento de WSDL de terceiros não podem processar os documentos WSDL que têm dependências de outros documentos por meio de um xsd:import. O WCF agora permite que você especifique que todas as informações de WSDL sejam retornadas em um único documento. Para solicitar um único documento WSDL, acrescente "? singleWSDL" ao URI ao solicitar metadados do serviço.

## <a name="websocket-support"></a>Suporte de WebSocket

WebSockets é uma tecnologia que fornece comunicação bidirecional verdadeira nas portas 80 e 443 com as características de desempenho semelhantes ao TCP. Duas novas associações foram adicionadas à comunicação de suporte em um transporte de WebSocket. <xref:System.ServiceModel.NetHttpBinding> e <xref:System.ServiceModel.NetHttpsBinding>. Para obter mais informações, consulte: [associações fornecidas pelo sistema](system-provided-bindings.md).

## <a name="new-transport-default-values"></a>Novos valores padrão de transporte

A tabela a seguir descreve as configurações que foram alteradas e onde localizar informações adicionais.

|Propriedade|Ativado|Novo padrão|Para obter mais informações, consulte|
|--------------|--------|-----------------|------------------------------|
|channelInitializationTimeout|<xref:System.ServiceModel.NetTcpBinding>|30 segundos|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.ChannelInitializationTimeout%2A>|
|listenBacklog|<xref:System.ServiceModel.NetTcpBinding>|12 * número de processadores|<xref:System.ServiceModel.NetTcpBinding.ListenBacklog%2A>|
|maxPendingAccepts|ConnectionOrientedTransportBindingElement<br /><br /> SMSvcHost.exe|2 * número de processadores para o transporte<br /><br /> 4 \* números de processadores para SMSvcHost.exe|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingAccepts%2A> [Configurando o serviço de compartilhamento de porta Net.TCP](./feature-details/configuring-the-net-tcp-port-sharing-service.md)|
|maxPendingConnections|ConnectionOrientedTransportBindingElement|12 * número de processadores|<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingConnections%2A>|
|receiveTimeout|SMSvcHost.exe|30 segundos|[Configurando o serviço de compartilhamento de porta Net.TCP](./feature-details/configuring-the-net-tcp-port-sharing-service.md)|

## <a name="xml-editor-tooltips"></a>Dicas do editor XML

Para ajudar os desenvolvedores novos e existentes do serviço WCF a configurarem os serviços, o editor de XML do Visual Studio agora fornece dicas de ferramenta para cada elemento de configuração e suas propriedades que fazem parte do arquivo de configuração do serviço.

## <a name="configuring-wcf-services-in-code"></a>Configurando serviços WCF em código

Windows Communication Foundation (WCF) permite que os desenvolvedores configurem serviços usando arquivos de configuração ou código. Os arquivos de configuração são úteis quando um serviço precisa ser configurado depois de ser implantado. Ao usar arquivos de configuração, um profissional de TI apenas precisa atualizar o arquivo de configuração, nenhuma recompilação é necessária. Os arquivos de configuração, porém, podem ser complexos e difíceis de manter. Não há suporte para depurar arquivos de configuração e os elementos de configuração são referenciados por nomes, o que torna os arquivos de configuração de criação sujeitos a erros e difíceis. O WCF também permite que você configure serviços no código. Em versões anteriores do WCF (4,0 e anteriores) a configuração de serviços no código era fácil em cenários de hospedagem interna, a <xref:System.ServiceModel.ServiceHost> classe permitia que você configurasse pontos de extremidade e comportamentos antes de chamar ServiceHost. Open. Em cenários hospedados na Web, porém, você não tem acesso à classe <xref:System.ServiceModel.ServiceHost>. Para configurar um serviço Web hospedado, você precisava criar um `System.ServiceModel.ServiceHostFactory` que criou o <xref:System.ServiceModel.Activation.ServiceHostFactory> e executar qualquer configuração necessária. A partir do .NET 4,5, o WCF fornece uma maneira mais fácil de configurar os serviços hospedados internamente e Web no código. Para obter mais informações, consulte [Configuring WCF Services in Code](configuring-wcf-services-in-code.md).

## <a name="channelfactory-caching"></a>Cache de ChannelFactory

Os aplicativos cliente do WCF usam a classe <xref:System.ServiceModel.ChannelFactory%601> para criar um canal de comunicação com um serviço WCF. Criar instâncias de <xref:System.ServiceModel.ChannelFactory%601> resulta em alguma sobrecarga porque envolve as seguintes operações:

1. Construir a árvore de <xref:System.ServiceModel.Description.ContractDescription>

2. Refletir todos os tipos CLR necessários

3. Construir a pilha de canal

4. Descarte de recursos

Para ajudar a minimizar a sobrecarga, o WCF pode armazenar em cache fábricas de canal quando você está usando um proxy de cliente WCF. Para obter mais informações, consulte [fábrica de canais e Caching](./feature-details/channel-factory-and-caching.md).

## <a name="compression-and-the-binary-encoder"></a>Compactação e o codificador binário

A partir do WCF 4.5, o codificador binário do WCF adiciona suporte para compactação. O tipo de compactação é configurado com a propriedade <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A>. O cliente e o serviço devem configurar a propriedade <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement.CompressionFormat%2A>. A compactação funcionará para os protocolos de HTTP, HTTPS e TCP. Se um cliente especificar o uso de compactação, mas o serviço não oferece suporte a isso, uma exceção de protocolo será gerada indicando uma incompatibilidade de protocolo. Para obter mais informações, consulte [escolhendo um codificador de mensagem](./feature-details/choosing-a-message-encoder.md).

## <a name="udp"></a>UDP

Foi adicionado suporte para um transporte UDP que permite aos desenvolvedores escrever serviços que usam mensagens "incêndio e esquecido". Um cliente envia uma mensagem para um serviço e não espera nenhuma resposta do serviço.

## <a name="multiple-authentication-support"></a>Suporte a várias autenticações

O suporte foi adicionado para dar suporte a vários modos de autenticação, conforme o suporte pelo IIS, em um único ponto de extremidade do WCF ao usar o transporte de HTTP e a segurança de transporte. O IIS permite que você habilite vários modos de autenticação em um diretório virtual. Esse recurso permite que um único ponto de extremidade WCF dê suporte a vários modos de autenticação habilitados para o diretório virtual em que o serviço WCF está hospedado.

## <a name="idn-support"></a>Suporte a IDN

O suporte foi adicionado para permitir os serviços WCF com nomes de domínio internacionalizados. Para obter mais informações, consulte [WCF e nomes de domínio internacionalizados](./feature-details/wcf-and-internationalized-domain-names.md).

## <a name="httpclient"></a>HttpClient

Uma nova classe chamada <xref:System.Net.Http.HttpClient> foi adicionada para facilitar muito o trabalho com solicitações de HTTP. Para obter mais informações, consulte [tornando os aplicativos sociais e conectados com os serviços http](https://channel9.msdn.com/Events/BUILD/BUILD2011/PLAT-581T) e o [exemplo de cliente http](https://code.msdn.microsoft.com/windowsapps/HttpClient-sample-55700664).

## <a name="configuration-intellisense"></a>Intellisense de configuração

Os valores de atributo nos arquivos de configuração para os atributos personalizados definidos no projeto agora dão suporte ao intellisense para facilitar o trabalho com configurações de maneira rápida e precisa.

## <a name="configuration-tooltips"></a>Dicas de ferramenta de configuração

Os elementos e atributos do WCF agora têm dicas de ferramenta no editor de XML para identificar com mais facilidade e precisão a finalidade do elemento ou atributo.

## <a name="paste-data-as-classes"></a>Colar dados como classes

Em um projeto WCF, os tipos de dados definidos no XML (como são expostos em um serviço) podem ser colados diretamente em uma página de código. O tipo de XML será colado como um tipo CLR. Consulte [gerando classes de tipo de dados do XML](generating-data-type-classes-from-xml.md) para obter mais detalhes.

## <a name="webservicehost-and-default-endpoints"></a>WebServiceHost e pontos de extremidade padrão

No Visual Studio 2010, o WebServiceHost automaticamente criava um ponto de extremidade padrão se você tivesse especificado explicitamente um ponto de extremidade ou não. No Visual Studio 2012 e posterior, WebServiceHost só cria um ponto de extremidade padrão se nenhum ponto de extremidades é explicitamente adicionado. Se o cliente estiver esperando o ponto de extremidade padrão, você poderá adicionar explicitamente um ponto de extremidade e apontar o cliente para ele. Como alternativa, você pode informar WCF para reverter para o comportamento anterior adicionando a seguinte configuração ao arquivo de configuração do aplicativo

```xml
<appSettings>
    <add key="wcf:webservicehost:enableautomaticendpointscompatability" value="true"/>
  </appSettings>
```

## <a name="ihttpcookiecontainermanager"></a>IHttpCookieContainerManager

Essa interface, exposta pelo <xref:System.ServiceModel.Channels.IChannelFactory%601>, facilita muito o trabalho com cookies no lado do cliente. Quando AllowCookies está definido como verdadeiro na associação, você poderá acessar os cookies usando o seguinte código:

```csharp
IHttpCookieContainerManager cookieManager = factory.GetProperty<IHttpCookieContainerManager>();
System.Net.CookieContainer container = cookieManager.CookieContainer;
```

Você pode recuperar ou definir cookies a partir do <xref:System.Net.CookieContainer>. Quando AllowCookies for definido como falso, você poderá recuperar os cookies manualmente usando <xref:System.ServiceModel.OperationContext> e enviá-los em outras solicitações usando outro <xref:System.ServiceModel.OperationContext> ou inspetor da mensagem. A interface de IHttpCookieContainerManager permite autenticar um usuário com um serviço e usar o cookie de autenticação retornado por esse serviço para autenticar com outros serviços.
