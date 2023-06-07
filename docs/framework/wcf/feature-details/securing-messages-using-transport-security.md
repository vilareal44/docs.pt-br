---
title: Mensagens de segurança que usam a segurança de transporte
ms.date: 03/30/2017
ms.assetid: 9029771a-097e-448a-a13a-55d2878330b8
ms.openlocfilehash: 7d160f6f0d1d29e34ca3365501b86d1a736de67b
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84589936"
---
# <a name="securing-messages-using-transport-security"></a>Mensagens de segurança que usam a segurança de transporte
Esta seção discute a segurança de transporte do MSMQ (enfileiramento de mensagens) que você pode usar para proteger mensagens enviadas a uma fila.  
  
> [!NOTE]
> Antes de ler este tópico, é recomendável que você leia os [conceitos de segurança](security-concepts.md).  
  
 A ilustração a seguir fornece um modelo conceitual de comunicação em fila usando o Windows Communication Foundation (WCF). Esta ilustração e terminologia são usadas para explicar os conceitos de segurança de transporte.  
  
 ![Diagrama de aplicativos enfileirados](media/distributed-queue-figure.jpg "Distributed-Queue-figura")  
  
 Ao enviar mensagens em fila usando o WCF com <xref:System.ServiceModel.NetMsmqBinding> o, a mensagem WCF é anexada como um corpo da mensagem MSMQ. A segurança de transporte protege toda a mensagem do MSMQ (cabeçalhos ou propriedades de mensagens MSMQ e o corpo da mensagem). Como é o corpo da mensagem do MSMQ, usar a segurança de transporte também protege a mensagem do WCF.  
  
 O principal conceito por trás da segurança de transporte é que o cliente precisa atender aos requisitos de segurança para obter a mensagem para a fila de destino. Isso é diferente da segurança da mensagem, em que a mensagem é protegida para o aplicativo que recebe a mensagem.  
  
 Segurança de transporte usando <xref:System.ServiceModel.NetMsmqBinding> e <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> impacta como as mensagens MSMQ são protegidas em trânsito entre a fila de transmissão e a fila de destino onde o seguro implica:  
  
- Assinar a mensagem para garantir que ela não seja violada.  
  
- Criptografar a mensagem para garantir que ela não possa ser vista ou adulterada. Isso é recomendado, mas opcional.  
  
- O Gerenciador de fila de destino que identifica o remetente da mensagem para não-repúdio.  
  
 No MSMQ, independentemente da autenticação, a fila de destino tem uma ACL (lista de controle de acesso) para verificar se o cliente tem permissão para enviar a mensagem para a fila de destino. O aplicativo receptor também é verificado quanto à permissão para receber a mensagem da fila de destino.  
  
## <a name="wcf-msmq-transport-security-properties"></a>Propriedades de segurança de transporte MSMQ do WCF  
 O MSMQ usa a segurança do Windows para autenticação. Ele usa o SID (identificador de segurança do Windows) para identificar o cliente e usa Active Directory serviço de diretório como a autoridade de certificação ao autenticar o cliente. Isso requer que o MSMQ seja instalado com a integração do Active Directory. Como o SID de domínio do Windows é usado para identificar o cliente, essa opção de segurança só é significativa quando o cliente e o serviço fazem parte do mesmo domínio do Windows.  
  
 O MSMQ também fornece a capacidade de anexar um certificado com a mensagem que não está registrada com Active Directory. Nesse caso, ele garante que a mensagem foi assinada usando o certificado anexado.  
  
 O WCF fornece essas duas opções como parte da segurança de transporte MSMQ e é o pivô principal para segurança de transporte.  
  
 Por padrão, a segurança de transporte é ativada.  
  
 Considerando esses conceitos básicos, as seções a seguir detalham as propriedades de segurança de transporte agrupadas com o <xref:System.ServiceModel.NetMsmqBinding> e o <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> .  
  
#### <a name="msmq-authentication-mode"></a>Modo de autenticação do MSMQ  
 O <xref:System.ServiceModel.MsmqTransportSecurity.MsmqAuthenticationMode%2A> determina se a segurança de domínio do Windows ou uma segurança externa baseada em certificado deve ser usada para proteger a mensagem. Nos dois modos de autenticação, o canal de transporte na fila do WCF usa o `CertificateValidationMode` especificado na configuração do serviço. O modo de validação de certificado especifica o mecanismo usado para verificar a validade do certificado.  
  
 Quando a segurança de transporte é ativada, a configuração padrão é <xref:System.ServiceModel.MsmqAuthenticationMode.WindowsDomain> .  
  
#### <a name="windows-domain-authentication-mode"></a>Modo de autenticação de domínio do Windows  
 A escolha de usar a segurança do Windows requer a integração de Active Directory. <xref:System.ServiceModel.MsmqAuthenticationMode.WindowsDomain>é o modo de segurança de transporte padrão. Quando isso é definido, o canal do WCF anexa o SID do Windows à mensagem do MSMQ e usa seu certificado interno obtido do Active Directory. O MSMQ usa esse certificado interno para proteger a mensagem. O Gerenciador de filas de recebimento usa Active Directory para pesquisar e localizar um certificado correspondente para autenticar o cliente e verifica se o SID também corresponde ao do cliente. Essa etapa de autenticação é executada se um certificado, gerado internamente no caso do `WindowsDomain` modo de autenticação ou gerado externamente no caso do `Certificate` modo de autenticação, é anexado à mensagem mesmo que a fila de destino não esteja marcada como exigindo autenticação.  
  
