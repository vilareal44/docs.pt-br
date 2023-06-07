---
title: Compatibilidade da funcionalidade de confiança parcial
ms.date: 03/30/2017
ms.assetid: a36a540b-1606-4e63-88e0-b7c59e0e6ab7
ms.openlocfilehash: 85e34e365d125fe4f00756549ba5bda4311b78f8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84579157"
---
# <a name="partial-trust-feature-compatibility"></a>Compatibilidade da funcionalidade de confiança parcial
O Windows Communication Foundation (WCF) dá suporte a um subconjunto limitado de funcionalidade durante a execução em um ambiente parcialmente confiável. Os recursos com suporte em confiança parcial são projetados em um conjunto específico de cenários, conforme descrito no tópico [cenários de implantação com suporte](supported-deployment-scenarios.md) .  
  
## <a name="minimum-permission-requirements"></a>Requisitos mínimos de permissão  
 O WCF dá suporte a um subconjunto de recursos em aplicativos em execução em qualquer um dos seguintes conjuntos de permissões nomeados padrão:  
  
- Permissões de confiança média  
  
- Permissões de zona da Internet  
  
 A tentativa de usar o WCF em aplicativos parcialmente confiáveis com permissões mais restritivas pode resultar em exceções de segurança em tempo de execução.  
  
## <a name="contracts"></a>Contratos  
 Os contratos estão sujeitos às seguintes restrições ao serem executados sob confiança parcial:  
  
- A classe de serviço que implementa a `[ServiceContract]` interface deve ser `public` e ter um `public` Construtor. Se ele definir `[OperationContract]` métodos, eles deverão ser `public` . Se, em vez disso, ele implementa uma `[ServiceContract]` interface, essas implementações de método podem ser explícitas ou `private` , desde que a `[ServiceContract]` interface seja `public` .  
  
- Ao usar o `[ServiceKnownType]` atributo, o método especificado deve ser `public` .  
  
- `[MessageContract]`as classes e seus membros podem ser `public` . Se a `[MessageContract]` classe for definida no assembly do aplicativo, ela poderá ser `internal` e ter `internal` Membros.  
  
## <a name="system-provided-bindings"></a>Associações fornecidas pelo sistema  
 O <xref:System.ServiceModel.BasicHttpBinding> e o <xref:System.ServiceModel.WebHttpBinding> têm suporte total em um ambiente de confiança parcial. O <xref:System.ServiceModel.WSHttpBinding> tem suporte apenas para o modo de segurança de transporte.  
  
 Associações que usam transportes que não sejam HTTP, como, <xref:System.ServiceModel.NetTcpBinding> <xref:System.ServiceModel.NetNamedPipeBinding> ou, não têm <xref:System.ServiceModel.NetMsmqBinding> suporte quando são executadas em um ambiente de confiança parcial.  
  
## <a name="custom-bindings"></a>Associações personalizadas  
 Associações personalizadas podem ser criadas e usadas em um ambiente de confiança parcial, mas devem seguir as restrições especificadas nesta seção.  
  
### <a name="transports"></a>Transportes  
 Os únicos elementos de associação de transporte permitidos são <xref:System.ServiceModel.Channels.HttpTransportBindingElement> e <xref:System.ServiceModel.Channels.HttpsTransportBindingElement> .  
  
### <a name="encoders"></a>Codificadores  
 Os codificadores a seguir são permitidos:  
  
- O codificador de texto ( <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> ).  
  
- O codificador binário ( <xref:System.ServiceModel.Channels.BinaryMessageEncodingBindingElement> ).  
  
- O codificador de mensagem da Web ( <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement> ).  
  
 Não há suporte para os codificadores MTOM (mecanismo de otimização de transmissão de mensagens).  
  
### <a name="security"></a>Segurança  
 Os aplicativos parcialmente confiáveis podem usar os recursos de segurança de nível de transporte do WCF para proteger sua comunicação. Não há suporte para segurança em nível de mensagem. Configurar uma associação para usar a segurança no nível de mensagem resulta em uma exceção em tempo de execução.  
  
