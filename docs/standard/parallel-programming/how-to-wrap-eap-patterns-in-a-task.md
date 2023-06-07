---
title: 'Como: encapsular padrões de EAP em uma tarefa'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, how to wrap EAP patterns
ms.assetid: f11ed467-af2f-4504-8a2e-299a6c36d44e
ms.openlocfilehash: eab94ac91be0c755a1da74e2f2220e3b76cc4249
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290779"
---
# <a name="how-to-wrap-eap-patterns-in-a-task"></a>Como: encapsular padrões de EAP em uma tarefa
O exemplo a seguir mostra como expor uma sequência arbitrária de operações de padrão assíncrono baseado em evento (EAP) como uma tarefa usando um <xref:System.Threading.Tasks.TaskCompletionSource%601>. O exemplo também mostra como usar um <xref:System.Threading.CancellationToken> para invocar os métodos de cancelamento internos nos objetos <xref:System.Net.WebClient>.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[FromAsync#08](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#08)]
 [!code-vb[FromAsync#08](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#08)]  
  
## <a name="see-also"></a>Confira também

- [TPL e programação assíncrona do .NET Framework](tpl-and-traditional-async-programming.md)
