---
title: <windowsAuthentication> de <serviceCredentials>
ms.date: 03/30/2017
ms.assetid: e0709473-0997-4de3-8f49-783527309a48
ms.openlocfilehash: ded04f6e87fce2e12dac8f681ba2d4178f8fd204
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399108"
---
# <a name="windowsauthentication-of-servicecredentials"></a>\<windowsAuthentication> de \<serviceCredentials>
Especifica as configurações de uma credencial de serviço do Windows.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceBehaviors>**](servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-servicebehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<serviceCredentials>**](servicecredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<windowsAuthentication>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<windowsAuthentication allowAnonymousLogons="Boolean"
                       includeWindowsGroups="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`includeWindowsGroups`|Um atributo booliano opcional que especifica se o sistema inclui grupos do Windows no contexto de segurança. O padrão é `true`.<br /><br /> Definir esse atributo para `true` tem um impacto no desempenho conforme ele resulta em uma expansão de grupo completo. Defina esse atributo como `false` se você não precisar estabelecer a lista de grupos aos quais um usuário pertence.|  
|`allowAnonymousLogons`|Um atributo booliano opcional que especifica se chamadores anônimos e não autenticados são permitidos. O padrão é `false`.<br /><br /> Quando o `clientCredentialType` atributo de uma associação é definido como `Windows` , o sistema não permite chamadores anônimos. Isso significa que somente os chamadores autenticados de domínio ou grupo de trabalho têm permissão para acessar o sistema. Você pode substituir esse comportamento usando esse atributo.<br /><br /> Use essa configuração com extrema cautela.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<serviceCredentials>](servicecredentials.md)|Especifica a credencial a ser usada na autenticação do serviço e as configurações relacionadas à validação da credencial do cliente.|  
  
## <a name="remarks"></a>Comentários  
 Use este elemento para especificar se deseja permitir o acesso de usuários anônimos do Windows definindo o `allowAnonymousLogons` atributo. Você também pode especificar se deseja incluir informações de grupo às quais os usuários pertencem ao AuthorizationContext definindo o `includeWindowsGroups` atributo. Se ele for definido como `true` (a configuração padrão), o serviço poderá determinar os grupos do Windows aos quais o cliente pertence.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WindowsServiceElement>
- <xref:System.ServiceModel.Configuration.ServiceCredentialsElement.WindowsAuthentication%2A>
- <xref:System.ServiceModel.Description.ServiceCredentials.WindowsAuthentication%2A>
- <xref:System.ServiceModel.Security.WindowsServiceCredential>
