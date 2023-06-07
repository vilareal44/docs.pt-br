---
title: Como criar uma associação personalizada utilizando o SecurityBindingElement
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], creating custom bindings
ms.assetid: 203a9f9e-3a73-427c-87aa-721c56265b29
ms.openlocfilehash: 15fdd50b05bd2217cb9819373cd1c015da52b15b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599003"
---
# <a name="how-to-create-a-custom-binding-using-the-securitybindingelement"></a>Como criar uma associação personalizada utilizando o SecurityBindingElement
O Windows Communication Foundation (WCF) inclui várias associações fornecidas pelo sistema que podem ser configuradas, mas não fornecem flexibilidade total ao configurar todas as opções de segurança às quais o WCF dá suporte. Este tópico demonstra como criar uma associação personalizada diretamente de elementos de associação individuais e realça algumas das configurações de segurança que podem ser especificadas ao criar uma ligação desse tipo. Para obter mais informações sobre como criar associações personalizadas, consulte [estendendo associações](../extending/extending-bindings.md).  
  
> [!WARNING]
> <xref:System.ServiceModel.Channels.SecurityBindingElement>o não oferece suporte à <xref:System.ServiceModel.Channels.IDuplexSessionChannel> forma de canal, que é a forma de canal padrão usada pelo transporte TCP quando <xref:System.ServiceModel.TransferMode> é definido como <xref:System.ServiceModel.TransferMode.Buffered> . Você deve definir <xref:System.ServiceModel.TransferMode> para para <xref:System.ServiceModel.TransferMode.Streamed> usar neste <xref:System.ServiceModel.Channels.SecurityBindingElement> cenário.  
  
## <a name="creating-a-custom-binding"></a>Criando uma associação personalizada  
 No WCF, todas as associações são feitas de *elementos de associação*. Cada elemento de ligação deriva da <xref:System.ServiceModel.Channels.BindingElement> classe. Para as associações padrão fornecidas pelo sistema, os elementos de associação são criados e configurados para você, embora você possa personalizar algumas das configurações de propriedade.  
  
 Por outro lado, para criar uma associação personalizada, os elementos de associação são criados e configurados e um <xref:System.ServiceModel.Channels.CustomBinding> é criado a partir dos elementos de associação.  
  
 Para fazer isso, você adiciona os elementos de associação individuais a uma coleção representada por uma instância da <xref:System.ServiceModel.Channels.BindingElementCollection> classe e, em seguida, define a `Elements` propriedade de `CustomBinding` igual a esse objeto. Você deve adicionar os elementos de associação na seguinte ordem: fluxo de transações, sessão confiável, segurança, composto duplex, unidirecional, segurança de fluxo, codificação de mensagens e transporte. Observe que nem todos os elementos de associação listados são necessários em cada associação.  
  
## <a name="securitybindingelement"></a>SecurityBindingElement  
 Três elementos de ligação estão relacionados à segurança de nível de mensagem, que derivam da <xref:System.ServiceModel.Channels.SecurityBindingElement> classe. Os três são <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> , <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> e <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> . O <xref:System.ServiceModel.Channels.TransportSecurityBindingElement> é usado para fornecer segurança de modo misto. Os outros dois elementos são usados quando a camada de mensagem fornece segurança.  
  
 Classes adicionais são usadas quando a segurança de nível de transporte é fornecida:  
  
- <xref:System.ServiceModel.Channels.HttpsTransportBindingElement>  
  
- <xref:System.ServiceModel.Channels.SslStreamSecurityBindingElement>  
  
- <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>  
  
## <a name="required-binding-elements"></a>Elementos de associação necessários  
 Há um grande número de elementos de ligação possíveis que podem ser combinados em uma associação. Nem todas essas combinações são válidas. Esta seção descreve os elementos necessários que devem estar presentes em uma associação de segurança.  
  
 As associações de segurança válidas dependem de vários fatores, incluindo o seguinte:  
  
- Modo de segurança.  
  
- Protocolo de transporte.  
  
- O padrão de troca de mensagens (MEP) especificado no contrato.  
  
 A tabela a seguir mostra as configurações de pilha de elemento de associação válidas para cada combinação dos fatores anteriores. Observe que esses são os requisitos mínimos. Você pode adicionar elementos de ligação adicionais à associação, como elementos de ligação de codificação de mensagens, elementos de associação de transação e outros elementos de ligação.  
  
