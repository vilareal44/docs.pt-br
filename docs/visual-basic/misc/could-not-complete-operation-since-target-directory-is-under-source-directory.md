---
title: Não foi possível concluir a operação porque o diretório de destino está sob o diretório de origem
ms.date: 07/20/2015
f1_keywords:
- vbrIO_CyclicOperation
ms.assetid: 850d3a24-5d51-4ac8-a912-630efcd75278
ms.openlocfilehash: 46ec7ae452d4f8259d0f8ca3a896d1b29151ed61
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84376736"
---
# <a name="could-not-complete-operation-since-target-directory-is-under-source-directory"></a>Não foi possível concluir a operação porque o diretório de destino está sob o diretório de origem
Falha em uma operação cíclica. Ciclo de operações cíclicas e, portanto, não pode ser concluído. Por exemplo, o objeto A pode tentar herdar do objeto B, que por sua vez é herdado do objeto A.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Ao herdar, verifique se não há referências cíclicas.  
  
## <a name="see-also"></a>Confira também

- [Tipos de erro](../programming-guide/language-features/error-types.md)
- [Usar pontos de interrupção no depurador do Visual Studio](/visualstudio/debugger/using-breakpoints)
