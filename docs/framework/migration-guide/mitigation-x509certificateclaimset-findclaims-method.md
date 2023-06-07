---
title: 'Mitigação: método X509CertificateClaimSet.FindClaims'
description: Saiba como o método X509CertificateClaimSet. FindClaims foi alterado para aplicativos direcionados .NET Framework 4.6.1.
ms.date: 03/30/2017
ms.assetid: ee356e3b-f932-48f5-875a-5e42340bee63
ms.openlocfilehash: 304d8fb5adc27b33f2410faaaf8662e0ff9be66d
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475314"
---
# <a name="mitigation-x509certificateclaimsetfindclaims-method"></a>Mitigação: método X509CertificateClaimSet.FindClaims

A partir de aplicativos direcionados .NET Framework 4.6.1, o <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=nameWithType> método tentará corresponder o `claimType` argumento com todas as entradas DNS em seu campo San.  
  
## <a name="impact"></a>Impacto  
 Essa alteração afeta somente os aplicativos que se destinam a versões do .NET Framework, começando pelo .NET Framework 4.6.1.  
  
 Para aplicativos direcionados a versões anteriores do .NET Framework, o método <xref:System.IdentityModel.Claims.X509CertificateClaimSet.FindClaims%2A?displayProperty=nameWithType> tenta corresponder o argumento `claimType` apenas com a última entrada DNS.  
  
## <a name="mitigation"></a>Atenuação  
 Se essa alteração for indesejável, os aplicativos que se destinam a versões do .NET Framework a partir do .NET Framework 4.6.1 podem recusar isso adicionando a configuração a seguir à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo:  
  
```xml  
<runtime>  
   <AppContextSwitchOverrides value="Switch.System.IdentityModel.DisableMultipleDNSEntriesInSANCertificate=true" />
</runtime>  
```  
  
 Além disso, os aplicativos que se destinam a versões anteriores do .NET Framework, mas que estão sendo executados no .NET Framework 4.6.1 e versões posteriores podem aceitar esse comportamento, adicionando a configuração a seguir à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo:  
  
```xml  
<runtime>  
    <AppContextSwitchOverrides value="Switch.System.IdentityModel.DisableMultipleDNSEntriesInSANCertificate=false" />
</runtime>  
```  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
