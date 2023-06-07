---
ms.openlocfilehash: d29be721b50d1c93723b325774a06e86f77dbebf
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621019"
---
### <a name="wcf-addressheadercollection-now-throws-an-argumentexception-if-an-addressheader-element-is-null"></a>AddressHeaderCollection do WCF agora gera ArgumentException se um elemento addressHeader for nulo

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.7.1, o construtor <xref:System.ServiceModel.Channels.AddressHeaderCollection.%23ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})> gera <xref:System.ArgumentException> se um elemento for <code>null</code>. No .NET Framework 4.7 e versões anteriores, nenhuma exceção é gerada.

#### <a name="suggestion"></a>Sugestão

Se houver problemas de compatibilidade com essa alteração no .NET Framework 4.7.1 ou uma versão anterior, será possível recusá-la adicionando a seguinte linha à seção <code>&lt;runtime&gt;</code> do arquivo app.config:<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.ServiceModel.DisableAddressHeaderCollectionValidation=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.7.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.ServiceModel.Channels.AddressHeaderCollection.%23ctor(System.Collections.Generic.IEnumerable{System.ServiceModel.Channels.AddressHeader})></li></ul>|
