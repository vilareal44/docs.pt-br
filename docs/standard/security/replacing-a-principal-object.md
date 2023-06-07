---
title: Substituindo um objeto Principal
ms.date: 07/15/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- principal objects, replacing
- security [.NET], replacing principal objects
- security [.NET], principals
ms.assetid: c323687e-b196-487b-beba-f38f9b3f961b
ms.openlocfilehash: fced91aef991bc228e65128d5e8d69e6a46588e5
ms.sourcegitcommit: b7a8b09828bab4e90f66af8d495ecd7024c45042
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/04/2020
ms.locfileid: "87557093"
---
# <a name="replacing-a-principal-object"></a>Substituindo um objeto Principal

Os aplicativos que fornecem serviços de autenticação devem ser capazes de substituir o objeto **principal** ( <xref:System.Security.Principal.IPrincipal> ) para um determinado thread. Além disso, o sistema de segurança deve ajudar a proteger a capacidade de substituir os objetos **principais** , pois uma **entidade** de segurança incorreta, comprometida de forma mal-intencionada, compromete o seu aplicativo reivindicando uma identidade ou função inverdadeira. Portanto, os aplicativos que exigem a capacidade de substituir objetos de **entidade** devem receber o <xref:System.Security.Permissions.SecurityPermission?displayProperty=nameWithType> objeto para o controle principal. (Observe que essa permissão não é necessária para executar verificações de segurança baseadas em função ou para a criação de objetos **principal** .)  
  
O objeto **principal** atual pode ser substituído executando as seguintes tarefas:  
  
1. Crie o objeto **principal** de substituição e o objeto de **identidade** associado.  
  
2. Anexe o novo objeto **principal** ao contexto de chamada.  
  
## <a name="example"></a>Exemplo

O exemplo a seguir mostra como criar um objeto principal genérico e usá-lo para definir a entidade de segurança de um thread.  
  
[!code-csharp[SetCurrentPrincipal#1](../../../samples/snippets/csharp/VS_Snippets_CLR/SetCurrentPrincipal/CS/program.cs#1)]
[!code-vb[SetCurrentPrincipal#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/SetCurrentPrincipal/VB/program.vb#1)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Security.Permissions.SecurityPermission?displayProperty=nameWithType>
- [Objetos Principal e Identity](principal-and-identity-objects.md)
- [Segurança de ASP.NET Core](/aspnet/core/security/)
