---
ms.openlocfilehash: 22b5abbe769733e8d5ca3e78dd9e6e13b2363737
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619796"
---
### <a name="opt-in-break-to-revert-from-different-45-sql-generation-to-simpler-40-sql-generation"></a>Interrupção de aceitação será revertida de geração de SQL 4.5 diferente para geração de SQL 4.0 mais simples

#### <a name="details"></a>Detalhes

As consultas que produzem instruções JOIN e contêm uma chamada para uma operação de limitação sem usar primeiro o OrderBy agora produzem um SQL mais simples. Após a atualização para o .NET Framework 4.5, essas consultas produziram SQLs mais complicados do que as versões anteriores.

#### <a name="suggestion"></a>Sugestão

Por padrão, esse recurso está desabilitado. Se Entity Framework gera instruções JOIN adicionais que causam a degradação do desempenho, pode-se habilitar esse recurso adicionando a seguinte entrada à seção <code>&lt;appSettings&gt;</code> do arquivo (app.config) de configuração da aplicação:<pre><code class="lang-xml">&lt;add key=&quot;EntityFramework_SimplifyLimitOperations&quot; value=&quot;true&quot; /&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Transparente|
|Versão|4.5.2|
|Type|Runtime|
