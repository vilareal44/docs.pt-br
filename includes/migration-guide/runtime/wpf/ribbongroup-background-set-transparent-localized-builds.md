---
ms.openlocfilehash: a3ba42868577ac20ea2e082f90fc4973a1bfe108
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622336"
---
### <a name="ribbongroup-background-is-set-to-transparent-in-localized-builds"></a>Tela de fundo RibbonGroup é definida como transparente em builds localizados

#### <a name="details"></a>Detalhes

A tela de fundo <xref:System.Windows.Controls.Ribbon.RibbonGroup?displayProperty=fullName> em builds localizados sempre foi pintada com um pincel Transparente, o que resultava em uma experiência de interface do usuário ruim. Isso foi corrigido no WPF do .NET Framework 4.7 por meio da atualização dos recursos localizados para <xref:System.Windows.Controls.Ribbon.RibbonGroup?displayProperty=fullName>, que, por sua vez, garante que o pincel correto seja selecionado.

#### <a name="suggestion"></a>Sugestão

Atualizar para o .NET Framework 4.7

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6.2|
|Type|Runtime|
