---
title: Como fazer auditoria de eventos de segurança do Windows Communication Foundation
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security [WCF], auditing events
ms.assetid: e71e9587-3336-46a2-9a9e-d72a1743ecec
ms.openlocfilehash: 186dd4a7fc2beae848e5cbd167a204352ee6ed4e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601290"
---
# <a name="how-to-audit-windows-communication-foundation-security-events"></a>Como fazer auditoria de eventos de segurança do Windows Communication Foundation
O Windows Communication Foundation (WCF) permite que você registre eventos de segurança no log de eventos do Windows, que pode ser exibido usando o Visualizador de Eventos do Windows. Este tópico explica como configurar um aplicativo para que ele registre eventos de segurança. Para obter mais informações sobre a auditoria do WCF, consulte [auditoria](auditing-security-events.md).  
  
### <a name="to-audit-security-events-in-code"></a>Para auditar eventos de segurança no código  
  
1. Especifique o local do log de auditoria. Para fazer isso, defina a <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.AuditLogLocation%2A> propriedade da <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> classe como um dos valores de <xref:System.ServiceModel.AuditLogLocation> enumeração, conforme mostrado no código a seguir.  
  
     [!code-csharp[AuditingSecurityEvents#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#2)]
     [!code-vb[AuditingSecurityEvents#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#2)]  
  
     A <xref:System.ServiceModel.AuditLogLocation> enumeração tem três valores: `Application` , `Security` ou `Default` . O valor especifica um dos logs visíveis no Visualizador de Eventos, o log de segurança ou o log do aplicativo. Se você usar o `Default` valor, o log real dependerá do sistema operacional em que o aplicativo está sendo executado. Se a auditoria estiver habilitada e o local do log não for especificado, o padrão será o `Security` log para plataformas que dão suporte à gravação no log de segurança; caso contrário, ele gravará no `Application` log. Somente o Windows Server 2003 e o Windows Vista dão suporte à gravação no log de segurança por padrão.  
  
2. Configure os tipos de eventos para auditar. Você pode auditar simultaneamente eventos de nível de serviço ou eventos de autorização no nível de mensagem. Para fazer isso, defina a <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.ServiceAuthorizationAuditLevel%2A> propriedade ou a <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.MessageAuthenticationAuditLevel%2A> propriedade como um dos <xref:System.ServiceModel.AuditLevel> valores de enumeração, conforme mostrado no código a seguir.  
  
     [!code-csharp[AuditingSecurityEvents#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#3)]
     [!code-vb[AuditingSecurityEvents#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#3)]  
  
3. Especifique se deseja suprimir ou expor falhas ao aplicativo em relação aos eventos de auditoria de log. Defina a <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> propriedade como `true` ou `false` , conforme mostrado no código a seguir.  
  
     [!code-csharp[AuditingSecurityEvents#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#4)]
     [!code-vb[AuditingSecurityEvents#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#4)]  
  
     A `SuppressAuditFailure` propriedade padrão é `true` , de modo que a falha na auditoria não afete o aplicativo. Caso contrário, uma exceção será gerada. Para qualquer auditoria bem-sucedida, um rastreamento detalhado é gravado. Para qualquer falha na auditoria, o rastreamento é gravado no nível de erro.  
  
4. Exclua o existente <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> da coleção de comportamentos encontrados na descrição de a <xref:System.ServiceModel.ServiceHost> . A coleção de comportamento é acessada pela <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> propriedade, que, por sua vez, é acessada da <xref:System.ServiceModel.ServiceHostBase.Description%2A> propriedade. Em seguida, adicione o novo <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> à mesma coleção, conforme mostrado no código a seguir.  
  
     [!code-csharp[AuditingSecurityEvents#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#5)]
     [!code-vb[AuditingSecurityEvents#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#5)]  
  
### <a name="to-set-up-auditing-in-configuration"></a>Para configurar a auditoria na configuração  
  
1. Para configurar a auditoria na configuração, adicione um [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) elemento à [\<behaviors>](../../configure-apps/file-schema/wcf/behaviors.md) seção do arquivo Web. config. Em seguida, adicione um [\<serviceSecurityAudit>](../../configure-apps/file-schema/wcf/servicesecurityaudit.md) elemento e defina os vários atributos, conforme mostrado no exemplo a seguir.  
  
    ```xml  
    <behaviors>  
       <behavior name="myAuditBehavior">  
          <serviceSecurityAudit auditLogLocation="Application"  
                suppressAuditFailure="false"
                serviceAuthorizationAuditLevel="None"
                messageAuthenticationAuditLevel="SuccessOrFailure" />  
          </behavior>  
    </behaviors>  
    ```  
  
2. Você deve especificar o comportamento para o serviço, conforme mostrado no exemplo a seguir.  
  
    ```xml  
    <services>  
        <service type="WCS.Samples.Service.Echo"
        behaviorConfiguration=" myAuditBehavior">  
           <endpoint address=""  
                    binding="wsHttpBinding"  
                    bindingConfiguration="CertificateDefault"
                    contract="WCS.Samples.Service.IEcho" />  
        </service>  
    </services>  
    ```  
  
## <a name="example"></a>Exemplo  
 O código a seguir cria uma instância da <xref:System.ServiceModel.ServiceHost> classe e adiciona uma nova <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior> à sua coleção de comportamentos.  
  
 [!code-csharp[AuditingSecurityEvents#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/auditingsecurityevents/cs/auditingsecurityevents.cs#1)]
 [!code-vb[AuditingSecurityEvents#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/auditingsecurityevents/vb/auditingsecurityevents.vb#1)]  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Definir a <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.SuppressAuditFailure%2A> propriedade como `true` , suprime qualquer falha ao gerar auditorias de segurança (se definido como `false` , uma exceção é lançada). No entanto, se você habilitar a seguinte propriedade de **configuração de segurança local** do Windows, uma falha ao gerar eventos de auditoria fará com que o Windows seja desligado imediatamente:  
  
 **Auditoria: Desligar o sistema imediatamente se não for possível registrar auditorias de segurança**  
  
 Para definir a propriedade, abra a caixa de diálogo **configurações de segurança local** . Em **configurações de segurança**, clique em **políticas locais**. Em seguida, clique em **Opções de segurança**.  
  
 Se a <xref:System.ServiceModel.AuditLogLocation> propriedade for definida como <xref:System.ServiceModel.AuditLogLocation.Security> e a **auditoria de acesso a objeto** não estiver definida na política de **segurança local**, os eventos de auditoria não serão gravados no log de segurança. Observe que nenhuma falha é retornada, mas as entradas de auditoria não são gravadas no log de segurança.  
  
## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior.AuditLogLocation%2A>
- <xref:System.ServiceModel.Description.ServiceSecurityAuditBehavior>
- <xref:System.ServiceModel.AuditLogLocation>
- [Auditoria](auditing-security-events.md)
