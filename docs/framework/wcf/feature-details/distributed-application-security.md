---
title: Segurança de aplicativos distribuídos
ms.date: 03/30/2017
helpviewer_keywords:
- distributed application security [WCF]
- security [WCF], transfer
ms.assetid: 53928a10-e474-46d0-ab90-5f98f8d7b668
ms.openlocfilehash: 3cae20cfe8d52497646ca173740533a22326c8f8
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599134"
---
# <a name="distributed-application-security"></a>Segurança de aplicativos distribuídos
A segurança do Windows Communication Foundation (WCF) é dividida em três áreas funcionais principais: segurança de transferência, controle de acesso e auditoria. A segurança de transferência fornece integridade, confidencialidade e autenticação. A segurança de transferência é fornecida por um dos seguintes: segurança de transporte, segurança de mensagem ou `TransportWithMessageCredential` .  
  
 Para obter uma visão geral da segurança de mensagens do WCF, consulte [visão geral de segurança](security-overview.md). Para obter mais informações sobre as outras duas partes da segurança do WCF, consulte [autorização](authorization-in-wcf.md) e [auditoria](auditing-security-events.md).  
  
## <a name="transfer-security-scenarios"></a>Cenários de segurança de transferência  
 Os cenários comuns que empregam a segurança de transferência do WCF incluem o seguinte:  
  
- Transferência segura usando o Windows. Um cliente e um serviço WCF são implantados em um domínio do Windows (ou uma floresta do Windows). As mensagens contêm dados pessoais, de modo que os requisitos incluem a autenticação mútua do cliente e do serviço, a integridade da mensagem e a confidencialidade da mensagem. Além disso, é necessária uma prova de que uma determinada transação ocorreu, por exemplo, o destinatário da mensagem deve registrar as informações de assinatura.  
  
- Transferência segura usando o `UserName` e o HTTPS. Um cliente e um serviço WCF precisam ser desenvolvidos para funcionar pela Internet. As credenciais do cliente são autenticadas em um banco de dados de pares de nome de usuário/senha. O serviço é implantado em um endereço HTTPS usando um certificado de protocolo SSL confiável (SSL). Como as mensagens viajam pela Internet, o cliente e o serviço precisam ser autenticados mutuamente e a confidencialidade e a integridade das mensagens devem ser preservadas durante a transferência.  
  
- Transferência segura usando certificados. Um cliente e um serviço WCF precisam ser desenvolvidos para funcionar na Internet pública. O cliente e o serviço têm certificados que podem ser usados para proteger as mensagens. O cliente e o serviço usam a Internet para se comunicar entre si e para executar transações de alto valor que exigem integridade de mensagem, confidencialidade e autenticação mútua.  
  
## <a name="integrity-confidentiality-and-authentication"></a>Integridade, confidencialidade e autenticação  
 Três funções — integridade, confidencialidade e autenticação – estão juntas chamadas de segurança de transferência. A segurança de transferência fornece as funções que ajudam a reduzir as ameaças a um aplicativo distribuído. A tabela a seguir descreve brevemente as três funções que compõem a segurança da transferência.  
  
|Função|Descrição|  
|--------------|-----------------|  
|Integridade|A *integridade* é a garantia de que os dados estão completos e precisos, especialmente depois que eles passam de um ponto para outro e possivelmente foram lidos por muitos atores. A integridade deve ser mantida para evitar a violação dos dados e normalmente é obtida pela assinatura digital de uma mensagem.|  
|Confidencialidade|A *confidencialidade* é a garantia de que uma mensagem não foi lida por ninguém além do leitor pretendido. Por exemplo, um número de cartão de crédito deve ser mantido confidencial à medida que é enviado pela Internet. A confidencialidade é geralmente fornecida pela criptografia de dados usando um esquema de chave pública/privada.|  
|Autenticação|A *autenticação* é a verificação de uma identidade solicitada. Por exemplo, ao usar uma conta bancária, é imperativo que apenas o proprietário real da conta tenha permissão para retirar fundos. A autenticação pode ser fornecida por uma variedade de meios. Um método comum é o sistema de usuário/senha. Um segundo é o uso de um certificado X. 509 que é fornecido por terceiros.|  
  
