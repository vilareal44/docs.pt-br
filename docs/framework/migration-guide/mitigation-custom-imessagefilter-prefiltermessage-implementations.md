---
title: 'Mitigação: implementações personalizadas de IMessageFilter.PreFilterMessage'
description: Saiba mais sobre o IMessageFilter. PreFilterMessage implementação personalizado incluído em aplicativos Windows Forms que visam .NET Framework 4.6.1 e posterior.
ms.date: 03/30/2017
ms.assetid: 9cf47c5b-0bb2-45df-9437-61cd7e7c2f4d
ms.openlocfilehash: 5fe7500d3ed6ff293514495df150a747e7946dda
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475249"
---
# <a name="mitigation-custom-imessagefilterprefiltermessage-implementations"></a>Mitigação: implementações personalizadas de IMessageFilter.PreFilterMessage

Em aplicativos do Windows Forms direcionados a versões do .NET Framework começando com o .NET Framework 4.6.1, uma implementação de <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType> personalizada pode filtrar mensagens quando o método <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> for chamado se a implementação <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage%2A?displayProperty=nameWithType>:

- Executa uma ou ambas as ações:

  - Adiciona um filtro de mensagem chamando o método <xref:System.Windows.Forms.Application.AddMessageFilter%2A>.

  - Remove um filtro de mensagem chamando o método <xref:System.Windows.Forms.Application.RemoveMessageFilter%2A>. método.

- **E** bombeia mensagens chamando o método <xref:System.Windows.Forms.Application.DoEvents%2A?displayProperty=nameWithType>.

## <a name="impact"></a>Impacto

Essa alteração afeta somente os aplicativos do Windows Forms que se destinam a versões do .NET Framework, começando pelo .NET Framework 4.6.1.

Para aplicativos do Windows Forms direcionados a versões anteriores do .NET Framework, essas implementações podem, em alguns casos, lançar uma exceção <xref:System.IndexOutOfRangeException> quando o método <xref:System.Windows.Forms.Application.FilterMessage%2A?displayProperty=nameWithType> é chamado

## <a name="mitigation"></a>Atenuação

Se essa alteração for indesejável, os aplicativos que se destinam ao .NET Framework 4.6.1 ou uma versão posterior podem recusar isso adicionando a configuração a seguir à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo:

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=true" />
</runtime>
```

Além disso, os aplicativos que visam versões anteriores do .NET Framework mas que estão sendo executados no .NET Framework 4.6.1 ou uma versão posterior podem aceitar esse comportamento, adicionando a configuração a seguir à [\<runtime>](../configure-apps/file-schema/runtime/runtime-element.md) seção do arquivo de configuração do aplicativo:

```xml
<runtime>
    <AppContextSwitchOverrides value="Switch.System.Windows.Forms.DontSupportReentrantFilterMessage=false" />
</runtime>
```

## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
