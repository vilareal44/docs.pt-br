---
title: Serialização de caracteres de controle com DataContractJsonSerializer
description: Saiba como a serialização de caracteres de controle foi alterada para estar em conformidade com ECMAScript V6 e V8 no .NET Framework 4,7.
ms.date: 04/07/2017
helpviewer_keywords:
- .NET Framework 4.7 retargeting changes
- retargeting changes
- DataContractJsonSerializer changes
- serialization changes
ms.assetid: e065d458-a128-44f2-9f17-66af9d5be954
ms.openlocfilehash: a317884da014097a7a60aef2cef4ec146ccb04f7
ms.sourcegitcommit: cf5a800a33de64d0aad6d115ffcc935f32375164
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86475379"
---
# <a name="mitigation-serialization-of-control-characters-with-the-datacontractjsonserializer"></a>Mitigação: serialização de caracteres de controle com o DataContractJsonSerializer

A partir do .NET Framework 4,7, a maneira como os caracteres de controle são serializados com o <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> foi alterado para estar de acordo com o ECMAScript V6 e o V8.

## <a name="impact"></a>Impacto

No .NET Framework 4.6.2 e em versões anteriores, o não <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> serializava alguns caracteres de controle especiais, como `\b` , `\f` e `\t` , de uma forma que era compatível com os padrões do ECMAScript V6 e do V8.

Para aplicativos que visam versões do .NET Framework a partir do .NET Framework 4,7, a serialização desses caracteres de controle é compatível com ECMAScript V6 e V8. As seguintes APIs são afetadas:

- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer.WriteObject%2A>

## <a name="mitigation"></a>Atenuação

Para aplicativos que visam versões do .NET Framework a partir do .NET Framework 4,7, esse comportamento é habilitado por padrão.

Se esse comportamento não for desejado, você poderá recusar esse recurso adicionando a seguinte linha na seção `<runtime>` do arquivo app.config ou web.config:

```xml
<runtime>
   <AppContextSwitchOverrides value="Switch.System.Runtime.Serialization.DoNotUseECMAScriptV6EscapeControlCharacter=false" />
</runtime>
```

## <a name="see-also"></a>Consulte também

- [Compatibilidade de aplicativos](application-compatibility.md)
