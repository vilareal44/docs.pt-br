---
title: Campo CoreResponseData. m_ResponseHeaders
description: Entenda o campo CoreResponseData. m_ResponseHeaders no .NET. Este campo é um tipo webheadcollection que tem cabeçalhos associados à resposta do servidor.
ms.date: 01/29/2018
topic_type:
- apiref
api_name:
- System.Net.CoreResponseData.m_ResponseHeaders
api_location:
- System.dll
api_type:
- Assembly
author: stevewhims
ms.openlocfilehash: 7c7b896193cb81e9fc9e3ec28110359003a36728
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989786"
---
# <a name="coreresponsedatam_responseheaders-field"></a>Campo ResponseHeaders CoreResponseData. m \_

`CoreResponseData.m_ResponseHeaders`é um <xref:System.Net.WebHeaderCollection> dos cabeçalhos associados à resposta do servidor.

## <a name="syntax"></a>Syntax
  
```csharp
public WebHeaderCollection m_ResponseHeaders
```

> [!WARNING]
> Essa API não deve ser usada diretamente no seu código. Em vez disso, você deve usar um <xref:System.Diagnostics.DiagnosticSource> para conectar o código de rede. Consulte o [Guia do usuário do diagnosticm](https://github.com/dotnet/runtime/blob/master/src/libraries/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md).
>
> A Microsoft não oferece suporte ao uso dessa classe em um aplicativo de produção em nenhuma circunstância.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)

**.NET Framework versões:** Disponível desde 2,0.
