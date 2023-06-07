---
title: Método Exception. PrepForRemoting (sistema)
description: Examine o método Exception. PrepForRemoting no .NET. O método adiciona o rastreamento de pilha do lado do servidor à mensagem antes que a exceção seja relançada no cliente.
ms.date: 10/08/2019
topic_type:
- apiref
api_name:
- System.Exception.PrepForRemoting
api_location:
- mscorlib.dll
api_type:
- Assembly
ms.openlocfilehash: 9ceb73499ae3bb308975e6db5b961bfe40165ba3
ms.sourcegitcommit: 1c37a894c923bea021a3cc38ce7cba946357bbe1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/19/2020
ms.locfileid: "85105260"
---
# <a name="exceptionprepforremoting-method"></a>Método Exception.PrepForRemoting

Preserva o rastreamento de pilha do lado do servidor anexando-o à mensagem antes que a exceção seja relançada no site de chamada do cliente.

```csharp
internal Exception PrepForRemoting();
```

## <a name="returns"></a>Retornos

<xref:System.Exception>  
Esta instância <xref:System.Exception>.

## <a name="remarks"></a>Comentários

> [!WARNING]
> O `Exception.PrepForRemoting` método é interno e não deve ser usado diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso desse método em um aplicativo de produção em qualquer circunstância.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System>

**Assembly:** mscorlib.dll (em mscorlib.dll)

**.NET Framework versões:** Disponível desde 1,0.
