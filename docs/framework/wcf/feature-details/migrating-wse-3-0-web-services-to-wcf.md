---
title: Migrando serviços Web de WSE 3.0 para o WCF
ms.date: 03/30/2017
ms.assetid: 7bc5fff7-a2b2-4dbc-86cc-ecf73653dcdc
ms.openlocfilehash: ecf27c227b3e39d0c449a1d2ff32dc5bd59c750b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84598782"
---
# <a name="migrating-wse-30-web-services-to-wcf"></a>Migrando serviços Web de WSE 3.0 para o WCF
Os benefícios da migração de serviços Web do WSE 3,0 para o Windows Communication Foundation (WCF) incluem um desempenho aprimorado e o suporte a transportes adicionais, cenários de segurança adicionais e especificações WS-*. Um serviço Web que é migrado do WSE 3,0 para o WCF pode ter até uma melhoria de desempenho de 200% a 400%. Para obter mais informações sobre os transportes com suporte do WCF, consulte [escolhendo um transporte](choosing-a-transport.md). Para obter uma lista dos cenários com suporte do WCF, consulte [cenários comuns de segurança](common-security-scenarios.md). Para obter uma lista das especificações com suporte pelo WCF, consulte [Guia de interoperabilidade de protocolos de serviços Web](web-services-protocols-interoperability-guide.md).  
  
 As seções a seguir fornecem orientação sobre como migrar um recurso específico de um serviço Web do WSE 3,0 para o WCF.  
  
## <a name="general"></a>Geral  
 Os aplicativos WSE 3,0 e WCF incluem interoperabilidade de nível de cabo e um conjunto comum de terminologias. Os aplicativos WSE 3,0 e WCF são interoperáveis de nível de conexão com base no conjunto de especificações WS-* que ambos dão suporte. Quando um aplicativo WSE 3,0 ou WCF é desenvolvido, há um conjunto comum de terminologias, como os nomes das asserções de segurança fechadas no WSE e nos modos de autenticação.  
  
 Embora existam muitos aspectos semelhantes entre os modelos de programação WCF e ASP.NET ou WSE 3,0, eles são diferentes. Para obter detalhes sobre o modelo de programação do WCF, consulte [programação básica do ciclo de vida](../basic-programming-lifecycle.md).  
  
> [!NOTE]
> Para migrar um serviço Web do WSE para o WCF, a ferramenta de [Utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) pode ser usada para gerar um cliente. Esse cliente, no entanto, contém interfaces e classes que também podem ser usadas como um ponto de partida para um serviço WCF. As interfaces que são geradas têm o <xref:System.ServiceModel.OperationContractAttribute> atributo aplicado aos membros do contrato com a <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> propriedade definida como `*` . Quando um cliente WSE chama um serviço Web com essa configuração, a seguinte exceção é gerada: **Web. Services3. ResponseProcessingException: WSE910: ocorreu um erro durante o processamento de uma mensagem de resposta, e você pode encontrar o erro na exceção interna**. Para atenuar isso, defina a <xref:System.ServiceModel.OperationContractAttribute.ReplyAction%2A> Propriedade do <xref:System.ServiceModel.OperationContractAttribute> atributo como um `null` valor diferente, como `http://Microsoft.WCF.Documentation/ResponseToOCAMethod` .  
  
## <a name="security"></a>Segurança  
  
### <a name="wse-30-web-services-that-are-secured-using-a-policy-file"></a>Serviços Web do WSE 3,0 que são protegidos usando um arquivo de política  
 Os serviços WCF podem usar um arquivo de configuração para proteger um serviço e esse mecanismo é semelhante a um arquivo de política do WSE 3,0. No WSE 3,0 ao proteger um serviço Web usando um arquivo de política, você pode usar uma declaração de segurança completa ou uma declaração de política personalizada. As declarações de segurança descompletadas são mapeadas de forma próxima ao modo de autenticação de um elemento de associação de segurança do WCF. Não apenas os modos de autenticação do WCF e as declarações de segurança do WSE 3,0 são fechadas com o mesmo nome ou da mesma forma, eles protegem as mensagens usando os mesmos tipos de credencial. Por exemplo, a `usernameForCertificate` declaração de segurança pronta para uso no WSE 3,0 é mapeada para o `UsernameForCertificate` modo de autenticação no WCF. Os exemplos de código a seguir demonstram como uma política mínima que usa a `usernameForCertificate` declaração de segurança pronta no WSE 3,0 é mapeada para um `UsernameForCertificate` modo de autenticação no WCF em uma associação personalizada.  
  
 **WSE 3,0**  
  
```xml  
<policies>  
  <policy name="MyPolicy">  
    <usernameForCertificate messageProtectionOrder="SignBeforeEncrypt"  
                            requireDeriveKeys="true"/>  
  </policy>  
</policies>  
```  
  
 **WCF**  
  
