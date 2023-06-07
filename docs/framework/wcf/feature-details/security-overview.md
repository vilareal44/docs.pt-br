---
title: Visão geral de segurança
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Communication Foundation, security
- WCF, security
ms.assetid: f478c80d-792d-4e7a-96bd-a2ff0b6f65f9
ms.openlocfilehash: 517d80395e09598fcbd067034223dc6ba58cbe2e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600432"
---
# <a name="windows-communication-foundation-security-overview"></a>Visão geral da segurança do Windows Communication Foundation
O Windows Communication Foundation (WCF) é uma plataforma de programação distribuída baseada em mensagem SOAP, e a proteção de mensagens entre clientes e serviços é essencial para proteger os dados. O WCF fornece uma plataforma versátil e interoperável para trocar mensagens seguras com base na infraestrutura de segurança existente e nos padrões de segurança reconhecidos para mensagens SOAP.  
  
> [!NOTE]
> Para obter um guia abrangente sobre a segurança do WCF, consulte [diretrizes de segurança do WCF](https://archive.codeplex.com/?p=WCFSecurity).  
  
 O WCF usa conceitos que são familiares se você tiver criado aplicativos seguros e distribuídos com tecnologias existentes, como HTTPS, segurança integrada do Windows ou nomes de usuário e senhas para autenticar usuários. O WCF não só se integra a infraestruturas de segurança existentes, mas também estende a segurança distribuída além dos domínios somente do Windows usando mensagens SOAP seguras. Considere o WCF uma implementação de mecanismos de segurança existentes com a principal vantagem de usar o SOAP como o protocolo, além dos protocolos existentes. Por exemplo, as credenciais que identificam um cliente ou um serviço, como nome de usuário e senha ou certificados X. 509, têm perfis SOAP baseados em XML interoperáveis. Usando esses perfis, as mensagens são trocadas com segurança, tirando proveito de especificações abertas, como assinaturas digitais XML e criptografia XML. Para obter uma lista de especificações, consulte [protocolos de serviços Web com suporte em associações de interoperabilidade fornecidas pelo sistema](web-services-protocols-supported-by-system-provided-interoperability-bindings.md).  
  
 Outro paralelo é o Component Object Model (COM) na plataforma Windows, que permite aplicativos distribuídos e seguros. O COM tem um mecanismo de segurança abrangente no qual o contexto de segurança pode ser fluído entre componentes; Esse mecanismo impõe a integridade, a confidencialidade e a autenticação. No entanto, o COM não permite a comunicação entre plataformas e mensagens seguras, como o WCF. Usando o WCF, você pode criar serviços e clientes que se estendem de domínios do Windows pela Internet. As mensagens interoperáveis do WCF são essenciais para a criação de serviços dinâmicos orientados a negócios que o ajudarão a se sentir confiante na segurança de suas informações.  
  
## <a name="windows-communication-foundation-security-benefits"></a>Windows Communication Foundation benefícios de segurança  
 O WCF é uma plataforma de programação distribuída baseada em mensagens SOAP. Usando o WCF, você pode criar aplicativos que funcionam como serviços e clientes de serviço, criando e processando mensagens de um número ilimitado de outros serviços e clientes. Nesse aplicativo distribuído, as mensagens podem fluir de nó para nó, por meio de firewalls, para a Internet e por meio de vários intermediários SOAP. Isso introduz uma variedade de ameaças à segurança de mensagens. Os exemplos a seguir ilustram algumas ameaças comuns que a segurança do WCF pode ajudar a mitigar ao trocar mensagens entre entidades:  
  
- Observação de tráfego de rede para obter informações confidenciais. Por exemplo, em um cenário de serviços bancários online, um cliente solicita a transferência de fundos de uma conta para outra. Um usuário mal-intencionado intercepta a mensagem e, com o número da conta e a senha, posteriormente realiza uma transferência de fundos da conta comprometida.  
  
- Entidades não autorizadas atuando como serviços sem conscientização do cliente. Nesse cenário, um usuário mal-intencionado (o invasor) atua como um serviço online e intercepta mensagens do cliente para obter informações confidenciais. Em seguida, o invasor usa os dados roubados para transferir fundos da conta comprometida. Esse ataque também é conhecido como um *ataque de phishing*.  
  
- Alteração de mensagens para obter um resultado diferente do que o chamador pretendido. Por exemplo, alterar o número da conta para o qual um depósito é feito permite que os fundos vá para uma conta não autorizada.  
  
- Reproduções de hackers em que um hacker incômodo repete a mesma ordem de compra. Por exemplo, uma livraria online recebe centenas de pedidos e envia os livros para um cliente que os não os solicitou.  
  
- Incapacidade de um serviço de autenticar um cliente. Nesse caso, o serviço não pode garantir que a pessoa apropriada realizou a transação.  
  
 Em resumo, a segurança de transferência fornece as seguintes garantias:  
  
- Autenticação do ponto de extremidade de serviço (entrevistado).  
  
- Autenticação da entidade de cliente (iniciador).  
  
- Integridade da mensagem.  
  
- Confidencialidade da mensagem.  
  
- Detecção de reprodução.  
  
### <a name="integration-with-existing-security-infrastructures"></a>Integração com infraestruturas de segurança existentes  
 Muitas vezes, as implantações de serviço Web têm soluções de segurança existentes em vigor, por exemplo, protocolo SSL (SSL) ou o protocolo Kerberos. Alguns aproveitam uma infraestrutura de segurança que já foi implantada, como domínios do Windows usando Active Directory. Geralmente, é necessário integrar-se com essas tecnologias existentes ao avaliar e adotar as mais recentes.  
  
 A segurança do WCF integra-se aos modelos de segurança de transporte existentes e pode aproveitar a infraestrutura existente para modelos de segurança de transferência mais recentes com base na segurança de mensagem SOAP.  
  
### <a name="integration-with-existing-authentication-models"></a>Integração com modelos de autenticação existentes  
 Uma parte importante de qualquer modelo de segurança de comunicação é a capacidade de identificar e autenticar entidades na comunicação. Essas entidades na comunicação usam "identidades digitais" ou credenciais, para se autenticarem com os pares de comunicação. À medida que as plataformas de comunicação distribuídas evoluíram, foram implementadas várias autenticações de credenciais e modelos de segurança relacionados. Por exemplo, na Internet, o uso de um nome de usuário e senha para identificar os usuários é comum. Na intranet, o uso de um controlador de domínio Kerberos para fazer backup da autenticação de usuário e serviço está se tornando comum. Em determinados cenários, como entre dois parceiros comerciais, os certificados podem ser usados para autenticar mutuamente os parceiros.  
  
 Portanto, no mundo dos serviços Web, em que o mesmo serviço pode ser exposto a clientes corporativos internos, bem como a parceiros externos ou clientes da Internet, é importante que a infraestrutura forneça integração com esses modelos de autenticação de segurança existentes. A segurança do WCF dá suporte a uma ampla variedade de tipos de credencial (modelos de autenticação), incluindo:  
  
- Chamador anônimo.  
  
- Credencial do cliente do nome de usuário.  
  
- Credencial do cliente do certificado.  
  
- Windows (protocolo Kerberos e NT LanMan [NTLM]).  
  
### <a name="standards-and-interoperability"></a>Padrões e interoperabilidade  
 Em um mundo com grandes implantações existentes, a homogeneidade é rara. Plataformas de computação/comunicações distribuídas precisam interoperar com a oferta de tecnologias diferentes de fornecedores. Da mesma forma, a segurança também deve ser interoperável.  
  
 Para habilitar sistemas de segurança interoperáveis, as empresas ativas no setor de serviços da Web criaram uma variedade de padrões. Especificamente em relação à segurança, alguns padrões notáveis foram propostos: WS-Security: segurança de mensagem SOAP (aceita pelo corpo de padrões OASIS e, anteriormente conhecido como WS-Security), WS-Trust, WS-SecureConversation e WS-SecurityPolicy.  
  
 O WCF dá suporte a uma ampla variedade de cenários de interoperabilidade. A <xref:System.ServiceModel.BasicHttpBinding> classe é destinada ao perfil de segurança básico (BSP) e a <xref:System.ServiceModel.WSHttpBinding> classe é direcionada aos padrões de segurança mais recentes, como WS-Security 1,1 e WS-SecureConversation. Ao aderir a esses padrões, a segurança do WCF pode interoperar e integrar os serviços da Web que são hospedados em sistemas operacionais e plataformas diferentes do Microsoft Windows.  
  
## <a name="wcf-security-functional-areas"></a>Áreas funcionais de segurança do WCF  
 A segurança do WCF é dividida em três áreas funcionais: segurança de transferência, controle de acesso e auditoria. As seções a seguir discutem brevemente essas áreas e fornecem links para obter mais informações.  
  
### <a name="transfer-security"></a>Segurança de transferência  
 A segurança de transferência abrange três funções de segurança principais: integridade, confidencialidade e autenticação. A *integridade* é a capacidade de detectar se uma mensagem foi violada. A *confidencialidade* é a capacidade de manter uma mensagem ilegível por qualquer outra pessoa que não seja o destinatário pretendido; Isso é obtido por meio de criptografia. A *autenticação* é a capacidade de verificar uma identidade solicitada. Juntas, essas três funções ajudam a garantir que as mensagens cheguem com segurança de um ponto para outro.  
  
#### <a name="transport-and-message-security-modes"></a>Modos de segurança de transporte e mensagem  
 Dois mecanismos principais são usados para implementar a segurança de transferência no WCF: modo de segurança de *transporte* e modo de segurança de *mensagem* .  
  
- O *modo de segurança de transporte* usa um protocolo de nível de transporte, como https, para obter segurança de transferência. O modo de transporte tem a vantagem de ser amplamente adotada, disponível em várias plataformas e menos computacionalmente complexa. No entanto, ele tem a desvantagem de proteger mensagens somente do ponto a ponto.  
  
- Por outro lado, o *modo de segurança de mensagem*usa o WS-Security (e outras especificações) para implementar a segurança de transferência. Como a segurança da mensagem é aplicada diretamente às mensagens SOAP e está contida nos envelopes SOAP, juntamente com os dados do aplicativo, ele tem a vantagem de ser de transporte independente de protocolo, mais extensível e garantir a segurança de ponta a ponta (em comparação com o ponto a ponto); Ela tem a desvantagem de ser várias vezes mais lenta do que o modo de segurança de transporte, pois precisa lidar com a natureza XML das mensagens SOAP.  
  
 Para obter mais informações sobre essas diferenças, consulte [protegendo serviços e clientes](securing-services-and-clients.md).  
  
 Um terceiro modo de segurança usa ambos os modos anteriores e traz as vantagens de ambos. Esse modo é chamado `TransportWithMessageCredential` . Nesse modo, a segurança da mensagem é usada para autenticar o cliente e a segurança de transporte é usada para autenticar o servidor e fornecer confidencialidade e integridade da mensagem. Graças a isso, o `TransportWithMessageCredential` modo de segurança é quase tão rápido quanto o modo de segurança de transporte e fornece extensibilidade de autenticação de cliente da mesma maneira que a segurança de mensagem. No entanto, ao contrário do modo de segurança da mensagem, ele não fornece segurança completa de ponta a ponta.  
  
### <a name="access-control"></a>Controle de acesso  
 O *controle de acesso* também é conhecido como autorização. A *autorização* permite que usuários diferentes tenham privilégios diferentes para exibir dados. Por exemplo, como os arquivos de recursos humanos de uma empresa contêm dados confidenciais de funcionários, somente os gerentes têm permissão para exibir dados de funcionários. Além disso, os gerentes podem exibir apenas os dados para seus subordinados diretos. Nesse caso, o controle de acesso é baseado na função ("gerente"), bem como na identidade específica do gerente (para impedir que um gerente examine os registros de funcionário de outro gerente).  
  
 No WCF, os recursos de controle de acesso são fornecidos por meio da integração com o Common Language Runtime (CLR) <xref:System.Security.Permissions.PrincipalPermissionAttribute> e por meio de um conjunto de APIs conhecido como *modelo de identidade*. Para obter detalhes sobre o controle de acesso e autorização baseada em declarações, consulte [estendendo a segurança](../extending/extending-security.md).  
  
### <a name="auditing"></a>Auditoria  
 A *auditoria* é o log de eventos de segurança no log de eventos do Windows. Você pode registrar eventos relacionados à segurança, como falhas de autenticação (ou êxitos). Para obter mais informações, consulte [auditoria](auditing-security-events.md). Para obter detalhes de programação, consulte [como: auditar eventos de segurança](how-to-audit-wcf-security-events.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Security.Permissions.PrincipalPermissionAttribute>
- [Protegendo serviços](../securing-services.md)
- [Cenários comuns de segurança](common-security-scenarios.md)
- [Associações e segurança](bindings-and-security.md)
- [Protegendo serviços e clientes](securing-services-and-clients.md)
- [Autenticação](authentication-in-wcf.md)
- [Autorização](authorization-in-wcf.md)
- [Federação e tokens emitidos](federation-and-issued-tokens.md)
- [Auditoria](auditing-security-events.md)
- [Diretrizes e práticas recomendadas de segurança](security-guidance-and-best-practices.md)
- [Configurando serviços usando arquivos de configuração](../configuring-services-using-configuration-files.md)
- [Associações fornecidas pelo sistema](../system-provided-bindings.md)
- [Visão geral de criação de ponto de extremidade](../endpoint-creation-overview.md)
- [Estendendo a segurança](../extending/extending-security.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
