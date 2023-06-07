---
ms.openlocfilehash: 3f330d5e601d599231ef0336b785e677fe1d114e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616015"
---
### <a name="dataobjectgetdata-now-retrieves-data-as-utf-8"></a>DataObject.GetData agora recupera dados como UTF-8

#### <a name="details"></a>Detalhes

Para aplicativos destinados ao .NET Framework 4 ou que são executados no .NET Framework 4.5.1 ou em versões mais recentes, `DataObject.GetData` recupera dados formatados em HTML como uma cadeia de caracteres ASCII. Como resultado, caracteres não ASCII (caracteres cujos códigos ASCII são maiores que 0x7F) são representados por dois caracteres aleatórios.<p/>Para aplicativos direcionados ao .NET Framework 4.5 ou posteriores e executados no .NET Framework 4.5.2, o `DataObject.GetData` recupera dados formatados em HTML como UTF-8, que representam caracteres maiores que 0x7F corretamente.

#### <a name="suggestion"></a>Sugestão

Se você implementou uma solução alternativa para o problema de codificação com cadeias de caracteres formatadas em HTML (por exemplo, codificando explicitamente a cadeia de caracteres HTML recuperada da área de transferência enviando-a para <xref:System.Text.UTF8Encoding.GetString(System.Byte[],System.Int32,System.Int32)?displayProperty=fullName>) e estiver redirecionando seu aplicativo da versão 4 para a versão 4.5, essa solução deverá ser removida. Se o comportamento antigo for necessário por algum motivo, o aplicativo poderá ser destinado ao .NET Framework 4.0 para obtê-lo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.5.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.DataObject.GetData(System.String)?displayProperty=nameWithType>
- <xref:System.Windows.DataObject.GetData(System.Type)?displayProperty=nameWithType>
- <xref:System.Windows.DataObject.GetData(System.String,System.Boolean)?displayProperty=nameWithType>
