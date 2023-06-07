---
ms.openlocfilehash: 424e8ff704b888aa3d2c1254ac712da4034f59b8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616020"
---
### <a name="obsoleteattribute-exports-as-both-obsoleteattribute-and-deprecatedattribute-in-winmd-scenarios"></a>ObsoleteAttribute exporta ObsoleteAttribute e DeprecatedAttribute em cenários WinMD

#### <a name="details"></a>Detalhes

Quando você cria uma biblioteca de Metadados do Windows (arquivo .winmd), o atributo <xref:System.ObsoleteAttribute?displayProperty=fullName> é exportado como <xref:System.ObsoleteAttribute?displayProperty=fullName> e [ Windows.Foundation.DeprecatedAttribute](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.deprecatedattribute).

#### <a name="suggestion"></a>Sugestão

A recompilação do código-fonte existente que usa o atributo <xref:System.ObsoleteAttribute?displayProperty=fullName> pode gerar avisos durante o consumo desse código do C++/CX ou JavaScript. Não é recomendável aplicar <xref:System.ObsoleteAttribute?displayProperty=fullName> e [Windows.Foundation.DeprecatedAttribute](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.deprecatedattribute) ao código em assemblies gerenciados, isso pode gerar avisos de compilação.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.5.1       |
| Type    | Redirecionando |
