---
title: 'Mitigação: verificações de dois-pontos no caminho'
description: Saiba mais sobre as alterações feitas no .NET Framework 4.6.2 para dar suporte a verificações da sintaxe do separador de unidade apropriada (os dois-pontos).
ms.date: 03/30/2017
ms.assetid: a0bb52de-d279-419d-8f23-4b12d1a3f36e
ms.openlocfilehash: f32ee54f88bc4747fd0d8065b0dce06b151d1d9a
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475444"
---
# <a name="mitigation-path-colon-checks"></a>Mitigação: verificações de dois-pontos no caminho
Começando com os aplicativos direcionados ao .NET Framework 4.6.2, várias alterações foram feitas para dar suporte aos caminhos anteriormente sem suporte (em termos de comprimento e formato). Em particular, as verificações da sintaxe adequada do separador de unidade (os dois-pontos) foram corrigidas.  
  
## <a name="impact"></a>Impacto  
 Essas alterações bloqueiam alguns caminhos de URI aos quais esses os métodos <xref:System.IO.Path.GetDirectoryName%2A?displayProperty=nameWithType> e <xref:System.IO.Path.GetPathRoot%2A?displayProperty=nameWithType> anteriormente davam suporte.  
  
## <a name="mitigation"></a>Atenuação  
 Para contornar o problema de um caminho aceitável anteriormente que não tem mais suporte pelos métodos <xref:System.IO.Path.GetDirectoryName%2A?displayProperty=nameWithType> e <xref:System.IO.Path.GetPathRoot%2A?displayProperty=nameWithType>, é possível fazer o seguinte:  
  
- Remova manualmente o esquema de uma URL. Por exemplo, remova `file://` de uma URL.  
  
- Passe o URI para um construtor <xref:System.Uri> e recupere o valor da propriedade <xref:System.Uri.LocalPath%2A?displayProperty=nameWithType>.  
  
- Recuse a normalização do novo caminho definindo a opção `Switch.System.IO.UseLegacyPathHandling`<xref:System.AppContext> como `true`.  
  
    ```xml  
    <runtime>  
        <AppContextSwitchOverrides value="Switch.System.IO.UseLegacyPathHandling=true" />
    </runtime>  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
