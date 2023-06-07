---
ms.openlocfilehash: d25c14f93da5fe8acf06269554fed30ddc6bc95d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614289"
---
### <a name="operationcontextcurrent-may-return-null-when-called-in-a-using-clause"></a>OperationContext.Current pode retornar nulo quando chamado usando cláusula

#### <a name="details"></a>Detalhes

<xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> poderá retornar `null` e um <xref:System.NullReferenceException> poderá ocorrer quando todas as seguintes condições forem verdadeiras:

- Você recupera o valor da propriedade <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> em um método que retorna um <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601>.
- Você cria uma instância do objeto <xref:System.ServiceModel.OperationContextScope> em uma cláusula `using`.
- Você recupera o valor da propriedade <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> dentro de `using statement`. Por exemplo:

```csharp
using (new OperationContextScope(OperationContext.Current))
{
    // OperationContext.Current is null.
    OperationContext context = OperationContext.Current;

    // ...
}
```

#### <a name="suggestion"></a>Sugestão

Para solucionar esse problema, você pode fazer o seguinte:

- Modifique seu código da seguinte maneira para instanciar um novo não `null` <xref:System.ServiceModel.OperationContext.Current%2A> objeto:

    ```csharp
    OperationContext ocx = OperationContext.Current;
    using (new OperationContextScope(OperationContext.Current))
    {
        OperationContext.Current = new OperationContext(ocx.Channel);

        // ...
    }
    ```

- Instalar a atualização mais recente do .NET Framework 4.6.2 ou atualizar para uma versão posterior do .NET Framework. Isso desabilita o fluxo do <xref:System.Threading.ExecutionContext> em <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType> e restaura o comportamento de aplicativos do WCF no .NET Framework 4.6.1 e em versões anteriores. Esse comportamento é configurável; é equivalente a adicionar a seguinte configuração de aplicativo ao arquivo de configuração:

    ```xml
    <appSettings>
      <add key="Switch.System.ServiceModel.DisableOperationContextAsyncFlow" value="true" />
    </appSettings>
    ```

    Se essa alteração for indesejável e seu aplicativo depender do fluxo do contexto de execução entre contextos de operação, você poderá habilitar o fluxo da seguinte maneira:

    ```xml
    <appSettings>
      <add key="Switch.System.ServiceModel.DisableOperationContextAsyncFlow" value="false" />
    </appSettings>
    ```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.6.2       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.ServiceModel.OperationContext.Current?displayProperty=nameWithType>