```xml  
<customBinding>  
  <binding name="MyBinding">  
    <security authenticationMode="UserNameForCertificate"
              messageProtectionOrder="SignBeforeEncrypt"  
              requireDerivedKeys="true"/>  
  </binding>  
</customBinding>  
```  
  
 Para migrar as configurações de segurança de um serviço Web WSE 3,0 que são especificadas em um arquivo de política para o WCF, uma associação personalizada deve ser criada em um arquivo de configuração e a declaração de segurança pronta para uso deve ser definida para seu modo de autenticação equivalente. Além disso, a associação personalizada deve ser configurada para usar a especificação WS-Addressing de agosto de 2004 quando os clientes do WSE 3,0 se comunicam com o serviço. Quando o serviço WCF migrado não requer comunicação com clientes WSE 3,0 e só deve manter a paridade de segurança, considere usar as associações definidas pelo sistema do WCF com as configurações de segurança apropriadas em vez de criar uma associação personalizada.  
  
 A tabela a seguir lista o mapeamento entre um arquivo de política do WSE 3,0 e a associação personalizada equivalente no WCF.  
  
|Declaração de segurança do WSE 3,0 pronto para uso|Configuração de associação personalizada do WCF|  
|----------------------------------------|--------------------------------------|  
|\<usernameOverTransportSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="UserNameOverTransport" />     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<mutualCertificate10Security />|`<customBinding>   <binding name="MyBinding">     <security messageSecurityVersion="WSSecurity10WSTrustFebruary2005WSSecureConversationFebruary2005WSSecurityPolicy11BasicSecurityProfile10" authenticationMode="MutualCertificate" />     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<usernameForCertificateSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="UsernameForCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<anonymousForCertificateSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="AnonymousForCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<kerberosSecurity />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="Kerberos"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
|\<mutualCertificate11Security />|`<customBinding>   <binding name="MyBinding">     <security authenticationMode="MutualCertificate"/>     <textMessageEncoding messageVersion="Soap12WSAddressingAugust2004" />   </binding> </customBinding>`|  
  
 Para obter mais informações sobre como criar associações personalizadas no WCF, consulte [associações personalizadas](../extending/custom-bindings.md).  
  
### <a name="wse-30-web-services-that-are-secured-using-application-code"></a>Serviços Web do WSE 3,0 que são protegidos usando o código do aplicativo  
 Se o WSE 3,0 ou o WCF for usado, os requisitos de segurança podem ser especificados no código do aplicativo, em vez de na configuração. No WSE 3,0, isso é feito pela criação de uma classe derivada da `Policy` classe e, em seguida, pela adição dos requisitos chamando o `Add` método. Para obter mais detalhes sobre como especificar os requisitos de segurança no código, consulte [como proteger um serviço Web sem usar um arquivo de política](https://docs.microsoft.com/previous-versions/dotnet/netframework-2.0/aa528763(v=msdn.10)). No WCF, para especificar os requisitos de segurança no código, crie uma instância da <xref:System.ServiceModel.Channels.BindingElementCollection> classe e adicione uma instância de a <xref:System.ServiceModel.Channels.SecurityBindingElement> ao <xref:System.ServiceModel.Channels.BindingElementCollection> . Os requisitos de asserção de segurança são definidos usando os métodos auxiliares do modo de autenticação estática da <xref:System.ServiceModel.Channels.SecurityBindingElement> classe. Para obter mais detalhes sobre como especificar os requisitos de segurança no código usando o WCF, consulte [como: criar uma associação personalizada usando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md) e [como criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md).  
  
### <a name="wse-30-custom-policy-assertion"></a>Declaração de política personalizada do WSE 3,0  
 No WSE 3,0, há dois tipos de declarações de política personalizada: aqueles que protegem uma mensagem SOAP e aqueles que não protegem uma mensagem SOAP. As declarações de política que protegem mensagens SOAP derivam da classe WSE 3,0 `SecurityPolicyAssertion` e o equivalente conceitual no WCF é a <xref:System.ServiceModel.Channels.SecurityBindingElement> classe.  
  
 Um ponto importante a ser observado é que as asserções de segurança do WSE 3,0 prontos são um subconjunto dos modos de autenticação do WCF. Se você tiver criado uma declaração de política personalizada no WSE 3,0, pode haver um modo de autenticação do WCF equivalente. Por exemplo, o WSE 3,0 não fornece uma asserção de segurança CertificateOverTransport que é equivalente à `UsernameOverTransport` declaração de segurança pronta para uso, mas usa um certificado X. 509 para fins de autenticação de cliente. Se você definiu sua própria declaração de política personalizada para esse cenário, o WCF torna a migração direta. O WCF define um modo de autenticação para esse cenário, para que você possa aproveitar os métodos auxiliares do modo de autenticação estática para configurar um WCF <xref:System.ServiceModel.Channels.SecurityBindingElement> .  
  
 Quando não há um modo de autenticação do WCF equivalente a uma declaração de política personalizada que protege mensagens SOAP, derive uma classe de <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> classes do ou do <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> WCF e especifique o elemento de associação equivalente. Para obter mais detalhes, consulte [como: criar uma associação personalizada usando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md).  
  
 Para converter uma declaração de política personalizada que não protege uma mensagem SOAP, consulte [filtragem](filtering.md) e o [interceptador de mensagem personalizado](../samples/custom-message-interceptor.md)de exemplo.  
  
### <a name="wse-30-custom-security-token"></a>Token de segurança personalizado do WSE 3,0  
 O modelo de programação do WCF para criar um token personalizado é diferente do WSE 3,0. Para obter detalhes sobre como criar um token personalizado no WSE, consulte [criando tokens de segurança personalizados](https://docs.microsoft.com/previous-versions/dotnet/netframework-2.0/aa529304(v=msdn.10)). Para obter detalhes sobre como criar um token personalizado no WCF, consulte [como: criar um token personalizado](../extending/how-to-create-a-custom-token.md).  
  
### <a name="wse-30-custom-token-manager"></a>Gerenciador de tokens personalizados do WSE 3,0  
 O modelo de programação para criar um Gerenciador de token personalizado é diferente no WCF do que no WSE 3,0. Para obter detalhes sobre como criar um Gerenciador de token personalizado e os outros componentes necessários para um token de segurança personalizado, consulte [como: criar um token personalizado](../extending/how-to-create-a-custom-token.md).  
  
> [!NOTE]
> Se você tiver criado um `UsernameToken` Gerenciador de token de segurança personalizado, o WCF fornece um mecanismo mais fácil para especificar a lógica de autenticação do que criar um Gerenciador de token de segurança personalizado. Para obter mais detalhes, consulte [como: usar um validador de senha e nome de usuário personalizado](how-to-use-a-custom-user-name-and-password-validator.md).  
  
### <a name="wse-30-web-services-that-use-mtom-encoded-soap-messages"></a>Serviços Web do WSE 3,0 que usam mensagens SOAP codificadas de MTOM  
 Como um aplicativo WSE 3, um aplicativo WCF pode especificar a codificação de mensagem MTOM na configuração. Para migrar essa configuração, adicione o [\<mtomMessageEncoding>](../../configure-apps/file-schema/wcf/mtommessageencoding.md) à associação para o serviço. O exemplo de código a seguir demonstra como a codificação MTOM é especificada no WSE 3,0 para um serviço equivalente no WCF.  
  
 **WSE 3,0**  
  
```xml  
<messaging>  
    <mtom clientMode="On"/>  
