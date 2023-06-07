---
ms.openlocfilehash: c5a061dffa1deb66e1769d6ec70dfa2155ac6a31
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85615597"
---
### <a name="calling-createdefaultauthorizationcontext-with-a-null-argument-has-changed"></a>Chamada para CreateDefaultAuthorizationContext com um argumento nulo foi alterada

#### <a name="details"></a>Detalhes

A implementação de <xref:System.IdentityModel.Policy.AuthorizationContext?displayProperty=fullName> retornado por uma chamada a <xref:System.IdentityModel.Policy.AuthorizationContext.CreateDefaultAuthorizationContext(System.Collections.Generic.IList{System.IdentityModel.Policy.IAuthorizationPolicy})?displayProperty=fullName> com um argumento authorizationPolicies nulo mudou sua implementação no .NET Framework 4.6.

#### <a name="suggestion"></a>Sugestão

Em casos raros, os aplicativos WCF que usam a autenticação personalizada podem ver diferenças de comportamento. Nesses casos, o comportamento anterior pode ser restaurado de uma destas duas maneiras:

- Recompile o aplicativo para se destinar a uma versão anterior ao .NET Framework 4.6. Para serviços hospedados pelo IIS, use o elemento `<httpRuntime targetFramework="x.x">` para direcionar a uma versão anterior do .NET Framework.
- Adicione a seguinte linha à seção `<appSettings>` de seu arquivo app.config:

    ```xml
    <add key="appContext.SetSwitch:Switch.System.IdentityModel.EnableCachedEmptyDefaultAuthorizationContext" value="true" />
    ```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IdentityModel.Policy.AuthorizationContext.CreateDefaultAuthorizationContext(System.Collections.Generic.IList{System.IdentityModel.Policy.IAuthorizationPolicy})?displayProperty=nameWithType>
