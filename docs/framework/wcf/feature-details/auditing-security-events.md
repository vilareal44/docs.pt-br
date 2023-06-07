---
title: Auditoria de eventos de segurança
ms.date: 03/30/2017
helpviewer_keywords:
- auditing security events [WCF]
ms.assetid: 5633f61c-a3c9-40dd-8070-1c373b66a716
ms.openlocfilehash: b130ed57ba086535122c8c8795c42863348870d0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597651"
---
# <a name="auditing-security-events"></a>Auditoria de eventos de segurança
Os aplicativos criados com o Windows Communication Foundation (WCF) podem registrar eventos de segurança (êxito, falha ou ambos) com o recurso de auditoria. Os eventos são gravados no log de eventos do sistema do Windows e podem ser examinados usando o Visualizador de Eventos.  
  
 A auditoria fornece uma maneira para um administrador detectar um ataque que já ocorreu ou está em andamento. Além disso, a auditoria pode ajudar um desenvolvedor a depurar problemas relacionados à segurança. Por exemplo, se um erro na configuração da política de autorização ou verificação acidentalmente negar acesso a um usuário autorizado, um desenvolvedor poderá descobrir e isolar rapidamente a causa desse erro examinando o log de eventos.  
  
 Para obter mais informações sobre a segurança do WCF, consulte [visão geral de segurança](security-overview.md). Para obter mais informações sobre programação do WCF, consulte [programação básica do WCF](../basic-wcf-programming.md).  
  
## <a name="audit-level-and-behavior"></a>Nível de auditoria e comportamento  
 Existem dois níveis de auditorias de segurança:  
  
- Nível de autorização de serviço, no qual um chamador é autorizado.  
  
- Nível de mensagem, no qual o WCF verifica a validade da mensagem e autentica o chamador.  
  
 Você pode verificar os dois níveis de auditoria para êxito ou falha, que é conhecido como o *comportamento de auditoria*.  
  
## <a name="audit-log-location"></a>Local do log de auditoria  
 Depois de determinar um nível de auditoria e um comportamento, você (ou um administrador) pode especificar um local para o log de auditoria. As três opções incluem: padrão, aplicativo e segurança. Quando você especifica Default, o log real depende de qual sistema você está usando e se o sistema dá suporte à gravação no log de segurança. Para obter mais informações, consulte a seção "sistema operacional" mais adiante neste tópico.  
  
 Para gravar no log de segurança, é necessário o `SeAuditPrivilege` . Por padrão, somente as contas do sistema local e do serviço de rede têm esse privilégio. Para gerenciar as funções de log de segurança `read` e `delete` requer o `SeSecurityPrivilege` . Por padrão, somente os administradores têm esse privilégio.  
  
 Por outro lado, os usuários autenticados podem ler e gravar no log do aplicativo. Por padrão, o Windows XP grava eventos de auditoria no log do aplicativo. O log também pode conter informações pessoais que são visíveis para todos os usuários autenticados.  
  
## <a name="suppressing-audit-failures"></a>Suprimindo falhas de auditoria  
 Outra opção durante a auditoria é a possibilidade de suprimir qualquer falha de auditoria. Por padrão, uma falha de auditoria não afeta um aplicativo. Se necessário, no entanto, você pode definir a opção como `false` , o que faz com que uma exceção seja gerada.  
  
## <a name="programming-auditing"></a>Auditoria de programação  
 Você pode especificar o comportamento de auditoria de forma programática ou por meio da configuração.  
  
### <a name="auditing-classes"></a>Classes de auditoria  
 A tabela a seguir descreve as classes e as propriedades usadas para programar o comportamento de auditoria.  
  
