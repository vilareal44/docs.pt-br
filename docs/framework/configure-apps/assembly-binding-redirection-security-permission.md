---
title: Permissão de segurança para redirecionamento de associações de assemblies
description: Saiba mais sobre a permissão de segurança necessária para redirecionamento de associação de assembly explícita em um arquivo de configuração de aplicativo no .NET.
ms.date: 03/30/2017
helpviewer_keywords:
- side-by-side execution, assembly binding redirection
- assemblies [.NET Framework], binding redirection
ms.assetid: 24a5cdff-7ed9-4195-93f3-edf6899019fc
ms.openlocfilehash: a8596bcac4efb0aea07efcfde6726d8bbf148c24
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105092"
---
# <a name="assembly-binding-redirection-security-permission"></a>Permissão de segurança para redirecionamento de associações de assemblies
O redirecionamento de associação de assembly explícito em um arquivo de configuração do aplicativo requer uma permissão de segurança. Isso se aplica ao redirecionamento de assemblies do .NET Framework e assemblies de terceiros. A permissão é concedida definindo o <xref:System.Security.Permissions.SecurityPermissionFlag> sinalizador no <xref:System.Security.Permissions.SecurityPermission> . Os assemblies gerenciados não têm permissões por padrão.  
  
 A permissão de segurança é concedida a aplicativos executados na zona de zona confiável (computador local) e na intranet. Os aplicativos executados na zona da Internet são estritamente proibidos de executar o redirecionamento de associação de assembly.  
  
 A permissão não será necessária se o redirecionamento de assembly for executado em um arquivo de política do Publicador que é controlado pelo editor de componentes ou no arquivo de configuração do computador que é controlado pelo administrador. No entanto, a permissão é necessária para que um aplicativo ignore explicitamente a política do editor usando o [\<publisherPolicy apply="no"/>](./file-schema/runtime/publisherpolicy-element.md) elemento no arquivo de configuração do aplicativo.  
  
 A tabela a seguir mostra as configurações de segurança padrão para o sinalizador **BindingRedirects** .  
  
|Zona|Configuração do sinalizador BindingRedirects|  
|----------|-----------------------------------|  
|Zona confiável (computador local)|**ON**|  
|Zona da Intranet|**ON**|  
|Zona da Internet|**OFF**|  
|Zonas não confiáveis|**OFF**|  
  
 Um administrador pode alterar essas configurações de segurança para dar suporte ou restringir cenários específicos em um determinado computador. Não há ferramentas para alterar a configuração de sinalizador **BindingRedirects** do padrão; um administrador deve editar manualmente o arquivo de Security.config no computador de um usuário.  
  
## <a name="see-also"></a>Veja também

- [Arquivos de política de editor e execução lado a lado](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/06d2bae3(v=vs.100))
- [Como: Habilitar e desabilitar o redirecionamento automático de associação](how-to-enable-and-disable-automatic-binding-redirection.md)
- [Execução lado a lado](../deployment/side-by-side-execution.md)
