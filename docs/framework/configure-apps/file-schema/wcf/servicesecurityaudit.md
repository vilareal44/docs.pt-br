---
title: <serviceSecurityAudit>
ms.date: 03/30/2017
ms.assetid: ba517369-a034-4f8e-a2c4-66517716062b
ms.openlocfilehash: 10888f26053014ffb1fec49d1dfe87c7fd09ab54
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399574"
---
# \<serviceSecurityAudit>
Especifica as configurações que habilitam a auditoria de eventos de segurança durante operações de serviço.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceSecurityAudit>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<serviceSecurityAudit auditLogLocation="Default/Application/Security"
                      messageAuthenticationAuditLevel="None/Success/Failure/SuccessOrFailure"
                      serviceAuthorizationAuditLevel="None/Success/Failure/SuccessOrFailure"
                      suppressAuditFailure="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|auditLogLocation|Especifica o local do log de auditoria. Os valores válidos incluem os seguintes:<br /><br /> -Padrão: os eventos de segurança são gravados no log do aplicativo no Windows XP e no log de eventos no Windows Server 2003 e no Windows Vista.<br />-Application: os eventos de auditoria são gravados no log de eventos do aplicativo.<br />-Security: os eventos de auditoria são gravados no log de eventos de segurança.<br /><br /> O valor padrão é Default. Para obter mais informações, consulte <xref:System.ServiceModel.AuditLogLocation>.|  
|suppressAuditFailure|Um valor booliano que especifica o comportamento para suprimir falhas de gravação no log de auditoria.<br /><br /> Os aplicativos devem ser notificados quanto a falhas de gravação no log de auditoria. Se seu aplicativo não for projetado para lidar com falhas de auditoria, você deverá usar esse atributo para suprimir as falhas de gravação no log de auditoria.<br /><br /> Se esse atributo for `true` , exceções diferentes de OutOfMemoryException, StackOverFlowException, ThreadAbortException e ArgumentException que resultam de tentativas de gravação de eventos de auditoria são manipuladas pelo sistema e não são propagadas para o aplicativo. Se esse atributo for `false` , todas as exceções resultantes das tentativas de gravar eventos de auditoria serão passadas para o aplicativo.<br /><br /> O padrão é `true`.|  
|serviceAuthorizationAuditLevel|Especifica os tipos de eventos de autorização que são registrados no log de auditoria. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: nenhuma auditoria de eventos de autorização de serviço é executada.<br />-Êxito: somente eventos de autorização de serviço bem-sucedidos são auditados.<br />-Falha: somente os eventos de autorização do serviço de falha são auditados.<br />-SuccessOrFailure: os eventos de autorização de serviço de êxito e falha são auditados.<br /><br /> O valor padrão é Nenhum. Para obter mais informações, consulte <xref:System.ServiceModel.AuditLevel>.|  
|messageAuthenticationAuditLevel|Especifica o tipo de eventos de auditoria de autenticação de mensagem registrados. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: nenhum evento de auditoria é gerado.<br />-Êxito: somente a segurança bem-sucedida (validação completa, incluindo validação de assinatura de mensagem, codificação e validação de token), os eventos são registrados.<br />-Failure: somente os eventos de falha são registrados.<br />-SuccessOrFailure: os eventos Success e Failure são registrados em log.<br /><br /> O valor padrão é Nenhum. Para obter mais informações, consulte <xref:System.ServiceModel.AuditLevel>.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um elemento de comportamento.|  
  
## <a name="remarks"></a>Comentários  
 Este elemento de configuração é usado para auditar eventos de autenticação Windows Communication Foundation (WCF). Quando a auditoria está habilitada, as tentativas de autenticação bem-sucedidas ou com falha (ou ambas) podem ser auditadas. Os eventos são gravados em um dos três logs de eventos: aplicativo, segurança ou o log padrão para a versão do sistema operacional. Os logs de eventos podem ser exibidos usando o Visualizador de eventos do Windows.  
  
 Para obter um exemplo detalhado de como usar esse elemento de configuração, consulte [comportamento de auditoria de serviço](../../../wcf/samples/service-auditing-behavior.md).  
  
 Por padrão, no Windows XP, os eventos de auditoria podem ser vistos no log do aplicativo; no Windows Server 2003 e no Windows Vista, os eventos de auditoria podem ser vistos no log de segurança. O local dos eventos de auditoria pode ser especificado definindo o `auditLogLocation` atributo como ' Application ' ou ' Security '. Para obter mais informações, consulte [como: auditar eventos de segurança](../../../wcf/feature-details/how-to-audit-wcf-security-events.md). Se os eventos forem gravados no log de segurança, o > LocalSecurityPolicy habilitar o acesso ao objeto deverá ser definido como "êxito" e "falha".  
  
 Ao examinar o log de eventos, a origem dos eventos de auditoria é "ServiceModel Audit 3.0.0.0". Os registros de auditoria de autenticação de mensagens têm uma categoria de "MessageAuthentication", enquanto os registros de auditoria de autorização de serviço têm uma categoria de "Service Authorização".  
  
 Os eventos de auditoria de autenticação de mensagens abordam se a mensagem foi violada, se a mensagem expirou e se o cliente pode se autenticar no serviço. Eles fornecem informações sobre se a autenticação foi bem-sucedida ou falhou junto com a identidade do cliente e o ponto de extremidade para o qual a mensagem foi enviada junto com a ação associada à mensagem.  
  
 Os eventos de auditoria de autorização de serviço abrangem a decisão de autorização feita por um Gerenciador de autorização de serviço. Eles fornecem informações sobre se a autorização foi bem-sucedida ou falhou junto com a identidade do cliente, o ponto de extremidade para o qual a mensagem foi enviada, a ação associada à mensagem, o identificador do contexto de autorização que foi gerado a partir da mensagem de entrada e o tipo do Gerenciador de autorização que fez a decisão de acesso.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<system.serviceModel>
  <behaviors>
    <serviceBehaviors>
      <behavior name="NewBehavior">
        <serviceSecurityAudit auditLogLocation="Application"
                              suppressAuditFailure="true"
                              serviceAuthorizationAuditLevel="Success"
                              messageAuthenticationAuditLevel="Success" />
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceSecurityAuditElement>
- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Auditoria](../../../wcf/feature-details/auditing-security-events.md)
- [Como fazer auditoria de eventos de segurança](../../../wcf/feature-details/how-to-audit-wcf-security-events.md)
- [Comportamento de auditoria de serviço](../../../wcf/samples/service-auditing-behavior.md)
