---
ms.openlocfilehash: 53d74db1a77e62cc64250658281fd3e4706fe494
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614269"
---
### <a name="allow-unicode-bidirectional-control-characters-in-uris"></a>Permitir caracteres de controle bidirecionais Unicode em URIs

#### <a name="details"></a>Detalhes

O Unicode especifica vários caracteres de controle especiais usados para especificar a orientação do texto. Nas versões anteriores do .NET Framework, esses caracteres eram excluídos incorretamente de todos os URIs mesmo quando estavam presentes em sua forma codificada por porcentagem. Para atender melhor ao [RFC 3987](https://tools.ietf.org/html/rfc3987), agora esses caracteres são permitidos nos URIs. Quando encontrados sem codificação em um URI, eles são codificados por porcentagem. Quando encontrados codificados por porcentagem, eles são deixados no estado em que se encontram.

#### <a name="suggestion"></a>Sugestão

Para aplicativos direcionados a versões do .NET Framework começando com a 4.7.2, o suporte para caracteres bidirecionais Unicode é habilitado por padrão. Se essa alteração não for desejada, você poderá desabilitá-la adicionando a seguinte opção [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção `<runtime>` do arquivo de configuração de aplicativo:

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Uri.DontKeepUnicodeBidiFormattingCharacters=true" />
</runtime>
```

Para aplicativos direcionados a versões anteriores do .NET Framework, mas executados em versões começando com o .NET Framework 4.7.2, o suporte para caracteres bidirecionais Unicode é desabilitado por padrão. Você poderá habilitá-la adicionando a seguinte opção [AppContextSwitchOverrides](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção `<runtime>` do arquivo de configuração de aplicativo:

```xml
<runtime>
<AppContextSwitchOverrides value="Switch.System.Uri.DontKeepUnicodeBidiFormattingCharacters=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Uri?displayProperty=nameWithType>
