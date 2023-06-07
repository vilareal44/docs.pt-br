---
ms.openlocfilehash: ee4a27dde9f1e7756401e3d8b709514082f5d3b1
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556124"
---
### <a name="domainupdownuselegacyscrolling-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade DomainUpDown. UseLegacyScrolling

A `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` opção de compatibilidade, que foi introduzida no .NET Framework 4.7.1, não tem suporte no Windows Forms no .NET Core ou no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

A partir do .NET Framework 4.7.1, a `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` opção de compatibilidade permitia que os desenvolvedores desaceitem <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> ações e independentes <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> . A opção restaurou o comportamento herdado, no qual <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> será ignorada se o texto do contexto estiver presente e o desenvolvedor for solicitado a usar a <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType> ação no controle antes da <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType> ação. Para obter mais informações, consulte [ \<AppContextSwitchOverrides> elemento](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md).

No .NET Core e no .NET 5,0 e posterior, `Switch.System.Windows.Forms.DomainUpDown.UseLegacyScrolling` não há suporte para a opção.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

Remova a opção. Não há suporte para a opção e nenhuma funcionalidade alternativa está disponível.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Forms.DomainUpDown.DownButton?displayProperty=nameWithType>
- <xref:System.Windows.Forms.DomainUpDown.UpButton?displayProperty=nameWithType>

<!-- 

#### Affected APIs

- `M:System.Windows.Forms.DomainUpDown.DownButton`
- `M:System.Windows.Forms.DomainUpDown.UpButton`

-->
