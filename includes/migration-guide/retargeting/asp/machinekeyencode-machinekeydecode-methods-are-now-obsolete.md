---
ms.openlocfilehash: e9d34465970287ed94c0f0373ee4ae94d55aa1ff
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617104"
---
### <a name="machinekeyencode-and-machinekeydecode-methods-are-now-obsolete"></a>Os métodos MachineKey.Encode e MachineKey.Decode agora estão obsoletos

#### <a name="details"></a>Detalhes

Esses métodos são agora obsoletos. A compilação de código que chama estes métodos gera um aviso do compilador.

#### <a name="suggestion"></a>Sugestão

As alternativas recomendadas são <xref:System.Web.Security.MachineKey.Protect(System.Byte[],System.String[])> e <xref:System.Web.Security.MachineKey.Unprotect(System.Byte[],System.String[])>. Como alternativa, os avisos de compilação podem ser suprimidos ou evitados usando um compilador mais antigo. Ainda há suporte para as APIs.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Web.Security.MachineKey.Encode(System.Byte[],System.Web.Security.MachineKeyProtection)?displayProperty=nameWithType>
- <xref:System.Web.Security.MachineKey.Decode(System.String,System.Web.Security.MachineKeyProtection)?displayProperty=nameWithType>
