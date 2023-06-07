---
ms.openlocfilehash: 9b184f54650d2efb31ec66f443198b19ceaeb5f3
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614281"
---
### <a name="applicationfiltermessage-no-longer-throws-for-re-entrant-implementations-of-imessagefilterprefiltermessage"></a>Application.FilterMessage não é mais gerada para implementações de reentrância de IMessageFilter.PreFilterMessage

#### <a name="details"></a>Detalhes

Antes do .NET Framework 4.6.1, a chamada a uma <xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)> com uma <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage(System.Windows.Forms.Message@)> que chamava <xref:System.Windows.Forms.Application.AddMessageFilter(System.Windows.Forms.IMessageFilter)?displayProperty=fullName> ou <xref:System.Windows.Forms.Application.RemoveMessageFilter(System.Windows.Forms.IMessageFilter)?displayProperty=fullName> (ao chamar <xref:System.Windows.Forms.Application.DoEvents> também) causava uma <xref:System.IndexOutOfRangeException?displayProperty=fullName>.<p/>A partir dos aplicativos destinados ao .NET Framework 4.6.1, essa exceção não é mais gerada e filtros reentrantes, conforme descrito acima, podem ser usados.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que <xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)> não será mais gerado para o comportamento <xref:System.Windows.Forms.IMessageFilter.PreFilterMessage(System.Windows.Forms.Message@)> reentrante descrito acima. Isso afeta somente os aplicativos destinados ao .NET Framework 4.6.1. Os aplicativos destinados ao .NET Framework 4.6.1 podem recusar essa alteração (ou os aplicativos de Frameworks mais antigos podem aceitar) usando a opção de compatibilidade [DontSupportReentrantFilterMessage](~/docs/framework/migration-guide/mitigation-custom-imessagefilter-prefiltermessage-implementations.md#mitigation).

| Name          | Valor       |
|:--------------|:------------|
| Escopo         | Microsoft Edge        |
| Versão       | 4.6.1       |
| Type          | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Forms.Application.FilterMessage(System.Windows.Forms.Message@)?displayProperty=nameWithType>