</messaging>  
```  
  
 **WCF**  
  
```xml  
<customBinding>  
  <binding name="MyBinding">  
    <mtomMessageEncoding/>  
  </binding>  
</customBinding>  
```  
  
## <a name="messaging"></a>Mensagens  
  
### <a name="wse-30-applications-that-use-the-wse-messaging-api"></a>Aplicativos WSE 3,0 que usam a API de mensagens do WSE  

 Quando a API de mensagens do WSE é usada para obter acesso direto ao XML que é comunicado entre o cliente e o serviço Web, o aplicativo pode ser convertido para usar o "XML antigo" (POX). Para obter mais detalhes sobre POX, consulte [interoperabilidade com aplicativos Pox](interoperability-with-pox-applications.md). Para obter mais detalhes sobre a API de mensagens do WSE, consulte [enviando e recebendo mensagens SOAP usando a API de mensagens do WSE](https://docs.microsoft.com/previous-versions/dotnet/netframework-2.0/aa529293(v=msdn.10)).  
  
## <a name="transports"></a>Transportes  
  
### <a name="tcp"></a>TCP  
 Por padrão, os clientes do WSE 3,0 e os serviços Web que enviam mensagens SOAP usando o transporte TCP não interoperam com clientes WCF e serviços Web. Essa incompatibilidade ocorre devido a diferenças no enquadramento usado no protocolo TCP e por motivos de desempenho. No entanto, um exemplo do WCF detalha como implementar uma sessão TCP personalizada que interopera com o WSE 3,0. Para obter detalhes sobre este exemplo, consulte [transporte: interoperabilidade TCP do WSE 3,0](../samples/transport-wse-3-0-tcp-interoperability.md).  
  
 Para especificar que um aplicativo WCF usa o transporte TCP, use o [\<netTcpBinding>](../../configure-apps/file-schema/wcf/nettcpbinding.md) .  
  
### <a name="custom-transport"></a>Transporte personalizado  
 O equivalente a um transporte personalizado do WSE 3,0 no WCF é uma extensão de canal. Para obter detalhes sobre como criar uma extensão de canal, consulte [estendendo a camada de canal](../extending/extending-the-channel-layer.md).  
  
## <a name="see-also"></a>Consulte também

- [Ciclo de vida de programação básica](../basic-programming-lifecycle.md)
- [Associações personalizadas](../extending/custom-bindings.md)
- [Como criar uma associação personalizada utilizando o SecurityBindingElement](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [Como criar um SecurityBindingElement para um modo de autenticação especificado](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
