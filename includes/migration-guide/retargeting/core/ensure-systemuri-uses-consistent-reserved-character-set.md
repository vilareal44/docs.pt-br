---
ms.openlocfilehash: 21921156295d89aad04f3197fef9fa322f3c8c87
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614274"
---
### <a name="ensure-systemuri-uses-a-consistent-reserved-character-set"></a>Verifique se o System.URI usa um conjunto consistente de caracteres reservados

#### <a name="details"></a>Detalhes

No <xref:System.Uri?displayProperty=fullName>, determinados caracteres codificados por porcentagem que, às vezes, eram decodificados agora permanecem codificados de forma consistente. Isso ocorre nas propriedades e nos métodos que acessam os componentes de caminho, consulta, fragmento ou informações do usuário do URI. O comportamento será alterado somente quando os dois itens a seguir forem verdadeiros:

- O URI contiver a forma codificada de um dos seguintes caracteres reservados: `:`, `'`, `(`, `)`, `!` ou `*`.
- O URI contiver um caractere Unicode ou não reservado codificado. Se ambas as condições acima forem verdadeiras, os caracteres reservados codificados serão deixados codificados. Nas versões anteriores do .NET Framework, eles são decodificados.

#### <a name="suggestion"></a>Sugestão

Para aplicativos direcionados a versões do .NET Framework começando com a 4.7.2, o novo comportamento de decodificação é habilitado por padrão. Se essa alteração não for desejada, você poderá desabilitá-la adicionando a seguinte opção [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção `<runtime>` do arquivo de configuração de aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets=true" />
</runtime>
```

Para aplicativos direcionados a versões anteriores do .NET Framework, mas executados em versões começando com o .NET Framework 4.7.2, o novo comportamento de decodificação é desabilitado por padrão. Você pode habilitá-lo adicionando o seguinte comutador [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à `<runtime>` seção do arquivo de configuração do aplicativo:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.Uri.DontEnableStrictRFC3986ReservedCharacterSets=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Uri?displayProperty=nameWithType>
