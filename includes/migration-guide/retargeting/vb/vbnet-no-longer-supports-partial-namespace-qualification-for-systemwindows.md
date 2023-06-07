---
ms.openlocfilehash: cef8096c971da8ae245ff974697022f350cb9195
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85616014"
---
### <a name="vbnet-no-longer-supports-partial-namespace-qualification-for-systemwindows-apis"></a>VB.NET não é mais compatível com a qualificação do namespace parcial para APIs System.Windows

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.5.2, os projetos VB.NET não podem especificar APIs System.Windows com namespaces parcialmente qualificados. Por exemplo, fazer referência a `Windows.Forms.DialogResult` falhará. Em vez disso, o código deve fazer referência ao nome totalmente qualificado (<xref:System.Windows.Forms.DialogResult>) ou importar o namespace específico e simplesmente fazer referência a <xref:System.Windows.Forms.DialogResult?displayProperty=fullName>.

#### <a name="suggestion"></a>Sugestão

O código deve ser atualizado para fazer referência as APIs `System.Windows` com nomes simples (e importando o namespace relevante) ou com nomes totalmente qualificados.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.5.2       |
| Type    | Redirecionando |