## <a name="security-modes"></a>Modos de segurança  
 O WCF tem vários modos de segurança de transferência, que são descritos na tabela a seguir.  
  
|Mode|Descrição|  
|----------|-----------------|  
|Nenhum|Nenhuma segurança é fornecida na camada de transporte ou na camada de mensagem. Nenhuma das associações predefinidas usa esse modo por padrão, exceto o [\<basicHttpBinding>](../../configure-apps/file-schema/wcf/basichttpbinding.md) elemento ou, ao usar o código, a <xref:System.ServiceModel.BasicHttpBinding> classe.|  
|Transport|Usa um transporte seguro, como HTTPS para integridade, confidencialidade e autenticação mútua.|  
|Mensagem|Usa a segurança de mensagem SOAP para integridade, confidencialidade e autenticação mútua. As mensagens SOAP são protegidas de acordo com os padrões WS-Security.|  
|Modo Misto|Usa segurança de transporte para integridade, confidencialidade e autenticação de servidor. Usa segurança de mensagem (WS-Security e outros padrões) para autenticação de cliente.<br /><br /> (Essa enumeração para esse modo é `TransportWithMessageCredential` .)|  
|Ambos|Executa a proteção e a autenticação em ambos os níveis. Esse modo está disponível somente no [\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md) elemento.|  
  
## <a name="credentials-and-transfer-security"></a>Credenciais e segurança de transferência  
 Uma *credencial* são dados que são apresentados para estabelecer uma identidade ou recursos reivindicados. A apresentação de uma credencial envolve a apresentação dos dados e a prova de posse dos dados. O WCF dá suporte a uma variedade de tipos de credencial nos níveis de segurança de transporte e de mensagem. Você pode especificar um tipo de credencial para uma associação WCF.  
  
 Em muitos países ou regiões, a licença de um driver é um exemplo de uma credencial. Uma licença contém dados que representam a identidade e os recursos de uma. Ele contém a prova de posse na forma da imagem do possessor. A licença é emitida por uma autoridade confiável, geralmente um departamento de licenciamento governamental. A licença é selada e pode conter um holograma, mostrando que ela não foi violada ou falsificada.  
  
 Por exemplo, considere dois tipos de credenciais com suporte no WCF: nome de usuário e credenciais de certificado (X. 509).  
  
 Para a credencial de nome de usuário, o nome de usuário representa a identidade solicitada e a senha apresenta a prova de posse. Nesse caso, a autoridade confiável é o sistema que valida o nome de usuário e a senha.  
  
 Na credencial do certificado, o nome da entidade, o nome alternativo da entidade ou campos específicos dentro do certificado podem ser usados para representar a identidade e/ou os recursos solicitados. A prova de posse dos dados na credencial é estabelecida usando a chave privada associada para gerar uma assinatura.  
  
 Para obter mais informações sobre como programar a segurança de transferência e especificar credenciais, consulte [associações e](bindings-and-security.md) comportamentos de segurança e [segurança](security-behaviors-in-wcf.md).  
  
### <a name="transport-client-credential-types"></a>Tipos de credencial do cliente de transporte  
 A tabela a seguir mostra os possíveis valores usados ao criar um aplicativo que usa a segurança de transferência. Você pode usar esses valores no código ou nas configurações de associação.  
  
