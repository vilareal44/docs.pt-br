---
title: Elemento <serviceAuthorization>
ms.date: 03/30/2017
ms.assetid: 18cddad5-ddcb-4839-a0ac-1d6f6ab783ca
ms.openlocfilehash: f476f754a340f52859be2986e42754cba0ef3771
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "71834016"
---
# <a name="serviceauthorization-element"></a>Elemento \<serviceAuthorization>

Especifica as configurações que autorizam o acesso às operações de serviço

[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<serviceAuthorization>**  

## <a name="syntax"></a>Sintaxe

```xml
<serviceAuthorization impersonateCallerForAllOperations="Boolean"
                      principalPermissionMode="None/UseWindowsGroups/UseAspNetRoles/Custom"
                      roleProviderName="String"
                      serviceAuthorizationManagerType="String">
  <authorizationPolicies>
    <add policyType="String" />
  </authorizationPolicies>
</serviceAuthorization>
```

## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai:

### <a name="attributes"></a>Atributos

|Atributo|Descrição|  
|---------------|-----------------|  
|impersonateCallerForAllOperations|Um valor booliano que especifica se todas as operações no serviço representam o chamador. O padrão é `false`.<br /><br /> Quando uma operação de serviço específica representa o chamador, o contexto do thread é alternado para o contexto do chamador antes de executar o serviço especificado.|  
|principalPermissionmode|Define a entidade de segurança usada para executar operações no servidor. Os valores incluem o seguinte:<br /><br /> -Nenhum<br />- UseWindowsGroups<br />- UseAspNetRoles<br />-Personalizado<br /><br /> O valor padrão é UseWindowsGroups. O valor é do tipo <xref:System.ServiceModel.Description.PrincipalPermissionMode> . Para obter mais informações sobre como usar esse atributo, consulte [como: restringir o acesso com a classe PrincipalPermissionAttribute](../../../wcf/how-to-restrict-access-with-the-principalpermissionattribute-class.md).|  
|roleProviderName|Uma cadeia de caracteres que especifica o nome do provedor de função, que fornece informações de função para um aplicativo Windows Communication Foundation (WCF). O padrão é uma cadeia de caracteres vazia.|  
|ServiceAuthorizationManagerType|Uma cadeia de caracteres que contém o tipo do Gerenciador de autorização de serviço. Para obter mais informações, consulte <xref:System.ServiceModel.ServiceAuthorizationManager>.|  

### <a name="child-elements"></a>Elementos filho

|Elemento|Descrição|  
|-------------|-----------------|  
|authorizationPolicies|Contém uma coleção de tipos de política de autorização, que podem ser adicionados usando a `add` palavra-chave. Cada política de autorização contém um único `policyType` atributo necessário que é uma cadeia de caracteres. O atributo especifica uma política de autorização, que permite a transformação de um conjunto de declarações de entrada em outro conjunto de declarações. Controle de acesso pode ser concedido ou negado com base nisso. Para obter mais informações, consulte <xref:System.ServiceModel.Configuration.AuthorizationPolicyTypeElement>.|  

### <a name="parent-elements"></a>Elementos pai

|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Contém uma coleção de configurações para o comportamento de um serviço.|  

## <a name="remarks"></a>Comentários

Esta seção contém elementos que afetam a autorização, os provedores de função personalizados e a representação.  
  
O `principalPermissionMode` atributo especifica os grupos de usuários a serem usados ao autorizar o uso de um método protegido. O valor padrão é `UseWindowsGroups` e especifica que os grupos do Windows, como "administradores" ou "usuários", são pesquisados em busca de uma identidade que tente acessar um recurso. Você também pode especificar `UseAspNetRoles` para usar um provedor de função personalizado configurado no \<system.web> elemento, conforme mostrado no código a seguir:

```xml
<system.web>
  <membership defaultProvider="SqlProvider"
              userIsOnlineTimeWindow="15">
    <providers>
      <clear />
      <add name="SqlProvider"
           type="System.Web.Security.SqlMembershipProvider"
           connectionStringName="SqlConn"
           applicationName="MembershipProvider"
           enablePasswordRetrieval="false"
           enablePasswordReset="false"
           requiresQuestionAndAnswer="false"
           requiresUniqueEmail="true"
           passwordFormat="Hashed" />
    </providers>
  </membership>
  <!-- Other configuration code not shown. -->
</system.web>
```
  
O código a seguir mostra o `roleProviderName` usado com o `principalPermissionMode` atributo:
  
```xml
<behaviors>
  <behavior name="ServiceBehaviour">
    <serviceAuthorization principalPermissionMode ="UseAspNetRoles"
                          roleProviderName ="SqlProvider" />
  </behavior>
  <!-- Other configuration code not shown. -->
</behaviors>
```

Para obter um exemplo detalhado de como usar esse elemento de configuração, consulte [autorizando o acesso às operações de serviço e à](../../../wcf/samples/authorizing-access-to-service-operations.md) [política de autorização](../../../wcf/samples/authorization-policy.md).
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ServiceAuthorizationElement>
- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
- [Comportamentos de segurança](../../../wcf/feature-details/security-behaviors-in-wcf.md)
- [Autorizando o acesso às operações de serviço](../../../wcf/samples/authorizing-access-to-service-operations.md)
- [Como criar gerenciador de autorização personalizado para um serviço](../../../wcf/extending/how-to-create-a-custom-authorization-manager-for-a-service.md)
- [Como restringir o acesso com a classe PrincipalPermissionAttribute](../../../wcf/how-to-restrict-access-with-the-principalpermissionattribute-class.md)
- [Política de autorização](../../../wcf/samples/authorization-policy.md)
