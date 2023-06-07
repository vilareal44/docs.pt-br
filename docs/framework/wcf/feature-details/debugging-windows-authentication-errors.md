---
title: Depurando erros de autenticação do Windows
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, authentication
- WCF, Windows authentication
ms.assetid: 181be4bd-79b1-4a66-aee2-931887a6d7cc
ms.openlocfilehash: eb3274b98234324bd47aa456feb4845da5a7f3a9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599276"
---
# <a name="debug-windows-authentication-errors"></a>Depurar erros de autenticação do Windows

Ao usar a autenticação do Windows como um mecanismo de segurança, a interface de provedor de suporte de segurança (SSPI) lida com processos de segurança. Quando ocorrem erros de segurança na camada SSPI, eles são exibidos por Windows Communication Foundation (WCF). Este tópico fornece uma estrutura e um conjunto de perguntas para ajudar a diagnosticar os erros.  
  
 Para obter uma visão geral do protocolo Kerberos, consulte [Kerberos explicou](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-2000-server/bb742516(v=technet.10)); para obter uma visão geral do SSPI, consulte [SSPI](/windows/win32/secauthn/sspi).  
  
 Para a autenticação do Windows, o WCF normalmente usa o SSP ( *Negotiate* Security Support Provider), que executa a autenticação mútua do Kerberos entre o cliente e o serviço. Se o protocolo Kerberos não estiver disponível, por padrão, o WCF retornará ao NT LAN Manager (NTLM). No entanto, você pode configurar o WCF para usar apenas o protocolo Kerberos (e para gerar uma exceção se o Kerberos não estiver disponível). Você também pode configurar o WCF para usar formulários restritos do protocolo Kerberos.  
  
## <a name="debugging-methodology"></a>Metodologia de depuração  
 O método básico é o seguinte:  
  
1. Determine se você está usando a autenticação do Windows. Se você estiver usando qualquer outro esquema, este tópico não se aplicará.  
  
2. Se você tiver certeza de que está usando a autenticação do Windows, determine se a configuração do WCF usa Kerberos Direct ou Negotiate.  
  
3. Depois de determinar se sua configuração está usando o protocolo Kerberos ou NTLM, você pode entender as mensagens de erro no contexto correto.  
  
### <a name="availability-of-the-kerberos-protocol-and-ntlm"></a>Disponibilidade do protocolo Kerberos e NTLM  
 O SSP do Kerberos requer um controlador de domínio para atuar como o centro de distribuição de chaves do Kerberos (KDC). O protocolo Kerberos está disponível somente quando o cliente e o serviço estão usando identidades de domínio. Em outras combinações de conta, o NTLM é usado, conforme resumido na tabela a seguir.  
  
 Os cabeçalhos de tabela mostram possíveis tipos de conta usados pelo servidor. A coluna à esquerda mostra os possíveis tipos de conta usados pelo cliente.  
  
||Usuário local|Sistema Local|Usuário de domínio|Computador de domínio|  
|-|----------------|------------------|-----------------|--------------------|  
|Usuário local|NTLM|NTLM|NTLM|NTLM|  
|Sistema Local|NTLM anônima|NTLM anônima|NTLM anônima|NTLM anônima|  
|Usuário de domínio|NTLM|NTLM|Kerberos|Kerberos|  
|Computador de domínio|NTLM|NTLM|Kerberos|Kerberos|  
  
 Especificamente, os quatro tipos de conta incluem:  
  
- Usuário local: perfil de usuário somente máquina. Por exemplo: `MachineName\Administrator` ou `MachineName\ProfileName`.  
  
- Sistema local: o sistema de conta interno em um computador que não está ingressado em um domínio.  
  
- Usuário de domínio: uma conta de usuário em um domínio do Windows. Por exemplo: `DomainName\ProfileName`.  
  
- Computador de domínio: um processo com a identidade do computador em execução em um computador ingressado em um domínio do Windows. Por exemplo: `MachineName\Network Service`.  
  
> [!NOTE]
> A credencial de serviço é capturada quando o <xref:System.ServiceModel.ICommunicationObject.Open%2A> método da <xref:System.ServiceModel.ServiceHost> classe é chamado. A credencial do cliente é lida sempre que o cliente envia uma mensagem.  
  
## <a name="common-windows-authentication-problems"></a>Problemas comuns de autenticação do Windows  
 Esta seção aborda alguns problemas comuns de autenticação do Windows e possíveis soluções.  
  
### <a name="kerberos-protocol"></a>Protocolo Kerberos  
  
