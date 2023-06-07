---
title: Método ServicePointManager. CloseConnectionGroups (System.Net)
ms.date: 06/12/2020
ms.technology: dotnet-networking
topic_type:
- apiref
api_name:
- System.Net.ServicePointManager.CloseConnectionGroups
api_location:
- System.dll
api_type:
- Assembly
ms.openlocfilehash: aae9a389ae35e249d43c9dc830a68ec32cf4afef
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990295"
---
# <a name="servicepointmanagercloseconnectiongroups-method"></a>Método ServicePointManager. CloseConnectionGroups

Itera em todos os pontos de serviço e fecha os grupos de conexão que têm o nome especificado.

```csharp
internal static void CloseConnectionGroups(string connectionGroupName)
```

> [!WARNING]
> Esse método é interno e não deve ser usado diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso desse método em um aplicativo de produção em qualquer circunstância.

## <a name="parameters"></a>Parâmetros

`connectionGroupName` <xref:System.String>

O nome do grupo de conexões a ser fechado.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)
