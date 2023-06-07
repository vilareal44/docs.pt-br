---
ms.openlocfilehash: d4f0095bc9fde98087dce528c8154d9c9ac6ddb7
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621768"
---
### <a name="wpf-spell-checking-fails-in-unexpected-ways"></a>A Verificação Ortográfica do WPF falha de formas inesperadas

#### <a name="details"></a>Detalhes

Isso inclui alguns problemas da Verificação Ortográfica do WPF:<ul><li>Às vezes, a Verificação Ortográfica do WPF gera <xref:System.Runtime.InteropServices.COMException?displayProperty=fullName></li><li>A Verificação Ortográfica do WPF falha com <xref:System.UnauthorizedAccessException> quando aplicativos são iniciados usando "Executar como usuário diferente"</li><li>O Verificador Ortográfico do WPF identifica incorretamente erros ortográficos em palavras compostas, como "Hausnummer" em alemão.</li></ul>

#### <a name="suggestion"></a>Sugestão

Problema nº 1 – esse problema foi corrigido no .NET Framework 4.6.2 Problema nº 2 – a Verificação Ortográfica do WPF deixou de ter suporte quando aplicativos são iniciados usando "Executar como usuário diferente". A partir do .NET Framework 4.6.2, aplicativos iniciados dessa maneira não falharão inesperadamente, em vez disso, a Verificação Ortográfica será desabilitada silenciosamente. Problema nº 3 – esse problema foi corrigido no .NET Framework 4.6.2.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6.1|
|Type|Runtime|
