---
ms.openlocfilehash: ad953a1562db407c04d7860c60eb5964fe6fe2ca
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619803"
---
### <a name="deserialization-of-mailmessage-objects-serialized-under-the-net-framework-45-may-fail"></a>Desserialização de objetos MailMessage serializados no .NET Framework 4.5 pode falhar

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.5, os objetos <xref:System.Web.Mail.MailMessage> podem incluir caracteres não ASCII. No .NET Framework 4, somente caracteres ASCII têm suporte. Os objetos <xref:System.Web.Mail.MailMessage> que contêm caracteres não ASCII e são serializados no .NET Framework 4.5 ou versões posteriores não podem ser desserializados no .NET Framework 4.

#### <a name="suggestion"></a>Sugestão

Verifique se o seu código fornece tratamento de exceção ao desserializar um objeto <xref:System.Web.Mail.MailMessage>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Web.Mail.MailMessage?displayProperty=nameWithType></li></ul>|
