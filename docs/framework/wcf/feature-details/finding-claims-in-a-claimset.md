---
title: Encontrando declarações em um ClaimSet
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- claims [WCF], finding in a claimset
- claims [WCF]
ms.assetid: a76ce107-aeb3-47d0-bfa9-134c53664e20
ms.openlocfilehash: 1954da20d382630f965582fcc01bbaf72665720a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595447"
---
# <a name="finding-claims-in-a-claimset"></a>Encontrando declarações em um ClaimSet
Examinar o conteúdo de um <xref:System.IdentityModel.Claims.ClaimSet> para tipos específicos de declarações é uma tarefa comum ao usar a autorização baseada em declarações. Para examinar um <xref:System.IdentityModel.Claims.ClaimSet> para a presença de declarações específicas, use o <xref:System.IdentityModel.Claims.ClaimSet.FindClaims%2A> método. Esse método fornece melhor desempenho do que iterar diretamente sobre o <xref:System.IdentityModel.Claims.ClaimSet> . O exemplo a seguir demonstra esse uso. Observe que os `claimType` `claimRight` parâmetros e podem ser `null` . Nesse caso, os parâmetros corresponderão a todos os tipos de declaração e direitos de declaração.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[c_FindClaimsPerf#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_findclaimsperf/cs/c_findclaimsperf.cs#2)]
 [!code-vb[c_FindClaimsPerf#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_findclaimsperf/vb/c_findclaimsperf.vb#2)]  
  
## <a name="see-also"></a>Confira também

- [Gerenciamento de declarações e autorizações com o modelo de identidade](managing-claims-and-authorization-with-the-identity-model.md)
