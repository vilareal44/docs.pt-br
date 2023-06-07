---
title: Classe HttpStatusDescription (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.HttpStatusDescription
- System.Net.HttpStatusDescription.Get
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: 0214d8259c735de11abe204680d619a7298466de
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990303"
---
# <a name="httpstatusdescription-class"></a>Classe HttpStatusDescription

Fornece descrições de status HTTP padrão. Esta classe não pode ser herdada.

```csharp
internal static class HttpStatusDescription
```

> [!WARNING]
> Essa classe é interna e não deve ser usada diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="get-method"></a>método Get

Retorna a descrição associada ao código de status HTTP especificado.

```csharp
internal static string Get(int code)
```

### <a name="parameters"></a>Parâmetros

`code` <xref:System.Int32>

O código de status HTTP, por exemplo, `404` .

### <a name="return-value"></a>Valor retornado

<xref:System.String?displayProperty=nameWithType>

A descrição do status HTTP.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
