---
title: 'Mitigação: quadros PNG em objetos de ícone'
description: Saiba como configurar o comportamento de quadros PNG em objetos de ícone se o novo comportamento incluído no .NET Framework 4,6 e posterior for indesejável.
ms.date: 03/30/2017
ms.assetid: ca87fefb-7144-4b4e-8832-5a939adbb4b2
ms.openlocfilehash: b7ba2951a38ee2d1c7a9b1fc45c5a81d24986a85
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475431"
---
# <a name="mitigation-png-frames-in-icon-objects"></a>Mitigação: quadros PNG em objetos de ícone
A partir do .NET Framework 4.6, o método <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> converte com êxito ícones com quadros PNG em objetos <xref:System.Drawing.Bitmap>.  
  
 Em aplicativos direcionados ao .NET Framework 4.5.2 e versões anteriores, o método <xref:System.Drawing.Icon.ToBitmap%2A?displayProperty=nameWithType> gera uma exceção <xref:System.ArgumentOutOfRangeException> quando o objeto <xref:System.Drawing.Icon> tem quadros PNG.  
  
## <a name="impact"></a>Impacto  
 Essa alteração afeta aplicativos que são recompilados para direcionamento ao .NET Framework 4.6 e que implementam tratamento especial para a <xref:System.ArgumentOutOfRangeException> que será gerada se um objeto <xref:System.Drawing.Icon> tiver quadros PNG. Ao executar no .NET Framework 4.6, a conversão é bem-sucedida, uma <xref:System.ArgumentOutOfRangeException> não é mais gerada e, portanto, o manipulador de exceção não é invocado.  
  
### <a name="mitigation"></a>Atenuação  
 Se esse comportamento for indesejável, você poderá manter o comportamento anterior adicionando o seguinte elemento à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do seu arquivo de app.config:  
  
```xml  
<AppContextSwitchOverrides
      value="Switch.System.Drawing.DontSupportPngFramesInIcons=true" />  
```  
  
 Se o arquivo app.config já contiver o elemento `AppContextSwitchOverrides`, o novo valor deverá ser mesclado ao atributo `value`, da seguinte forma:  
  
```xml  
<AppContextSwitchOverrides
      value="Switch.System.Drawing.DontSupportPngFramesInIcons=true;previous key=previous-value" />
```
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
