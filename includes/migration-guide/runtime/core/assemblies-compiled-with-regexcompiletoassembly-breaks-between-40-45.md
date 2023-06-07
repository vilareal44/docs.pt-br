---
ms.openlocfilehash: 0bd90b3d479a7e0897aaf78b7718ae156a4a239f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619773"
---
### <a name="assemblies-compiled-with-regexcompiletoassembly-breaks-between-40-and-45"></a>Assemblies compilados com interrupções Regex.CompileToAssembly entre o 4.0 e o 4.5

#### <a name="details"></a>Detalhes

Se uma assembly de expressões regulares compiladas é trabalhada com o .NET Framework 4.5 mas é destinada ao .NET Framework 4, numa tentativa de usar uma das expressões regulares em que a assembly de um sistema com .NET Framework 4 instalado gera uma exceção.

#### <a name="suggestion"></a>Sugestão

Para solucionar esse problema, pode-se seguir uma das seguintes alternativas:<ul><li>Compilar o assembly que contém as expressões regulares com o .NET Framework 4.</li><li>Use uma expressão regular interpretada.</li></ul>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Text.RegularExpressions.Regex.CompileToAssembly(System.Text.RegularExpressions.RegexCompilationInfo[],System.Reflection.AssemblyName)?displayProperty=nameWithType></li><li><xref:System.Text.RegularExpressions.Regex.CompileToAssembly(System.Text.RegularExpressions.RegexCompilationInfo[],System.Reflection.AssemblyName,System.Reflection.Emit.CustomAttributeBuilder[])?displayProperty=nameWithType></li><li><xref:System.Text.RegularExpressions.Regex.CompileToAssembly(System.Text.RegularExpressions.RegexCompilationInfo[],System.Reflection.AssemblyName,System.Reflection.Emit.CustomAttributeBuilder[],System.String)?displayProperty=nameWithType></li></ul>|