|Modo de segurança|Transport|Padrão de troca de mensagens de contrato|Padrão de troca de mensagens de contrato|Padrão de troca de mensagens de contrato|  
|-------------------|---------------|---------------------------------------|---------------------------------------|---------------------------------------|  
|||`Datagram`|`Request Reply`|`Duplex`|  
|Transport|Https||||  
|||OneWayBindingElement|||  
|||HttpsTransportBindingElement|HttpsTransportBindingElement||  
||TCP||||  
|||OneWayBindingElement|||  
|||SSL ou Windows StreamSecurityBindingElement|SSL ou Windows StreamSecurityBindingElement|SSL ou Windows StreamSecurityBindingElement|  
|||TcpTransportBindingElement|TcpTransportBindingElement|TcpTransportBindingElement|  
|Mensagem|Http|SymmetricSecurityBindingElement|SymmetricSecurityBindingElement|SymmetricSecurityBindingElement (modo de autenticação = SecureConversation)|  
|||||CompositeDuplexBindingElement|  
|||OneWayBindingElement||OneWayBindingElement|  
|||HttpTransportBindingElement|HttpTransportBindingElement|HttpTransportBindingElement|  
||Tcp|SecurityBindingElement|SecurityBindingElement|SymmetricSecurityBindingElement (modo de autenticação = SecureConversation)|  
|||TcpTransportBindingElement|TcpTransportBindingElement|TcpTransportBindingElement|  
|Misto (transporte com credenciais de mensagem)|Https|TransportSecurityBindingElement|TransportSecurityBindingElement||  
|||OneWayBindingElement|||  
|||HttpsTransportBindingElement|HttpsTransportBindingElement||  
||TCP|TransportSecurityBindingElement|SymmetricSecurityBindingElement (modo de autenticação = SecureConversation)|SymmetricSecurityBindingElement (modo de autenticação = SecureConversation)|  
|||OneWayBindingElement|||  
|||SSL ou Windows StreamSecurityBindingElement|SSL ou Windows StreamSecurityBindingElement|SSL ou Windows StreamSecurityBindingElement|  
|||TcpTransportBindingElement|TcpTransportBindingElement|TcpTransportBindingElement|  
  
 Observe que há muitas configurações configuráveis nos SecurityBindingElements. Para obter mais informações, consulte [modos de autenticação SecurityBindingElement](securitybindingelement-authentication-modes.md).  
  
 Para obter mais informações, consulte [conversas seguras e sessões seguras](secure-conversations-and-secure-sessions.md).  
  
## <a name="procedures"></a>Procedimentos  
  
#### <a name="to-create-a-custom-binding-that-uses-a-symmetricsecuritybindingelement"></a>Para criar uma associação personalizada que usa um SymmetricSecurityBindingElement  
  
1. Crie uma instância da <xref:System.ServiceModel.Channels.BindingElementCollection> classe com o nome `outputBec` .  
  
2. Chame o método estático `M:System.ServiceModel.Channels.SecurityBindingElement.CreateSspiNegotiationBindingElement(true)` , que retorna uma instância da <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> classe.  
  
3. Adicione o <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> à coleção ( `outputBec` ) chamando o `Add` método da <xref:System.Collections.ObjectModel.Collection%601> <xref:System.ServiceModel.Channels.BindingElement> classe.  
  
4. Crie uma instância da <xref:System.ServiceModel.Channels.TextMessageEncodingBindingElement> classe e adicione-a à coleção ( `outputBec` ). Isso especifica a codificação usada pela associação.  
  
5. Crie um <xref:System.ServiceModel.Channels.HttpTransportBindingElement> e adicione-o à coleção ( `outputBec` ). Isso especifica que a associação usa o transporte HTTP.  
  
6. Crie uma nova associação personalizada criando uma instância da <xref:System.ServiceModel.Channels.CustomBinding> classe e passando a coleção `outputBec` para o construtor.  
  
7. A associação personalizada resultante compartilha muitas das mesmas características que o padrão <xref:System.ServiceModel.WSHttpBinding> . Ele especifica segurança em nível de mensagem e credenciais do Windows, mas desabilita sessões seguras, requer que a credencial de serviço seja especificada fora de banda e não criptografe assinaturas. O último pode ser controlado apenas definindo a <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.MessageProtectionOrder%2A> propriedade, conforme mostrado na etapa 4. Os outros dois podem ser controlados usando as configurações na associação padrão.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir fornece uma função completa para criar uma associação personalizada que usa um <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> .  
  
### <a name="code"></a>Código  
 [!code-csharp[c_CustomBinding#20](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_custombinding/cs/c_custombinding.cs#20)]
 [!code-vb[c_CustomBinding#20](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_custombinding/vb/source.vb#20)]  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Channels.SecurityBindingElement>
- <xref:System.ServiceModel.Channels.TransportSecurityBindingElement>
- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Estendendo associações](../extending/extending-bindings.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
