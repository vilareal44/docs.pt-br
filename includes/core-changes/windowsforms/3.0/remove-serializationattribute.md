---
ms.openlocfilehash: 4fb1ffed97a5b7f906bed13a69f1e71563d11dae
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87556118"
---
### <a name="serializableattribute-removed-from-some-windows-forms-types"></a>SerializableAttribute removido de alguns tipos de Windows Forms

O <xref:System.SerializableAttribute> foi removido de algumas classes de Windows Forms que não têm cenários de serialização binária conhecidos.

#### <a name="change-description"></a>Descrição da alteração

Os tipos a seguir são decorados com o <xref:System.SerializableAttribute> no .NET Framework, mas o atributo foi removido no .NET Core:

- `System.InvariantComparer`
- <xref:System.ComponentModel.Design.ExceptionCollection?displayProperty=nameWithType>
- <xref:System.ComponentModel.Design.Serialization.CodeDomSerializerException?displayProperty=nameWithType>
- `System.ComponentModel.Design.Serialization.CodeDomComponentSerializationService.CodeDomSerializationStore`
- <xref:System.Drawing.Design.ToolboxItem?displayProperty=nameWithType>
- `System.Resources.ResXNullRef`
- `System.Resources.ResXDataNode`
- `System.Resources.ResXFileRef`
- <xref:System.Windows.Forms.Cursor?displayProperty=nameWithType>
- `System.Windows.Forms.NativeMethods.MSOCRINFOSTRUCT`
- `System.Windows.Forms.NativeMethods.MSG`

Historicamente, esse mecanismo de serialização teve preocupações sérias em manutenção e segurança. Manter `SerializableAttribute` em tipos significa que esses tipos devem ser testados para alterações de serialização de versão para versão e alterações de serialização de estrutura para estrutura potencialmente. Isso dificulta a evolução desses tipos e pode ser dispendioso para manter. Esses tipos não têm nenhum cenário de serialização binária conhecido, o que minimiza o impacto da remoção do atributo.

Para obter mais informações, consulte [serialização binária](~/docs/standard/serialization/binary-serialization.md).

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="recommended-action"></a>Ação recomendada

Atualize qualquer código que possa depender desses tipos sendo marcados como serializáveis.

#### <a name="category"></a>Categoria

Windows Forms

#### <a name="affected-apis"></a>APIs afetadas

- Nenhum

<!--

#### Affected APIs

- Not detectable via API analysis

-->
