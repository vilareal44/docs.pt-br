---
title: Campo Connection. m_ConnectionList
description: Saiba mais sobre o campo Connection. m_ConnectionList no .NET, que contém objetos de conexão que servem o mesmo URI e compartilham valores para outras propriedades.
ms.date: 05/01/2017
topic_type:
- apiref
api_name:
- System.Net.ConnectionGroup.m_ConnectionList
api_location:
- System.dll
api_type:
- Assembly
ms.assetid: 186083cf-8dff-4600-a2ab-6fed4b4de6af
ms.openlocfilehash: 478b2441c062e8df6f4e718bd66d7af329f20f12
ms.sourcegitcommit: 45c8eed045779b70a47b23169897459d0323dc89
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/18/2020
ms.locfileid: "84989723"
---
# <a name="connectiongroupm_connectionlist-field"></a>Campo connectionlist do conjunto de conexões. m \_

`ConnectionGroup.m_ConnectionList`é um <xref:System.Collections.ArrayList> dos objetos de conexão que atendem ao mesmo URI e compartilham os mesmos valores para algumas outras propriedades, como expiração e autenticação.

## <a name="syntax"></a>Syntax
  
```csharp  
private ArrayList m_ConnectionList
```

> [!WARNING]
> O `ConnectionGroup.m_ConnectionList` campo é privado e não deve ser usado diretamente no seu código.
>
> A Microsoft não oferece suporte ao uso deste campo em um aplicativo de produção em qualquer circunstância.

## <a name="requirements"></a>Requisitos

**Namespace:** <xref:System.Net>

**Assembly:** Sistema (em System.dll)

**.NET Framework versões:** Disponível desde 2,0.
