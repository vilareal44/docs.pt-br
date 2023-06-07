---
ms.openlocfilehash: b893966ceb65246ed6a7d214011b17ca14c50d5a
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85802839"
---

> [!WARNING]
> Ao usar <xref:System.Text.RegularExpressions> o para processar a entrada não confiável, passe um tempo limite. Um usuário mal-intencionado pode fornecer entrada para `RegularExpressions` causar um [ataque de negação de serviço](https://www.us-cert.gov/ncas/tips/ST04-015). ASP.NET Core APIs do Framework que usam `RegularExpressions` passar um tempo limite.
