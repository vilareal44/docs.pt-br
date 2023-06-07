---
ms.openlocfilehash: e66a5c2a33a4ab5cf35013c1843936ffd01f90a2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614288"
---
### <a name="serialport-background-thread-exceptions"></a>Exceções de thread de tela de fundo de SerialPort

#### <a name="details"></a>Detalhes

Threads em segundo plano criados com fluxos <xref:System.IO.Ports.SerialPort> não terminam o processo quando são geradas exceções do sistema operacional. <br/>Em aplicativos direcionados ao .NET Framework 4.7 e a versões anteriores, um processo é terminado quando uma exceção do sistema operacional é gerada em um thread em segundo plano criado com um fluxo <xref:System.IO.Ports.SerialPort>. <br/>Nos aplicativos direcionados ao .NET Framework 4.7.1 e a versões anteriores, os threads em segundo plano esperam pelos eventos do SO relacionados à porta serial ativa e podem falhar em alguns casos, como uma remoção repentina da porta serial.

#### <a name="suggestion"></a>Sugestão

Para aplicativos destinados ao .NET Framework 4.7.1, será possível recusar a manipulação de exceções se ela não for desejável adicionando o seguinte à seção `<runtime>` do arquivo `app.config`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Ports.DoNotCatchSerialStreamThreadExceptions=true" />
</runtime>
```

Para aplicativos destinados a versões anteriores do .NET Framework mas executados no .NET Framework 4.7.1 ou posterior, é possível aceitar a manipulação de exceções adicionando o seguinte à seção `<runtime>` do arquivo `app.config`:

```xml
<runtime>
  <AppContextSwitchOverrides value="Switch.System.IO.Ports.DoNotCatchSerialStreamThreadExceptions=false" />
</runtime>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.IO.Ports.SerialPort?displayProperty=nameWithType>
