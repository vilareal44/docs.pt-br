---
ms.openlocfilehash: af10716fe5f4c07091e8605cdf620e4a499fb1e8
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619771"
---
### <a name="ipad-should-not-be-used-in-custom-capabilities-file-because-it-is-now-a-browser-capability"></a>IPad não deve ser usado no arquivo de funcionalidades personalizadas, pois agora é uma funcionalidade do navegador

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.5, o iPad é um identificador no arquivo de funcionalidades padrão do navegador ASP.NET, portanto ele não deve ser usado em um arquivo de funcionalidades personalizados

#### <a name="suggestion"></a>Sugestão

Se funcionalidades específicas do iPad forem exigidas, será necessário modificar o comportamento dele configurando funcionalidades na refID do gateway predefinido &quot;IPad&quot; em vez de gerar uma nova ID &quot;IPad&quot; pela correspondência do agente do usuário.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime|
