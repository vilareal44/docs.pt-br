---
ms.openlocfilehash: 3e4a5936bbac4e77efc5f7e68b55ddf49bae5d43
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614283"
---
### <a name="path-colon-checks-are-stricter"></a>Verificações de dois-pontos em caminhos estão mais rigorosas

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2, uma série de alterações foram feitas para dar suporte aos caminhos incompatíveis anteriormente (em termos de comprimento e formato). As verificações de sintaxe do separador de unidades (dois-pontos) correto foram aperfeiçoadas, o que teve como efeito colateral o bloqueio de alguns caminhos de URI em algumas APIs de Caminho selecionadas em que eles costumavam ser aceitos.

#### <a name="suggestion"></a>Sugestão

Ao passar um URI para as APIs afetadas, modifique a cadeia de caracteres para um caminho correto antes.<ul><li>Remova o esquema das URLs manualmente (por exemplo, remova `file://` das URLs).

- Passe o URI para a classe <xref:System.Uri> e use <xref:System.Uri.LocalPath>.

Como alternativa, é possível recusar a normalização do novo caminho definindo a opção AppContext `Switch.System.IO.UseLegacyPathHandling` como true.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IO.Path.GetDirectoryName(System.String)?displayProperty=nameWithType>
- <xref:System.IO.Path.GetPathRoot(System.String)?displayProperty=nameWithType>
