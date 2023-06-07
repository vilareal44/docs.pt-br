---
title: Campo de ponto de extremidade. m_ConnectionGroupList
description: Entenda o campo do ponto. m_ConnectionGroupList, uma tabela de hash de grupos de conexão que cada um tem uma conexão para o URI do ponto de extremidade no .NET.
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ServicePoint.m_ConnectionGroupList
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: df8afb59-f0f6-4ddc-b3c1-839b9fc601d8
ms.openlocfilehash: 0ebfeb782147f21abfde536b8053fa15b1e1a602
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989706"
---
# <a name="servicepointm_connectiongrouplist-field"></a>Campo ConnectionGroupList do ponto. m \_

`ServicePoint.m_ConnectionGroupList`é um <xref:System.Collections.Hashtable> dos grupos de conexão, cada um mantendo uma conexão para o <xref:System.Net.ServicePoint> URI do.

## <a name="syntax"></a>Syntax
  
```csharp  
private Hashtable m_ConnectionGroupList
```

> [!WARNING]
> O `ServicePoint.m_ConnectionGroupList` campo é privado e não deve ser usado diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso deste campo em um aplicativo de produção em qualquer circunstância.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)

**.NET Framework versões:** Disponível desde 2,0.
