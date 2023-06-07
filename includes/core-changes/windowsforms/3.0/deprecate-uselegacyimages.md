---
ms.openlocfilehash: d69f619522c7abc3171f1c089e53cc2754899f93
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556120"
---
### <a name="uselegacyimages-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade UseLegacyImages

A `Switch.System.Windows.Forms.UseLegacyImages` opção de compatibilidade, que foi introduzida no .NET Framework 4,8, não tem suporte no Windows Forms no .NET Core ou no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

A partir do .NET Framework 4,8, a `Switch.System.Windows.Forms.UseLegacyImages` opção de compatibilidade resolveu possíveis problemas de dimensionamento de imagem em cenários de ClickOnce em ambientes de DPI alto. Quando definido como `true` , a opção permite que o usuário restaure o dimensionamento de imagem herdado em monitores de DPI alto cuja escala seja definida como maior que 100%. Para obter mais informações, consulte as [notas de versão do .NET Framework 4,8](https://github.com/microsoft/dotnet/blob/master/releases/net48/dotnet48-changes.md#clickonce) no github.

No .NET Core e no .NET 5,0 e posterior, `Switch.System.Windows.Forms.UseLegacyImages` não há suporte para a opção.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

Remova a opção. Não há suporte para a opção e nenhuma funcionalidade alternativa está disponível.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

- Nenhum

<!-- 

#### Affected APIs

- Not detectable via API analysis

-->