|Setting|Descrição|  
|-------------|-----------------|  
|Nenhum|Especifica que o cliente não precisa apresentar nenhuma credencial. Isso se traduz em um cliente anônimo.|  
|Basic|Especifica autenticação básica. Para obter mais informações, consulte RFC2617, "[autenticação http: autenticação básica e resumida](http://schemas.xmlsoap.org/ws/2004/10/discovery/ws-discovery.pdf)".|  
|Digest|Especifica a autenticação Digest. Para obter mais informações, consulte RFC2617, "[autenticação http: autenticação básica e resumida](http://schemas.xmlsoap.org/ws/2004/10/discovery/ws-discovery.pdf)".|  
|Ntlm|Especifica a autenticação do Windows usando a negociação SSPI em um domínio do Windows.<br /><br /> A negociação SSPI resulta no uso do protocolo Kerberos ou NT LanMan (NTLM).|  
|Windows|Especifica a autenticação do Windows usando SSPI em um domínio do Windows. O SSPI escolhe o protocolo Kerberos ou NTLM como serviço de autenticação.<br /><br /> SSPI tenta o protocolo Kerberos primeiro; Se isso falhar, ele usará NTLM.|  
|Certificado|Executa a autenticação de cliente usando um certificado, normalmente X. 509.|  
  
### <a name="message-client-credential-types"></a>Tipos de credencial do cliente da mensagem  
 A tabela a seguir mostra os possíveis valores usados ao criar um aplicativo que usa a segurança de mensagem. Você pode usar esses valores no código ou nas configurações de associação.  
  
|Setting|Descrição|  
|-------------|-----------------|  
|Nenhum|Permite que o serviço interaja com clientes anônimos.|  
|Windows|Permite que as trocas de mensagens SOAP ocorram sob o contexto autenticado de uma credencial do Windows. Usa o mecanismo de negociação SSPI para escolher o protocolo Kerberos ou NTLM como um serviço de autenticação.|  
|Nome de Usuário|Permite que o serviço exija que o cliente seja autenticado com uma credencial de nome de usuário. Observe que o WCF não permite nenhuma operação criptográfica com o nome de usuário, como gerar uma assinatura ou criptografar dados. Dessa forma, o WCF impõe que o transporte seja protegido ao usar credenciais de nome de usuário.|  
|Certificado|Permite que o serviço exija que o cliente seja autenticado usando um certificado.|  
|CardSpace|Permite que o serviço exija que o cliente seja autenticado usando um CardSpace.|  
  
### <a name="programming-credentials"></a>Credenciais de programação  
 Para cada um dos tipos de credencial do cliente, o modelo de programação do WCF permite que você especifique os valores de credencial e os validadores de credenciais usando comportamentos de serviço e comportamentos de canal.  
  
 A segurança do WCF tem dois tipos de credenciais: comportamentos de credencial de serviço e comportamentos de credencial de canal. Os comportamentos de credencial no WCF especificam os dados reais, ou seja, as credenciais usadas para atender aos requisitos de segurança expressos por meio de associações. No WCF, uma classe de cliente é o componente de tempo de execução que converte entre a invocação de operação e as mensagens. Todos os clientes herdam da <xref:System.ServiceModel.ClientBase%601> classe. A <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> Propriedade na classe base permite que você especifique vários valores de credenciais de cliente.  
  
 No WCF, os comportamentos de serviço são atributos aplicados à classe que implementa um contrato de serviço (interface) para controlar o serviço programaticamente. A <xref:System.ServiceModel.Description.ServiceCredentials> classe permite que você especifique certificados para credenciais de serviço e configurações de validação de cliente para vários tipos de credencial de cliente.  
  
### <a name="negotiation-model-for-message-security"></a>Modelo de negociação para segurança de mensagem  
 O modo de segurança de mensagem permite que você execute a segurança de transferência para que a credencial de serviço seja configurada no cliente fora da banda. Por exemplo, se você estiver usando um certificado armazenado no repositório de certificados do Windows, deverá usar uma ferramenta como um snap-in do MMC (console de gerenciamento Microsoft).  
  
 O modo de segurança de mensagem também permite que você execute a segurança de transferência para que a credencial de serviço seja trocada pelo cliente como parte de uma negociação inicial. Para habilitar a negociação, defina a <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> propriedade como `true` .  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de criação de ponto de extremidade](../endpoint-creation-overview.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
- [Visão geral de segurança](security-overview.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
