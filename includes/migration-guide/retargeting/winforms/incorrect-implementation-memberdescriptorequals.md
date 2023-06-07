---
ms.openlocfilehash: 2d0798917b639bc90bf3f78f6fb9f19d0eaf2c71
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614299"
---
### <a name="incorrect-implementation-of-memberdescriptorequals"></a>Implementação incorreta de MemberDescriptor.Equals

#### <a name="details"></a>Detalhes

A implementação original do método <xref:System.ComponentModel.MemberDescriptor.Equals%2A?displayProperty=nameWithType> compara duas propriedades de cadeia de caracteres diferentes dos objetos que estão sendo comparados: o nome da categoria e a cadeia de caracteres de descrição. A correção é para comparar o <xref:System.ComponentModel.MemberDescriptor.Category> do primeiro objeto com o <xref:System.ComponentModel.MemberDescriptor.Category> de um segundo objeto, e o <xref:System.ComponentModel.MemberDescriptor.Description> do primeiro com o <xref:System.ComponentModel.MemberDescriptor.Description> do segunda.

#### <a name="suggestion"></a>Sugestão

Se seu aplicativo depende do <xref:System.ComponentModel.MemberDescriptor.Equals%2A?displayProperty=nameWithType> e, às vezes, retorna `false` quando os descritores são equivalentes, e você está direcionado ao .NET Framework 4.6.2 ou posterior, há várias opções:

- Fazer alterações no código para comparar os campos <xref:System.ComponentModel.MemberDescriptor.Category> e <xref:System.ComponentModel.MemberDescriptor.Description> manualmente além de chamar o método <xref:System.ComponentModel.MemberDescriptor.Equals%2A?displayProperty=nameWithType>.
- Recuse essa alteração adicionando o seguinte valor ao arquivo app.config:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.MemberDescriptorEqualsReturnsFalseIfEquivalent=true" />
</runtime>
```

Se seu aplicativo for direcionado ao NET Framework 4.6.1 ou anterior e executado no .NET Framework 4.6.2 ou posterior e você quiser que essa alteração seja habilitada, defina a opção de compatibilidade como `false` adicionando o seguinte valor ao arquivo app.config:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.MemberDescriptorEqualsReturnsFalseIfEquivalent=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.ComponentModel.MemberDescriptor.Equals(System.Object)?displayProperty=nameWithType>
