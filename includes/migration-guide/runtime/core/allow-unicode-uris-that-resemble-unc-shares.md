---
ms.openlocfilehash: 3e8601ba76dfb05e3d70b3af7440bd7e228768d0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621010"
---
### <a name="allow-unicode-in-uris-that-resemble-unc-shares"></a>Permitir Unicode em URIs semelhantes a compartilhamentos UNC

#### <a name="details"></a>Detalhes

No <xref:System.Uri?displayProperty=fullName>, a criação de um URI de arquivo contendo um nome do compartilhamento UNC e caracteres Unicode não resultará mais em um URI com estado interno inválido. O comportamento será alterado somente quando todos os itens a seguir forem verdadeiros:<ul><li>O URI tiver o esquema <code>file:</code> e for seguido por quatro ou mais barras.</li><li>O nome do host começar com um sublinhado ou outro símbolo não reservado.</li><li>O URI contiver caracteres Unicode.</li></ul>

#### <a name="suggestion"></a>Sugestão

Os aplicativos que funcionam com URIs que contêm Unicode de forma consistente podem ter aceitado o uso desse comportamento para não permitir referências a compartilhamentos UNC. Esses aplicativos devem usar <xref:System.Uri.IsUnc>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Uri?displayProperty=nameWithType></li></ul>|