|Classe|Descrição|  
|-----------|-----------------|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>|Habilita a configuração de opções de auditoria como um comportamento de serviço.|  
|<xref:System.ServiceModel.AuditLogLocation>|Enumeração para especificar em qual log gravar. Os valores possíveis são padrão, aplicativo e segurança. Quando você seleciona padrão, o sistema operacional determina o local real do log. Consulte a seção "opção de log de eventos de aplicativo ou de segurança" mais adiante neste tópico.|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.MessageAuthenticationAuditLevel%2A>|Especifica quais tipos de eventos de autenticação de mensagem são auditados no nível de mensagem. As opções são `None` , `Failure` , `Success` e `SuccessOrFailure` .|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.ServiceAuthorizationAuditLevel%2A>|Especifica quais tipos de eventos de autorização de serviço são auditados no nível de serviço. As opções são `None` , `Failure` , `Success` e `SuccessOrFailure` .|  
|<xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A>|Especifica o que acontece com a solicitação do cliente quando a auditoria falha. Por exemplo, quando o serviço tenta gravar no log de segurança, mas não tem `SeAuditPrivilege` . O valor padrão `true` indica que as falhas são ignoradas e a solicitação do cliente é processada normalmente.|  
  
 Para obter um exemplo de como configurar um aplicativo para registrar eventos de auditoria, consulte [como: auditar eventos de segurança](how-to-audit-wcf-security-events.md).  
  
### <a name="configuration"></a>Configuração  
 Você também pode usar a configuração para especificar o comportamento de auditoria adicionando um [\<serviceSecurityAudit>](../../configure-apps/file-schema/wcf/servicesecurityaudit.md) sob o [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) . Você deve adicionar o elemento em um [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) conforme mostrado no código a seguir.  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <behavior>  
        <!-- auditLogLocation="Application" or "Security" -->  
        <serviceSecurityAudit  
                  auditLogLocation="Application"  
                  suppressAuditFailure="true"  
                  serviceAuthorizationAuditLevel="Failure"  
                  messageAuthenticationAuditLevel="SuccessOrFailure" />
      </behavior>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
 Se a auditoria estiver habilitada e um `auditLogLocation` não for especificado, o nome de log padrão será o log de "segurança" para a plataforma que dá suporte à gravação no log de segurança; caso contrário, será o log de "aplicativo". Somente os sistemas operacionais Windows Server 2003 e Windows Vista dão suporte à gravação no log de segurança. Para obter mais informações, consulte a seção "sistema operacional" mais adiante neste tópico.  
  
## <a name="security-considerations"></a>Considerações de segurança  
 Se um usuário mal-intencionado sabe que a auditoria está habilitada, esse invasor pode enviar mensagens inválidas que fazem com que as entradas de auditoria sejam gravadas. Se o log de auditoria for preenchido dessa maneira, o sistema de auditoria falhará. Para atenuar isso, defina a <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> propriedade como `true` e use as propriedades do Visualizador de eventos para controlar o comportamento de auditoria.  
  
 Eventos de auditoria que são gravados no log do aplicativo no Windows XP são visíveis para qualquer usuário autenticado.  
  
## <a name="choosing-between-application-and-security-event-logs"></a>Escolhendo entre logs de eventos de segurança e de aplicativo  
 As tabelas a seguir fornecem informações para ajudá-lo a escolher entre fazer logon no aplicativo ou no log de eventos de segurança.  
  
#### <a name="operating-system"></a>Sistema operacional  
  
|Sistema|Log do aplicativo|Log de segurança|  
|------------|---------------------|------------------|  
|Windows XP SP2 ou posterior|Com suporte|Sem suporte|  
|Windows Server 2003 SP1 e Windows Vista|Com suporte|O contexto do thread deve possuir`SeAuditPrivilege`|  
  
#### <a name="other-factors"></a>Outros fatores  
 Além do sistema operacional, a tabela a seguir descreve outras configurações que controlam a habilitação do registro em log.  
  
|Fator|Log do aplicativo|Log de segurança|  
|------------|---------------------|------------------|  
|Gerenciamento de política de auditoria|Não aplicável.|Juntamente com a configuração, o log de segurança também é controlado pela política da autoridade de segurança local (LSA). A categoria "acesso ao objeto de auditoria" também deve ser habilitada.|  
|Experiência do usuário padrão|Todos os usuários autenticados podem gravar no log do aplicativo, portanto, nenhuma etapa de permissão adicional é necessária para processos de aplicativo.|O processo (contexto) do aplicativo deve ter `SeAuditPrivilege` .|  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- <xref:System.ServiceModel.AuditLogLocation>
- [Visão geral de segurança](security-overview.md)
- [Programação básica do WCF](../basic-wcf-programming.md)
- [Como fazer auditoria de eventos de segurança](how-to-audit-wcf-security-events.md)
- [\<serviceSecurityAudit>](../../configure-apps/file-schema/wcf/servicesecurityaudit.md)
- [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md)
- [Modelo de segurança para o Windows Server app Fabric](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
