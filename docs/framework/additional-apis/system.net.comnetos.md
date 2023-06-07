---
title: Classe ComNetOS (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.ComNetOS
- System.Net.ComNetOS.IsWin7orLater
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: ed2b970d07df2c338870b386e75c1688703f1d68
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990316"
---
# <a name="comnetos-class"></a>Classe ComNetOS

Fornece informações sobre o sistema operacional atual, como sua versão e o tipo de instalação (cliente ou servidor). Esta classe não pode ser herdada.
  
```csharp  
internal static class ComNetOS
```

> [!WARNING]
> Essa classe é interna e não deve ser usada diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="iswin7orlater-field"></a>Campo IsWin7orLater

Especifica se a versão do sistema operacional é o Windows 7 ou posterior.

```csharp
internal static readonly bool IsWin7orLater
```

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
