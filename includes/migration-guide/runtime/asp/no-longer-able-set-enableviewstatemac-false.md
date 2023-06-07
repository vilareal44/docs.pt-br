---
ms.openlocfilehash: cecb7b2abd4f57fdaacb0ea373cc19dc3cd9b24a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619769"
---
### <a name="no-longer-able-to-set-enableviewstatemac-to-false"></a>Não é mais possível definir EnableViewStateMac como false

#### <a name="details"></a>Detalhes

O ASP.NET não permite mais que os desenvolvedores especifiquem <code>&lt;pages enableViewStateMac=&quot;false&quot;/&gt;</code> ou <code>&lt;@Page EnableViewStateMac=&quot;false&quot; %&gt;</code>. O MAC (Message Authentication Code) de estado da exibição agora é obrigatório em todas as solicitações com estado de exibição embutido. Apenas aplicativos que definiram explicitamente a propriedade EnableViewStateMac como <code>false</code> são afetados.

#### <a name="suggestion"></a>Sugestão

O EnableViewStateMac deve ser considerado como true e os erros MAC resultantes devem ser resolvidos (conforme explicado nesta [diretriz](https://support.microsoft.com/kb/2915218), que contém várias resoluções dependendo das especificidades do que está causando erros de Mac).

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Principal|
|Versão|4.5.2|
|Type|Runtime|
