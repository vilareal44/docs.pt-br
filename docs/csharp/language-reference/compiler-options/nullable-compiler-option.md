---
title: -Nullable (opções do compilador C#)
author: IEvangelist
ms.author: dapine
ms.date: 06/04/2020
f1_keywords:
- /nullable
helpviewer_keywords:
- nullable compiler option [C#]
- /nullable compiler option [C#]
- -nullable compiler option [C#]
ms.openlocfilehash: f1aba7e08f472411640d42f51d78ca6f7e5cc900
ms.sourcegitcommit: 4ad2f8920251f3744240c3b42a443ffbe0a46577
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86100880"
---
# <a name="-nullable-c-compiler-options"></a>-Nullable (opções do compilador C#)

A opção **-Nullable** permite especificar o contexto anulável desejado.

## <a name="syntax"></a>Sintaxe

```console
-nullable[+ | -]
-nullable:{enable | disable | warnings | annotations}
```

## <a name="arguments"></a>Argumentos

`+` &#124; `-`  
Especificar `+` ou apenas **anulável**faz com que o compilador habilite o contexto anulável. Especificando `-` , que estará em vigor se você não especificar **-Nullable**, desabilitará o contexto anulável.

`enable`&#124; `disable` &#124; `warnings` &#124;`annotations`  
Especifica a opção de contexto anulável. Semelhante a `+` ou `-` , para habilitar e desabilitar, mas permite mais granularidade da especificidade de contexto anulável. O `enable` argumento, que está em vigor, como se você especificar **-Nullable**, habilita o contexto anulável. Especificar `disable` desabilitará o contexto anulável. Ao fornecer o `warnings` argumento, **-Nullable: Warnings**, o contexto de aviso anulável é habilitado. Ao especificar o `annotations` argumento, **-Nullable: Annotations**, o contexto de anotação anulável é habilitado.

## <a name="remarks"></a>Comentários

A análise de fluxo é usada para inferir a nulidade das variáveis no código executável. A nulidade inferida de uma variável é independente da nulidade declarada da variável. As chamadas de método são analisadas mesmo quando são omitidas condicionalmente. Por exemplo, <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> no modo de versão.

A invocação de métodos anotados com os seguintes atributos também afetará a análise do fluxo:

- Pré-condições simples: <xref:System.Diagnostics.CodeAnalysis.AllowNullAttribute> e<xref:System.Diagnostics.CodeAnalysis.DisallowNullAttribute>
- Pós-condições simples: <xref:System.Diagnostics.CodeAnalysis.MaybeNullAttribute> e<xref:System.Diagnostics.CodeAnalysis.NotNullAttribute>
- Pós-condições condicionais: <xref:System.Diagnostics.CodeAnalysis.MaybeNullWhenAttribute> e<xref:System.Diagnostics.CodeAnalysis.NotNullWhenAttribute>
- <xref:System.Diagnostics.CodeAnalysis.DoesNotReturnIfAttribute>(por exemplo, `DoesNotReturnIf(false)` para <xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> ) e<xref:System.Diagnostics.CodeAnalysis.DoesNotReturnAttribute>
- <xref:System.Diagnostics.CodeAnalysis.NotNullIfNotNullAttribute>
- Pós-condições do membro: <xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String)> e<xref:System.Diagnostics.CodeAnalysis.MemberNotNullAttribute.%23ctor(System.String[])>

### <a name="to-set-this-compiler-option-in-a-project"></a>Para definir essa opção de compilador em um projeto

Edite o arquivo *. csproj* para adicionar a `<Nullable>` marca em uma `Project/PropertyGroup` hierarquia:

```xml
<Project Sdk="...">

  <PropertyGroup>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>
```

## <a name="see-also"></a>Confira também

- [Opções do compilador C#](./index.md)
- [Tipos de referência anuláveis](../../nullable-references.md)
