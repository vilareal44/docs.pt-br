---
title: "'Optional' esperado"
ms.date: 07/20/2015
f1_keywords:
- bc30202
- vbc30202
helpviewer_keywords:
- BC30202
ms.assetid: 6f75060c-2db4-4a79-b5d1-5780c09a74cd
ms.openlocfilehash: 411e3248409ab0184666f4efefb4ec4becf7cab1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409342"
---
# <a name="optional-expected"></a>'Optional' esperado
Um argumento opcional em uma declaração de procedimento é seguido por um argumento necessário. Todos os argumentos após um argumento opcional também devem ser opcionais.  
  
 **ID do erro:** BC30202  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Se o argumento se destinar a ser necessário, mova-o para preceder o primeiro argumento opcional na lista de argumentos.  
  
2. Se o argumento se destinar a ser opcional, use a `Optional` palavra-chave.  
  
## <a name="see-also"></a>Confira também

- [Parâmetros Opcionais](../../programming-guide/language-features/procedures/optional-parameters.md)
