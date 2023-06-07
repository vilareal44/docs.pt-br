---
ms.openlocfilehash: 72d48d1daa85b6891c122f2fcc5279642253b926
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614302"
---
### <a name="workflow-checksums-changed-from-md5-to-sha1"></a>Somas de verificação de fluxo de trabalho alteradas de MD5 para SHA1

#### <a name="details"></a>Detalhes

Para compatibilidade com a depuração com o Visual Studio, o runtime de fluxo de trabalho gera uma soma de verificação para uma instância de fluxo de trabalho usando um algoritmo de hash. No .NET Framework 4.6.2 e versões anteriores, o hash de soma de verificação do fluxo de trabalho usava o algoritmo MD5, o que causou problemas em sistemas habilitados para FIPS. A partir do .NET Framework 4.7, o algoritmo será o SHA1. Se o código persistir a essas somas de verificação, eles serão incompatíveis.

#### <a name="suggestion"></a>Sugestão

Se o código for incapaz de carregar instâncias de fluxo de trabalho por causa de uma falha na soma de verificação, tente configurar a opção `AppContext`&quot;Switch.System.Activities.UseMD5ForWFDebugger&quot; com true. Em código:

```csharp
System.AppContext.SetSwitch("Switch.System.Activities.UseMD5ForWFDebugger", true);
```

Ou em nossa configuração:

```xml
<configuration>
  <runtime>
    <AppContextSwitchOverrides value="Switch.System.Activities.UseMD5ForWFDebugger=true" />
  </runtime>
</configuration>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7         |
| Type    | Redirecionando |
