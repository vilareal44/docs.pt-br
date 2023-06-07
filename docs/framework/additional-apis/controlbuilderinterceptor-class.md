---
title: Classe ControlBuilderInterceptor
ms.date: 08/11/2020
api_name:
- System.Web.Compilation.ControlBuilderInterceptor
api_location:
- System.Web.dll
api_type:
- Assembly
topic_type:
- apiref
ms.openlocfilehash: 0cd7409deb9cb84783cfa70600999fa4b2a2d2e2
ms.sourcegitcommit: d337df55f83325918cbbd095eb573400bea49064
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88187985"
---
# <a name="controlbuilderinterceptor-class"></a>Classe ControlBuilderInterceptor

A `ControlBuilderInterceptor` classe permite que o processo de compilação seja personalizado ou controlado.

## <a name="syntax"></a>Sintaxe

```csharp
internal class ControlBuilderInterceptor
```

> [!WARNING]
> A `ControlBuilderInterceptor` classe é interna e não deve ser usada diretamente no seu código.
>
> Conforme descrito na seção comentários, a existência desse tipo pode ser verificada para determinar se o suporte ao tipo de interceptador está presente. A Microsoft não oferece suporte a nenhum outro uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="remarks"></a>Comentários

No .NET Framework 2,0 e .NET Framework 3,5, as atualizações de [agosto de 2020](https://portal.msrc.microsoft.com/security-guidance/releasenotedetail/2020-Aug) adicionaram suporte para o uso de um tipo de interceptador para personalizar ou controlar o processo de compilação. Você pode determinar se esse suporte está presente usando o <xref:System.Type.GetType?displayProperty=nameWithType> para verificar a existência do `ControlBuilderInterceptor` tipo, conforme demonstrado no código a seguir.

```csharp
Type type = Type.GetType("System.Web.Compilation.ControlBuilderInterceptor, System.Web, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a");
```

Se o valor de retorno for não nulo, o suporte ao Interceptor estará presente. Se o valor de retorno for `null` , ou se uma exceção for lançada, as atualizações de agosto de 2020 não foram instaladas e o suporte ao Interceptor está ausente.

Se o suporte ao Interceptor estiver presente, você poderá gravar e registrar um tipo de interceptador que irá interagir com o processo de compilação exatamente da mesma forma que o <xref:System.Web.Compilation.ControlBuilderInterceptor> faz em versões posteriores do .NET Framework. No .NET Framework 2,0 e .NET Framework 3,5, o tipo de interceptador pode ser qualquer classe que atenda aos seguintes requisitos:

* Tem um construtor público sem parâmetros.
* Tem métodos públicos e não estáticos chamados `PreControlBuilderInit` e `OnProcessGeneratedCode` que têm a mesma assinatura e semântica que os <xref:System.Web.Compilation.ControlBuilderInterceptor.PreControlBuilderInit(System.Web.UI.ControlBuilder,System.Web.UI.TemplateParser,System.Web.UI.ControlBuilder,System.Type,System.String,System.String,System.Collections.IDictionary,System.Collections.IDictionary)> métodos e <xref:System.Web.Compilation.ControlBuilderInterceptor.OnProcessGeneratedCode(System.Web.UI.ControlBuilder,System.CodeDom.CodeCompileUnit,System.CodeDom.CodeTypeDeclaration,System.CodeDom.CodeTypeDeclaration,System.CodeDom.CodeMemberMethod,System.CodeDom.CodeMemberMethod,System.Collections.IDictionary)> , que existem em versões posteriores do .NET Framework.

Registre o tipo de interceptador usando a `aspnet:20ControlBuilderInterceptor` chave em configurações do aplicativo ASP.net ( `<appSettings>` ). Essa configuração de aplicativo deve estar listada no computador ou no arquivo de *web.config* do aplicativo. Especifique o tipo de interceptador usando seu nome de tipo qualificado por assembly. O exemplo a seguir mostra como registrar um tipo de interceptador chamado `Fabrikam.Interceptor` .

```xml
<configuration>
  ...
  <appSettings>
    ...
    <add key="aspnet:20ControlBuilderInterceptor"
         value="Fabrikam.Interceptor, Fabrikam, Version=1.0.0.0, Culture=neutral, PublicKeyToken=2b3831f2f2b744f7" />
  </appSettings>
</configuration>
```

Para recuperar o nome qualificado do assembly de um tipo, use a <xref:System.Type.AssemblyQualifiedName?displayProperty=nameWithType> propriedade, conforme demonstrado no código a seguir.

```csharp
string assemblyQualifiedName = typeof(Fabrikam.Interceptor).AssemblyQualifiedName;
```

Quando o suporte ao Interceptor está presente, o processo de compilação interage com o tipo listado da maneira descrita acima. Quando o suporte ao Interceptor está ausente, a configuração do aplicativo é ignorada e não tem nenhum efeito.

## <a name="requirements"></a>Requisitos

**Namespace:** System. Web. Compilation

**Assembly:** System. Web (em System.Web.dll)

**Versões do .NET Framework:** 3,5, 2,0