> [!NOTE]
> Ao criar uma fila, você pode marcar a fila como uma fila autenticada para indicar que a fila requer autenticação do cliente enviando mensagens para a fila. Isso garante que nenhuma mensagem não autenticada seja aceita na fila.  
  
 O SID anexado à mensagem também é usado para verificar a ACL da fila de destino para garantir que o cliente tenha autoridade para enviar mensagens para a fila.  
  
#### <a name="certificate-authentication-mode"></a>Modo de autenticação de certificado  
 A opção de usar o modo de autenticação de certificado não requer integração de Active Directory. Na verdade, em alguns casos, como quando o MSMQ é instalado no modo de grupo de trabalho (sem integração com o Active Directory) ou ao usar o protocolo de transferência SRMP (protocolo SOAP de mensagens confiáveis) para enviar mensagens para a fila, o só <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> funciona.  
  
 Ao enviar uma mensagem WCF com <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> , o canal WCF não anexa um SID do Windows à mensagem MSMQ. Dessa forma, a ACL da fila de destino deve permitir que o `Anonymous` acesso do usuário envie para a fila. O Gerenciador de filas de recebimento verifica se a mensagem MSMQ foi assinada com o certificado, mas não executa nenhuma autenticação.  
  
 O certificado com suas informações de declarações e identidades é populado no <xref:System.ServiceModel.ServiceSecurityContext> pelo canal de transporte enfileirado do WCF. O serviço pode usar essas informações para executar sua própria autenticação do remetente.  
  
### <a name="msmq-protection-level"></a>Nível de proteção MSMQ  
 O nível de proteção determina como proteger a mensagem do MSMQ para garantir que ela não seja violada. Ele é especificado na <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> propriedade. O valor padrão é <xref:System.Net.Security.ProtectionLevel.Sign>.  
  
#### <a name="sign-protection-level"></a>Assinar nível de proteção  
 A mensagem MSMQ é assinada usando o certificado gerado internamente ao usar o `WindowsDomain` modo de autenticação ou um certificado gerado externamente ao usar o `Certificate` modo de autenticação.  
  
#### <a name="sign-and-encrypt-protection-level"></a>Assinar e criptografar o nível de proteção  
 A mensagem MSMQ é assinada usando o certificado gerado internamente ao usar o `WindowsDomain` modo de autenticação ou o certificado gerado externamente ao usar o `Certificate` modo de autenticação.  
  
 Além de assinar a mensagem, a mensagem MSMQ é criptografada usando a chave pública do certificado obtido de Active Directory que pertence ao Gerenciador de fila de recebimento que hospeda a fila de destino. O Gerenciador de filas de envio garante que a mensagem MSMQ seja criptografada em trânsito. O Gerenciador de filas de recebimento descriptografa a mensagem MSMQ usando a chave privada de seu certificado interno e armazena a mensagem na fila (se autenticada e autorizada) em texto não criptografado.  
  
> [!NOTE]
> Para criptografar a mensagem, Active Directory acesso é necessário ( `UseActiveDirectory` a propriedade de <xref:System.ServiceModel.NetMsmqBinding> deve ser definida como `true` ) e pode ser usada com <xref:System.ServiceModel.MsmqAuthenticationMode.Certificate> and <xref:System.ServiceModel.MsmqAuthenticationMode.WindowsDomain> .  
  
#### <a name="none-protection-level"></a>Nenhum nível de proteção  
 Isso é implícito quando <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> é definido como <xref:System.Net.Security.ProtectionLevel.None> . Este não pode ser um valor válido para outros modos de autenticação.  
  
> [!NOTE]
> Se a mensagem MSMQ for assinada, o MSMQ verificará se a mensagem está assinada com o certificado anexado (interno ou externo) independentemente do estado da fila, ou seja, da fila autenticada ou não.  
  
### <a name="msmq-encryption-algorithm"></a>Algoritmo de criptografia MSMQ  
 O algoritmo de criptografia especifica o algoritmo a ser usado para criptografar a mensagem do MSMQ na conexão. Essa propriedade será usada somente se <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> for definido como <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> .  
  
 Os algoritmos com suporte são `RC4Stream` e `AES` e o padrão é `RC4Stream` .  
  
 Você poderá usar o `AES` algoritmo somente se o remetente tiver o MSMQ 4,0 instalado. Além disso, a fila de destino também deve ser hospedada no MSMQ 4,0.  
  
### <a name="msmq-hash-algorithm"></a>Algoritmo de hash MSMQ  
 O algoritmo de hash especifica o algoritmo usado para criar uma assinatura digital da mensagem MSMQ. O Gerenciador de filas de recebimento usa esse mesmo algoritmo para autenticar a mensagem MSMQ. Essa propriedade será usada somente se <xref:System.ServiceModel.MsmqTransportSecurity.MsmqProtectionLevel%2A> for definido como <xref:System.Net.Security.ProtectionLevel.Sign> ou <xref:System.Net.Security.ProtectionLevel.EncryptAndSign> .  
  
 Os algoritmos com suporte são `MD5`, `SHA1`, `SHA256` e `SHA512`. O padrão é `SHA1`.

 Devido a problemas de colisão com MD5/SHA1, a Microsoft recomenda SHA256 ou melhor.
  
## <a name="see-also"></a>Consulte também

- [Visão geral de filas](queues-overview.md)
- [Conceitos de segurança](security-concepts.md)
- [Protegendo serviços e clientes](securing-services-and-clients.md)
