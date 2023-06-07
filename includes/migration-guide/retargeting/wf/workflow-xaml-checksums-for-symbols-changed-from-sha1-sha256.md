---
ms.openlocfilehash: 946e71f2f466664c8f9fcf4811288ce693a872eb
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617778"
---
### <a name="workflow-xaml-checksums-for-symbols-changed-from-sha1-to-sha256"></a>As somas de verificação XAML do Fluxo de Trabalho para símbolos mudaram de SHA1 para SHA256

#### <a name="details"></a>Detalhes

Para compatibilidade com a depuração com o Visual Studio, o runtime de fluxo de trabalho gera uma soma de verificação para um arquivo XAML do fluxo de trabalho usando um algoritmo de hash. No .NET Framework 4.6.2 e versões anteriores, o hash de soma de verificação do fluxo de trabalho usava o algoritmo MD5, o que causou problemas em sistemas habilitados para FIPS. Começando com o .NET Framework 4.7, o algoritmo padrão mudou para SHA1. Começando com o .NET Framework 4.8, o algoritmo padrão mudou para SHA256.

#### <a name="suggestion"></a>Sugestão

Se o seu código não puder carregar instâncias de fluxo de trabalho ou encontrar os símbolos apropriados devido a uma falha de soma de verificação, tente definir a `AppContext` opção "Switch.System. Activities. UseSHA1HashForDebuggerSymbols "to `true` . No código:

```csharp
System.AppContext.SetSwitch("Switch.System.Activities.UseSHA1HashForDebuggerSymbols", true);
```

Ou em nossa configuração:

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Activities.UseSHA1HashForDebuggerSymbols=true" />
  </runtime>
</configuration>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.8         |
| Type    | Redirecionando |
