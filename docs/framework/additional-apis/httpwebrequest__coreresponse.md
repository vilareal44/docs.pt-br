---
title: Campo HttpWebRequest. _CoreResponse
description: Leia sobre o campo HttpWebRequest. _CoreResponse no .NET. Esse campo é um objeto CoreResponseData ou Exception que contém o resultado da análise de resposta HTTP.
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.HttpWebRequest._CoreResponse
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 5093ec7ed2c3b94931dcd622ae9ccdb42feffa18
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989753"
---
# <a name="httpwebrequest_coreresponse-field"></a>HttpWebRequest. \_ Campo CoreResponse

`HttpWebRequest._CoreResponse`é um objeto (um [CoreResponseData](coreresponsedata.md) ou um <xref:System.Exception> ) que contém o resultado da análise de resposta http.

## <a name="syntax"></a>Syntax
  
```csharp
private object _CoreResponse
```

> [!WARNING]
> Essa API não deve ser usada diretamente no seu código. Em vez disso, você deve usar um <xref:System.Diagnostics.DiagnosticSource> para conectar o código de rede. Consulte o [Guia do usuário do diagnosticm](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)

**.NET Framework versões:** Disponível desde 2,0.
