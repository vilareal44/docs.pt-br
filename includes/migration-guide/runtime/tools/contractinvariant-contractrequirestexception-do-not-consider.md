---
ms.openlocfilehash: 29c66edfeb1690199aac39b9c3076d161b2075d4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621039"
---
### <a name="contractinvariant-or-contractrequirestexception-do-not-consider-stringisnullorempty-to-be-pure"></a>Contract.Invariant ou Contract.Requires\<TException> não consideram String.IsNullOrEmpty puro

#### <a name="details"></a>Detalhes

Para aplicativos direcionados para o .NET Framework 4.6.1, se o contrato invariável para o <xref:System.Diagnostics.Contracts.Contract.Invariant%2A?displayProperty=nameWithType> ou o contrato de pré-condição para <xref:System.Diagnostics.Contracts.Contract.Requires%2A?displayProperty=nameWithType)> chamar o <xref:System.String.IsNullOrEmpty%2A?displayProperty=nameWithType> método, o regravador emitirá o aviso do compilador CC1036: &quot; chamada para o método ' System. String. IsNullOrWhteSpace (System. String) ' sem [Pure] no método. &quot; Esse é um aviso do compilador em vez de um erro do compilador.

#### <a name="suggestion"></a>Sugestão

Esse comportamento foi abordado em [Problema nº 339 no GitHub](https://github.com/Microsoft/CodeContracts/issues/339). Para eliminar esse aviso, você pode baixar e compilar uma versão atualizada do código-fonte para a ferramenta de Contratos de Código no [GitHub](https://github.com/Microsoft/CodeContracts/blob/master/README.md). As informações para download são encontradas no fim da página.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Diagnostics.Contracts.Contract.Invariant(System.Boolean)?displayProperty=nameWithType></li><li><xref:System.Diagnostics.Contracts.Contract.Requires(System.Boolean)?displayProperty=nameWithType></li></ul>|
