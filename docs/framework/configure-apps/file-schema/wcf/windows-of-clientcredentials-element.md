---
title: <windows>do <clientCredentials> elemento
ms.date: 03/30/2017
ms.assetid: 793e41c2-31ea-4159-abbc-2123bf097233
ms.openlocfilehash: 61ca99213f0b83a5af5df0184a8c1de405366288
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399124"
---
# <a name="windows-of-clientcredentials-element"></a>\<windows>do \<clientCredentials> elemento
Especifica as configurações para uma credencial do Windows a ser usada para representar o cliente.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<clientCredentials>**](clientcredentials.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<windows>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<windows allowedImpersonationLevel="Identification/Impersonation/Delegation/Anonymous/None"
         allowNtlm="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`allowedImpersonationLevel`|Define a preferência de representação que o cliente se comunica com o servidor. O modo de representação que o cliente seleciona não é imposto no servidor. Os valores válidos incluem os seguintes:<br /><br /> -Identificação: o servidor pode obter a identidade e os privilégios do cliente, mas não pode representar o cliente.<br />-Impersonation: o servidor pode representar o contexto de segurança do cliente no sistema local.<br />-Delegation: o servidor pode representar o contexto de segurança do cliente em sistemas remotos.<br />-Anônimo: o servidor não pode representar ou identificar o cliente.<br />-Nenhum: um nível de representação não é atribuído.<br /><br /> O padrão é identificação. Esse atributo é do tipo <xref:System.Security.Principal.TokenImpersonationLevel> .|  
|`allowNtlm`|Definir essa propriedade como `true` permite que a autenticação faça downgrade para NTLM se o Kerberos não estiver disponível.<br /><br /> Definir essa propriedade como `false` faz com que o Windows Communication Foundation (WCF) faça um melhor esforço para gerar uma exceção se o NTLM for usado. Observe que a definição dessa propriedade como `false` pode não impedir que credenciais NTLM sejam enviadas pela conexão.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCredentials>](clientcredentials.md)|Especifica as credenciais usadas para autenticar o cliente para o serviço.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WindowsClientElement>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Configuration.ClientCredentialsElement.Windows%2A>
- <xref:System.ServiceModel.Description.ClientCredentials>
- <xref:System.ServiceModel.Description.ClientCredentials.Windows%2A>
- <xref:System.ServiceModel.Security.WindowsClientCredential>
- [Protegendo clientes](../../../wcf/securing-clients.md)
- [Trabalhando com certificados](../../../wcf/feature-details/working-with-certificates.md)
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