### <a name="unsupported-bindings"></a>Associações sem suporte  
 Não há suporte para associações que usam mensagens confiáveis, transações ou segurança no nível de mensagem.  
  
## <a name="serialization"></a>Serialização  
 Tanto o <xref:System.Runtime.Serialization.DataContractSerializer> quanto o <xref:System.Xml.Serialization.XmlSerializer> têm suporte em um ambiente de confiança parcial. No entanto, o uso do <xref:System.Runtime.Serialization.DataContractSerializer> está sujeito às seguintes condições:  
  
- Todos os `[DataContract]` tipos serializáveis devem ser `public` .  
  
- Todos os `[DataMember]` campos serializáveis ou propriedades em um `[DataContract]` tipo devem ser públicos e de leitura/gravação. Não há suporte para a serialização e desserialização de campos [ReadOnly](https://go.microsoft.com/fwlink/?LinkID=98854) ao executar o WCF em um aplicativo parcialmente confiável.  
  
- `[Serializable]`Não há suporte para o modelo de programação/ISerializable em um ambiente de confiança parcial.  
  
- Tipos conhecidos devem ser especificados no código ou na configuração no nível do computador (Machine. config). Tipos conhecidos não podem ser especificados na configuração em nível de aplicativo por motivos de segurança.  
  
- Os tipos que implementam <xref:System.Runtime.Serialization.IObjectReference> geram uma exceção em um ambiente parcialmente confiável.  
  
 Consulte a seção serialização em [práticas recomendadas de confiança parcial](partial-trust-best-practices.md) para obter mais informações sobre segurança ao usar com <xref:System.Runtime.Serialization.DataContractSerializer> segurança em um aplicativo parcialmente confiável.  
  
### <a name="collection-types"></a>Tipos de coleção  
 Alguns tipos de coleção implementam <xref:System.Collections.Generic.IEnumerable%601> e <xref:System.Collections.IEnumerable> . Os exemplos incluem tipos que implementam <xref:System.Collections.Generic.ICollection%601> . Esses tipos podem implementar uma `public` implementação de `GetEnumerator()` e uma implementação explícita do `GetEnumerator()` . Nesse caso, <xref:System.Runtime.Serialization.DataContractSerializer> o invoca a `public` implementação de `GetEnumerator()` , e não a implementação explícita de `GetEnumerator()` . Se nenhuma das `GetEnumerator()` implementações for `public` e todas forem implementações explícitas, o <xref:System.Runtime.Serialization.DataContractSerializer> invocará `IEnumerable.GetEnumerator()` .  
  
 Para tipos de coleção quando o WCF está sendo executado em um ambiente de confiança parcial, se nenhuma das `GetEnumerator()` implementações for `public` ou nenhuma delas for implementações de interface explícitas, uma exceção de segurança será lançada.  
  
### <a name="netdatacontractserializer"></a>NetDataContractSerializer  
 Muitos .NET Framework tipos de coleção, <xref:System.Collections.Generic.List%601> como <xref:System.Collections.ArrayList> , <xref:System.Collections.Generic.Dictionary%602> e <xref:System.Collections.Hashtable> não têm suporte pelo <xref:System.Runtime.Serialization.NetDataContractSerializer> em confiança parcial. Esses tipos têm o `[Serializable]` atributo definido e, como mencionado anteriormente na seção de serialização, esse atributo não tem suporte em confiança parcial. O <xref:System.Runtime.Serialization.DataContractSerializer> trata as coleções de uma maneira especial e, portanto, é capaz de contornar essa restrição, mas o <xref:System.Runtime.Serialization.NetDataContractSerializer> não tem esse mecanismo para burlar essa restrição.  
  
 O <xref:System.DateTimeOffset> tipo não é suportado pelo <xref:System.Runtime.Serialization.NetDataContractSerializer> em confiança parcial.  
  
 Um substituto não pode ser usado com o <xref:System.Runtime.Serialization.NetDataContractSerializer> (usando o <xref:System.Runtime.Serialization.SurrogateSelector> mecanismo) durante a execução em confiança parcial. Observe que essa restrição se aplica ao uso de um substituto, não à serialização.  
  
## <a name="enabling-common-behaviors-to-run"></a>Habilitando comportamentos comuns para execução  
 Os comportamentos de serviço ou ponto de extremidade não marcados com o <xref:System.Security.AllowPartiallyTrustedCallersAttribute> atributo (APTCA) que são adicionados à [\<commonBehaviors>](../../configure-apps/file-schema/wcf/commonbehaviors.md) seção de um arquivo de configuração não são executados quando o aplicativo é executado em um ambiente de confiança parcial e nenhuma exceção é lançada quando isso ocorre. Para impor a execução de comportamentos comuns, você deve executar uma das seguintes opções:  
  
- Marque seu comportamento comum com o <xref:System.Security.AllowPartiallyTrustedCallersAttribute> atributo para que ele possa ser executado quando implantado como um aplicativo de confiança parcial. Observe que uma entrada de registro pode ser definida no computador para impedir a execução de assemblies marcados com APTCA. .  
  
- Certifique-se de que, se o aplicativo for implantado como um aplicativo totalmente confiável, que os usuários não possam modificar as configurações de segurança de acesso ao código para executar o aplicativo em um ambiente de confiança parcial. Se puderem fazer isso, o comportamento não é executado e nenhuma exceção é lançada. Para garantir isso, consulte a opção **LevelFinal** usando [Caspol. exe (ferramenta de política de segurança de acesso do código)](../../tools/caspol-exe-code-access-security-policy-tool.md).  
  
 Para obter um exemplo de comportamento comum, consulte [como bloquear pontos de extremidade na empresa](../extending/how-to-lock-down-endpoints-in-the-enterprise.md).  
  
## <a name="configuration"></a>Configuração  
 Com uma exceção, o código parcialmente confiável só pode carregar seções de configuração do WCF no `app.config` arquivo local. Para carregar as seções de configuração do WCF que referenciam as seções do WCF em Machine. config ou em um arquivo Web. config de raiz requer ConfigurationPermission (Irrestrito). Sem essa permissão, as referências às seções de configuração do WCF (comportamentos, associações) fora do arquivo de configuração local resultam em uma exceção quando a configuração é carregada.  
  
 A única exceção é a configuração de tipo conhecido para serialização, conforme descrito na seção serialização deste tópico.  
  
> [!IMPORTANT]
> As extensões de configuração só têm suporte quando são executadas em confiança total.  
  
## <a name="diagnostics"></a>Diagnósticos  
  
### <a name="event-logging"></a>Log de eventos  
 Há suporte para o log de eventos limitado sob confiança parcial. Somente as falhas de ativação de serviço e de rastreamento/log de mensagens são registradas no log de eventos. O número máximo de eventos que podem ser registrados por um processo é 5, para evitar a gravação de mensagens excessivas no log de eventos.  
  
### <a name="message-logging"></a>Registro em log de mensagens  
 O log de mensagens não funciona quando o WCF é executado em um ambiente de confiança parcial. Se habilitado sob confiança parcial, ele não falha na ativação do serviço, mas nenhuma mensagem é registrada.  
  
### <a name="tracing"></a>Rastreamento  
 A funcionalidade de rastreamento restrito está disponível durante a execução em um ambiente de confiança parcial. No elemento <`listeners`> no arquivo de configuração, os únicos tipos que você pode adicionar são <xref:System.Diagnostics.TextWriterTraceListener> e o novo <xref:System.Diagnostics.EventSchemaTraceListener> . O uso do padrão <xref:System.Diagnostics.XmlWriterTraceListener> pode resultar em logs incompletos ou incorretos.  
  
 As origens de rastreamento com suporte são:  
  
- <xref:System.ServiceModel>  
  
- <xref:System.Runtime.Serialization>  
  
- <xref:System.IdentityModel.Claims>, <xref:System.IdentityModel.Policy>, <xref:System.IdentityModel.Selectors> e <xref:System.IdentityModel.Tokens>.  
  
 Não há suporte para as seguintes fontes de rastreamento:  
  
- CardSpace  
  
- <xref:System.IO.Log>  

- [System. ServiceModel. Internal. TransactionBridge](https://docs.microsoft.com/previous-versions/aa346556(v=vs.110))]
  
 Os seguintes membros da <xref:System.Diagnostics.TraceOptions> enumeração não devem ser especificados:  
  
- <xref:System.Diagnostics.TraceOptions.Callstack?displayProperty=nameWithType>  
  
- <xref:System.Diagnostics.TraceOptions.ProcessId?displayProperty=nameWithType>  
  
 Ao usar o rastreamento em um ambiente de confiança parcial, verifique se o aplicativo tem permissões suficientes para armazenar a saída do ouvinte de rastreamento. Por exemplo, ao usar o <xref:System.Diagnostics.TextWriterTraceListener> para gravar a saída de rastreamento em um arquivo de texto, verifique se o aplicativo tem a FileIOPermission necessária necessária para gravar com êxito no arquivo de rastreamento.  
  
> [!NOTE]
> Para evitar a inundação dos arquivos de rastreamento com erros duplicados, o WCF desabilita o rastreamento do recurso ou da ação após a primeira falha de segurança. Há um rastreamento de exceção para cada acesso de recurso com falha, a primeira vez que uma tentativa é feita para acessar o recurso ou executar a ação.  
  
## <a name="wcf-service-host"></a>Host de serviço WCF  
 O host de serviço WCF não oferece suporte a confiança parcial. Se você quiser usar um serviço WCF em confiança parcial, não use o modelo de projeto da biblioteca de serviço WCF no Visual Studio para compilar seu serviço. Em vez disso, crie um novo site no Visual Studio escolhendo o modelo de site do serviço WCF, que pode hospedar o serviço em um servidor Web no qual a confiança parcial do WCF tem suporte.  
  
## <a name="other-limitations"></a>Outras limitações  

  O WCF é geralmente limitado às considerações de segurança impostas por ele pelo aplicativo de hospedagem. Por exemplo, se o WCF estiver hospedado em um aplicativo de navegador XAML (XBAP), ele estará sujeito a limitações do XBAP, conforme descrito em [Windows Presentation Foundation segurança de confiança parcial](../../wpf/wpf-partial-trust-security.md).  
  
 Os seguintes recursos adicionais não estão habilitados ao executar o Indigo2 em um ambiente de confiança parcial:  
  
- WMI (Instrumentação de Gerenciamento do Windows)  
  
- O log de eventos só é parcialmente habilitado (consulte a discussão na seção **diagnóstico** ).  
  
- Contadores de desempenho  
  
 O uso de recursos do WCF que não têm suporte em um ambiente de confiança parcial pode resultar em exceções em tempo de execução.  
  
## <a name="unlisted-features"></a>Recursos não listados  
 A melhor maneira de descobrir que uma informação ou ação está indisponível durante a execução em um ambiente de confiança parcial é tentar acessar o recurso ou fazer a ação dentro de um `try` bloco e, em seguida, `catch` a falha. Para evitar a inundação dos arquivos de rastreamento com erros duplicados, o WCF desabilita o rastreamento do recurso ou da ação após a primeira falha de segurança. Há um rastreamento de exceção para cada acesso de recurso com falha, a primeira vez que uma tentativa é feita para acessar o recurso ou executar a ação.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Channels.HttpTransportBindingElement>
- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>
- <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement>
- <xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement>
- [Cenários de implantação com suporte](supported-deployment-scenarios.md)
- [Práticas recomendadas de confiança parcial](partial-trust-best-practices.md)
