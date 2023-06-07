---
ms.openlocfilehash: 072ed716910e2e1f1f98dbddc56d5bd097b75acc
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87555898"
---
### <a name="microsoftvisualbasicconstantsvbnewline-is-obsolete"></a>Microsoft. VisualBasic. Constants. vbNewLine é obsoleto

A <xref:Microsoft.VisualBasic.Constants.vbNewLine?displayProperty=fullName> constante é marcada como [ \[ obsoleta \] ](xref:System.ObsoleteAttribute) a partir do .NET Core 3,0.

#### <a name="version-introduced"></a>Versão introduzida

3.0

#### <a name="change-description"></a>Descrição da alteração

A partir do .NET Core 3,0, o atributo [obsoleto](xref:System.ObsoleteAttribute) foi aplicado à <xref:Microsoft.VisualBasic.Constants.vbNewLine?displayProperty=fullName> constante. O uso da constante produz um aviso do compilador. Em .NET Framework e versões anteriores do .NET Core, ele não foi marcado como obsoleto.

Essa alteração foi feita para dar suporte a Visual Basic como uma linguagem para o desenvolvimento de várias plataformas. A <xref:Microsoft.VisualBasic.Constants.vbNewLine> constante é equivalente a `\r\n` , a seqüência de caracteres de nova linha no Windows. Em sistemas baseados em UNIX, o caractere de nova linha é `\n` .

#### <a name="recommended-action"></a>Ação recomendada

A mensagem de atributo [obsoleto](xref:System.ObsoleteAttribute) para <xref:Microsoft.VisualBasic.Constants.vbNewLine> o inclui a seguinte recomendação:

Para um retorno de carro e alimentação de linha, use <xref:Microsoft.VisualBasic.Constants.vbCrLf> . Para a nova linha da plataforma atual, use <xref:System.Environment.NewLine?displayProperty=nameWithType> .

#### <a name="category"></a>Categoria

Visual Basic

#### <a name="affected-apis"></a>APIs afetadas

- <xref:Microsoft.VisualBasic.Constants.vbNewLine?displayProperty=fullName>

<!--

#### Affected APIs

- `F:Microsoft.VisualBasic.Constants.vbNewLine`

-->
