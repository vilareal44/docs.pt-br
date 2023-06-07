---
ms.openlocfilehash: 196a26bd235e5e2556baa7fac979b3316ff81e1f
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556119"
---
### <a name="enablevisualstylevalidation-compatibility-switch-not-supported"></a>Não há suporte para a opção de compatibilidade EnableVisualStyleValidation

`Switch.System.Windows.Forms.EnableVisualStyleValidation`Não há suporte para a opção de compatibilidade no Windows Forms no .NET Core ou no .net 5,0 e posterior.

#### <a name="change-description"></a>Descrição da alteração

No .NET Framework, a `Switch.System.Windows.Forms.EnableVisualStyleValidation` opção de compatibilidade permitia que um aplicativo recusasse a validação de estilos visuais fornecidos em um formato numérico.

No .NET Core e no .NET 5,0 e posterior, `Switch.System.Windows.Forms.EnableVisualStyleValidation` não há suporte para a opção.

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
