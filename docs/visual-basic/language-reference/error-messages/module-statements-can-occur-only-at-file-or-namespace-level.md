---
title: Instruções 'Module' só podem ocorrer no nível de namespace ou arquivo
ms.date: 07/20/2015
f1_keywords:
- bc30617
- vbc30617
helpviewer_keywords:
- BC30617
ms.assetid: 5e9de8e5-d26b-4fb2-9e28-814413fe9cef
ms.openlocfilehash: d01c30571fc34e142300ac8706c56d5e99175fcf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397201"
---
# <a name="module-statements-can-occur-only-at-file-or-namespace-level"></a>Instruções 'Module' só podem ocorrer no nível de namespace ou arquivo
`Module`as instruções devem aparecer na parte superior do arquivo de origem imediatamente após `Option` e `Imports` instruções, atributos globais e declarações de namespace, mas antes de todas as outras declarações.  
  
 **ID do erro:** BC30617  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Mova a `Module` instrução para a parte superior da sua declaração de namespace ou arquivo de origem.  
  
## <a name="see-also"></a>Confira também

- [Instrução Module](../statements/module-statement.md)
