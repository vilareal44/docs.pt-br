---
ms.openlocfilehash: 3db4b0b42a154c5bc9296889ae9dc4b7ecf1e58e
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556122"
---
### <a name="allowupdatechildcontrolindexfortabcontrols-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade AllowUpdateChildControlIndexForTabControls

A `Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls` opção de compatibilidade tem suporte no Windows Forms no .NET Framework 4,6 e em versões posteriores, mas não tem suporte no .NET Core ou no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

No .NET Framework 4,6 e versões posteriores, a seleção de uma guia reordena sua coleção de controle. A `Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls` opção de compatibilidade permite que um aplicativo ignore essa reordenação quando esse comportamento é indesejável.

No .NET Core e no .NET 5,0 e posterior, `Switch.System.Windows.Forms.AllowUpdateChildControlIndexForTabControls` não há suporte para a opção.

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
