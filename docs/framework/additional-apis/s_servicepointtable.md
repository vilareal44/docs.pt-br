---
title: Campo ServicePointManager. s_ServicePointTable
description: Leia sobre o campo ServicePointManager. s_ServicePointTable no .NET. Este campo de tabela de hash contém conexões HTTP ativas (pontos de bits) no AppDomain.
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ServicePointManager.s_ServicePointTable
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 24459679-291c-401a-9def-e42b29466fcf
ms.openlocfilehash: 9462ae10125dd37706f786a1f2cef78e62fbabcc
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989541"
---
# <a name="servicepointmanagers_servicepointtable-field"></a>Campo ServicePointManager. s do \_ Objectpointtable

`ServicePointManager.s_ServicePointTable`é um <xref:System.Collections.Hashtable> que contém a lista de conexões http ativas <xref:System.Net.ServicePoint> no <xref:System.AppDomain> .

## <a name="syntax"></a>Syntax
  
```csharp  
private static Hashtable s_ServicePointTable
```

> [!WARNING]
> O `ServicePointManager.s_ServicePointTable` campo é privado e não deve ser usado diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso deste campo em um aplicativo de produção em qualquer circunstância.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)

**.NET Framework versões:** Disponível desde 2,0.
