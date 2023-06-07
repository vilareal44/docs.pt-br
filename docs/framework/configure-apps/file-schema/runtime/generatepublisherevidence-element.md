---
title: Elemento <generatePublisherEvidence>
ms.date: 03/30/2017
helpviewer_keywords:
- generatePublisherEvidence element
- <generatePublisherEvidence> element
ms.assetid: 7d208f50-e8d5-4a42-bc1a-1cf3590706a8
ms.openlocfilehash: c2ba4a7244b7849e28eac38fb34a2cdd0d1f1048
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "81645353"
---
# <a name="generatepublisherevidence-element"></a>Elemento \<generatePublisherEvidence>
Especifica se o tempo de execução cria <xref:System.Security.Policy.Publisher> evidências para a CAS (segurança de acesso do código).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<runtime>**](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<generatePublisherEvidence>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<generatePublisherEvidence
   enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`enabled`|Atributo obrigatório.<br /><br /> Especifica se o tempo de execução cria <xref:System.Security.Policy.Publisher> evidências.|  
  
## <a name="enabled-attribute"></a>Atributo habilitado  
  
|Valor|Descrição|  
|-----------|-----------------|  
|`false`|Não cria <xref:System.Security.Policy.Publisher> evidências.|  
|`true`|Cria <xref:System.Security.Policy.Publisher> evidências. Este é o padrão.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|`configuration`|O elemento raiz em cada arquivo de configuração usado pelos aplicativos do Common Language Runtime e .NET Framework.|  
|`runtime`|Contém informações sobre opções de inicialização do runtime.|  
  
## <a name="remarks"></a>Comentários  
  
> [!NOTE]
> No .NET Framework 4 e posterior, esse elemento não tem nenhum efeito sobre os tempos de carregamento do assembly. Para obter mais informações, consulte a seção "simplificação da política de segurança" em [alterações de segurança](https://docs.microsoft.com/previous-versions/dotnet/framework/security/security-changes).  
  
 O Common Language Runtime (CLR) tenta verificar a assinatura Authenticode no tempo de carregamento para criar <xref:System.Security.Policy.Publisher> evidências para o assembly. No entanto, por padrão, a maioria dos aplicativos não precisa de <xref:System.Security.Policy.Publisher> evidências. A política de CAS padrão não depende do <xref:System.Security.Policy.PublisherMembershipCondition> . Você deve evitar o custo de inicialização desnecessário associado à verificação da assinatura do Publicador, a menos que seu aplicativo seja executado em um computador com uma política de CAS personalizada ou que pretenda atender às demandas de <xref:System.Security.Permissions.PublisherIdentityPermission> em um ambiente de confiança parcial. (As demandas de permissões de identidade sempre tiveram sucesso em um ambiente de confiança total.)  
  
> [!NOTE]
> Recomendamos que os serviços usem o `<generatePublisherEvidence>` elemento para melhorar o desempenho de inicialização.  O uso desse elemento também pode ajudar a evitar atrasos que podem causar um tempo limite e o cancelamento da inicialização do serviço.  
  
## <a name="configuration-file"></a>Arquivo de configuração  
 Esse elemento só pode ser usado no arquivo de configuração do aplicativo.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar o `<generatePublisherEvidence>` elemento para desabilitar a verificação da política de editor de CAS para um aplicativo.  
  
```xml  
<configuration>  
    <runtime>  
        <generatePublisherEvidence enabled="false"/>  
    </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>Confira também

- [Esquema de configurações do runtime](index.md)
- [Esquema do arquivo de configuração](../index.md)
