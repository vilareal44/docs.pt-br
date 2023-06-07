---
ms.openlocfilehash: ed526095459a48aa37b585dfed79cc12b9fb9e56
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621922"
---
### <a name="some-net-apis-cause-first-chance-handled-entrypointnotfoundexceptions"></a>Algumas APIs .NET causam EntryPointNotFoundExceptions de primeira chance

#### <a name="details"></a>Detalhes

No .NET Framework 4.5, um pequeno número de métodos .NET começou a gerar <xref:System.EntryPointNotFoundException?displayProperty=fullName>s de primeira chance. Essas exceções eram tratadas dentro do .NET Framework, mas podiam interromper a automação de teste que não esperava as exceções de primeira instância. Essas mesmas APIs interrompem alguns cenários ApiVerifier quando HighVersionLie é habilitado.

#### <a name="suggestion"></a>Sugestão

Esse bug pode ser evitado com a o upgrade para o .NET Framework 4.5.1. Como alternativa, a automação de teste pode ser atualizada para não interromper <xref:System.EntryPointNotFoundException?displayProperty=fullName> de primeira chance.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Diagnostics.Debug.Assert(System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Diagnostics.Debug.Assert(System.Boolean,System.String)?displayProperty=nameWithType></li><li><xref:System.Diagnostics.Debug.Assert(System.Boolean,System.String,System.String)?displayProperty=nameWithType></li><li><xref:System.Diagnostics.Debug.Assert(System.Boolean,System.String,System.String,System.Object[])?displayProperty=nameWithType></li><li><xref:System.Xml.Serialization.XmlSerializer.%23ctor(System.Type)></li></ul>|