#### <a name="spnupn-problems-with-the-kerberos-protocol"></a>Problemas de SPN/UPN com o protocolo Kerberos  
 Ao usar a autenticação do Windows e o protocolo Kerberos é usado ou negociado por SSPI, a URL usada pelo ponto de extremidade do cliente deve incluir o nome de domínio totalmente qualificado do host do serviço dentro da URL do serviço. Isso pressupõe que a conta sob a qual o serviço está sendo executado tenha acesso à chave SPN (nome da entidade de serviço) do computador que é criada quando o computador é adicionado ao domínio de Active Directory, que é mais comumente feito pela execução do serviço na conta de serviço de rede. Se o serviço não tiver acesso à chave de SPN do computador, você deverá fornecer o SPN ou nome UPN correto da conta sob a qual o serviço está sendo executado na identidade do ponto de extremidade do cliente. Para obter mais informações sobre como o WCF funciona com o SPN e o UPN, consulte [identidade de serviço e autenticação](service-identity-and-authentication.md).  
  
 Em cenários de balanceamento de carga, como Web farms ou ambientes Web, uma prática comum é definir uma conta exclusiva para cada aplicativo, atribuir um SPN a essa conta e garantir que todos os serviços do aplicativo sejam executados nessa conta.  
  
 Para obter um SPN para a conta de seu serviço, você precisa ser um administrador de domínio Active Directory. Para obter mais informações, consulte [complemento técnico do Kerberos para Windows](https://docs.microsoft.com/previous-versions/msp-n-p/ff649429(v=pandp.10)).  
  
#### <a name="kerberos-protocol-direct-requires-the-service-to-run-under-a-domain-machine-account"></a>O protocolo Kerberos direto requer que o serviço seja executado em uma conta de computador de domínio  
 Isso ocorre quando a `ClientCredentialType` propriedade é definida como `Windows` e a <xref:System.ServiceModel.MessageSecurityOverHttp.NegotiateServiceCredential%2A> propriedade é definida como `false` , conforme mostrado no código a seguir.  
  
 [!code-csharp[C_DebuggingWindowsAuth#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#1)]
 [!code-vb[C_DebuggingWindowsAuth#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#1)]  
  
 Para corrigir, execute o serviço usando uma conta de computador de domínio, como serviço de rede, em um computador ingressado no domínio.  
  
### <a name="delegation-requires-credential-negotiation"></a>A delegação requer negociação de credencial  
 Para usar o protocolo de autenticação Kerberos com delegação, você deve implementar o protocolo Kerberos com a negociação de credencial (às vezes chamada de "multilocação" ou "multietapa" Kerberos). Se você implementar a autenticação Kerberos sem negociação de credencial (às vezes chamada de Kerberos "One-shot" ou "single-perna"), uma exceção será lançada.  
  
 Para implementar o Kerberos com a negociação de credencial, execute as seguintes etapas:  
  
1. Implemente a delegação definindo <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> como <xref:System.Security.Principal.TokenImpersonationLevel.Delegation> .  
  
2. Exigir negociação SSPI:  
  
    1. Se você estiver usando associações padrão, defina a `NegotiateServiceCredential` propriedade como `true` .  
  
    2. Se você estiver usando associações personalizadas, defina o `AuthenticationMode` atributo do `Security` elemento como `SspiNegotiated` .  
  
3. Exigir que a negociação SSPI use o Kerberos ao não permitir o uso do NTLM:  
  
    1. Faça isso no código, com a seguinte instrução:`ChannelFactory.Credentials.Windows.AllowNtlm = false`  
  
    2. Ou você pode fazer isso no arquivo de configuração definindo o `allowNtlm` atributo como `false` . Esse atributo está contido no [\<windows>](../../configure-apps/file-schema/wcf/windows-of-clientcredentials-element.md) .  
  
### <a name="ntlm-protocol"></a>Protocolo NTLM  
  
#### <a name="negotiate-ssp-falls-back-to-ntlm-but-ntlm-is-disabled"></a>Negociar o SSP volta para o NTLM, mas o NTLM está desabilitado  
 A <xref:System.ServiceModel.Security.WindowsClientCredential.AllowNtlm%2A> propriedade é definida como `false` , o que faz com que Windows Communication Foundation (WCF) faça um melhor esforço para gerar uma exceção se o NTLM for usado. Definir essa propriedade como `false` não pode impedir que credenciais NTLM sejam enviadas pela conexão.  
  
 O seguinte mostra como desabilitar o fallback para NTLM.  
  
 [!code-csharp[C_DebuggingWindowsAuth#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#4)]
 [!code-vb[C_DebuggingWindowsAuth#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#4)]  
  
#### <a name="ntlm-logon-fails"></a>Falha no logon NTLM  
 As credenciais do cliente não são válidas no serviço. Verifique se o nome de usuário e a senha estão definidos corretamente e correspondem a uma conta que é conhecida pelo computador em que o serviço está sendo executado. O NTLM usa as credenciais especificadas para fazer logon no computador do serviço. Embora as credenciais possam ser válidas no computador em que o cliente está em execução, esse logon falhará se as credenciais não forem válidas no computador do serviço.  
  
#### <a name="anonymous-ntlm-logon-occurs-but-anonymous-logons-are-disabled-by-default"></a>O logon anônimo do NTLM ocorre, mas logons anônimos são desabilitados por padrão  
 Ao criar um cliente, a <xref:System.ServiceModel.Security.WindowsClientCredential.AllowedImpersonationLevel%2A> propriedade é definida <xref:System.Security.Principal.TokenImpersonationLevel.Anonymous> como, conforme mostrado no exemplo a seguir, mas, por padrão, o servidor não permite logons anônimos. Isso ocorre porque o valor padrão da <xref:System.ServiceModel.Security.WindowsServiceCredential.AllowAnonymousLogons%2A> propriedade da <xref:System.ServiceModel.Security.WindowsServiceCredential> classe é `false` .  
  
 O seguinte código de cliente tenta habilitar logons anônimos (Observe que a propriedade padrão é `Identification` ).  
  
 [!code-csharp[C_DebuggingWindowsAuth#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#5)]
 [!code-vb[C_DebuggingWindowsAuth#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#5)]  
  
 O código de serviço a seguir altera o padrão para habilitar logons anônimos pelo servidor.  
  
 [!code-csharp[C_DebuggingWindowsAuth#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#6)]
 [!code-vb[C_DebuggingWindowsAuth#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#6)]  
  
 Para obter mais informações sobre representação, consulte [delegação e representação](delegation-and-impersonation-with-wcf.md).  
  
 Como alternativa, o cliente está sendo executado como um serviço do Windows, usando o sistema de conta interno.  
  
### <a name="other-problems"></a>Outros Problemas  
  
#### <a name="client-credentials-are-not-set-correctly"></a>As credenciais do cliente não estão definidas corretamente  
 A autenticação do Windows usa a <xref:System.ServiceModel.Security.WindowsClientCredential> instância retornada pela <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> propriedade da <xref:System.ServiceModel.ClientBase%601> classe, não o <xref:System.ServiceModel.Security.UserNamePasswordClientCredential> . Veja a seguir um exemplo incorreto.  
  
 [!code-csharp[C_DebuggingWindowsAuth#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#2)]
 [!code-vb[C_DebuggingWindowsAuth#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#2)]  
  
 O exemplo a seguir mostra o correto.  
  
 [!code-csharp[C_DebuggingWindowsAuth#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_debuggingwindowsauth/cs/source.cs#3)]
 [!code-vb[C_DebuggingWindowsAuth#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_debuggingwindowsauth/vb/source.vb#3)]  
  
#### <a name="sspi-is-not-available"></a>SSPI não está disponível  
 Os seguintes sistemas operacionais não dão suporte à autenticação do Windows quando usados como um servidor: Windows XP Home Edition, Windows XP Media Center Edition e Windows Vista Home edições.  
  
#### <a name="developing-and-deploying-with-different-identities"></a>Desenvolvendo e implantando com identidades diferentes  
 Se você desenvolver seu aplicativo em um computador e implantá-lo em outro e usar tipos de conta diferentes para autenticar em cada computador, poderá ocorrer um comportamento diferente. Por exemplo, suponha que você desenvolva seu aplicativo em um computador Windows XP Pro usando o `SSPI Negotiated` modo de autenticação. Se você usar uma conta de usuário local para autenticar com o, o protocolo NTLM será usado. Depois que o aplicativo é desenvolvido, você implanta o serviço em um computador com Windows Server 2003 em que ele é executado em uma conta de domínio. Neste ponto, o cliente não poderá autenticar o serviço, pois ele usará o Kerberos e um controlador de domínio.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Security.WindowsClientCredential>
- <xref:System.ServiceModel.Security.WindowsServiceCredential>
- <xref:System.ServiceModel.Security.WindowsClientCredential>
- <xref:System.ServiceModel.ClientBase%601>
- [Delegação e representação](delegation-and-impersonation-with-wcf.md)
- [Cenários sem suporte](unsupported-scenarios.md)
