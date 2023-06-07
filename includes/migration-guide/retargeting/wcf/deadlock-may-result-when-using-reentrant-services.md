---
ms.openlocfilehash: dd7d3e445772e4b5ec148576ccd1374d56e251bd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614294"
---
### <a name="deadlock-may-result-when-using-reentrant-services"></a>Uso de serviços Reentrantes pode resultar em deadlock

#### <a name="details"></a>Detalhes

Um deadlock pode resultar em um serviço Reentrante, o que restringe as instâncias do serviço para um thread de execução de cada vez. Os serviços que podem apresentar esse problema terão o <xref:System.ServiceModel.ServiceBehaviorAttribute> a seguir no código:

```csharp
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Reentrant)]
```

#### <a name="suggestion"></a>Sugestão

Para solucionar esse problema, você pode fazer o seguinte:

- Definir o modo de simultaneidade do serviço como <xref:System.ServiceModel.ConcurrencyMode.Single?displayProperty=nameWithType> ou &lt;System.ServiceModel.ConcurrencyMode.Multiple?displayProperty=nameWithType&gt;. Por exemplo:

```csharp
[ServiceBehavior(ConcurrencyMode = ConcurrencyMode.Reentrant)]
```

- Instalar a atualização mais recente do .NET Framework 4.6.2 ou atualizar para uma versão posterior do .NET Framework. Isso desabilita o fluxo do <xref:System.Threading.ExecutionContext> em <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType>. Esse comportamento é configurável; é equivalente a adicionar a seguinte configuração de aplicativo ao arquivo de configuração:

```xml
<appSettings>
  <add key="Switch.System.ServiceModel.DisableOperationContextAsyncFlow" value="true" />
</appSettings>
```

O valor de `Switch.System.ServiceModel.DisableOperationContextAsyncFlow` nunca deve ser definido como `false` para serviços Reentrant.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.ServiceModel.ServiceBehaviorAttribute?displayProperty=nameWithType>
- <xref:System.ServiceModel.ConcurrencyMode.Reentrant?displayProperty=nameWithType>
