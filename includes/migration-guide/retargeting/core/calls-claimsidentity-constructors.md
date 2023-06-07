---
ms.openlocfilehash: b88f7d4a17f885b687d99ab9410a56039e176080
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614270"
---
### <a name="calls-to-claimsidentity-constructors"></a>Chamadas para construtores ClaimsIdentity

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6.2, haverá uma alteração na maneira como os construtores <xref:System.Security.Claims.ClaimsIdentity> com um parâmetro <xref:System.Security.Principal.IIdentity?displayProperty=fullName> configuram a propriedade <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName>. Se o argumento <xref:System.Security.Principal.IIdentity?displayProperty=fullName> for um objeto <xref:System.Security.Claims.ClaimsIdentity> e a propriedade <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> desse objeto <xref:System.Security.Claims.ClaimsIdentity> não for `null`, a propriedade <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> será anexada usando o método <xref:System.Security.Claims.ClaimsIdentity.Clone>. No Framework 4.6.1 e versões anteriores, a propriedade <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> é anexada como uma referência existente. Por causa desta alteração, a partir do .NET Framework 4.6.2, a propriedade <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> do novo objeto <xref:System.Security.Claims.ClaimsIdentity> não será igual à propriedade <xref:System.Security.Claims.ClaimsIdentity.Actor?displayProperty=fullName> do argumento <xref:System.Security.Principal.IIdentity?displayProperty=fullName> do construtor. No .NET Framework 4.6.1 e nas versões anteriores, ele é igual.

#### <a name="suggestion"></a>Sugestão

Se esse comportamento for indesejável, restaure o comportamento anterior definindo a opção `Switch.System.Security.ClaimsIdentity.SetActorAsReferenceWhenCopyingClaimsIdentity` no arquivo de configuração do aplicativo como `true`. Isso exige que você adicione o seguinte à seção `<runtime>` do arquivo web.config:

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Security.ClaimsIdentity.SetActorAsReferenceWhenCopyingClaimsIdentity=true" />
  </runtime>
</configuration>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Security.Principal.IIdentity)>
- <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Security.Principal.IIdentity,System.Collections.Generic.IEnumerable{System.Security.Claims.Claim})>
- <xref:System.Security.Claims.ClaimsIdentity.%23ctor(System.Security.Principal.IIdentity,System.Collections.Generic.IEnumerable{System.Security.Claims.Claim},System.String,System.String,System.String)>
