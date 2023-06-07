---
title: "'ReDim' só pode alterar a dimensão mais à direita"
ms.date: 07/20/2015
f1_keywords:
- vbrArray_TypeMismatch
ms.assetid: d53cf41b-7a7a-466c-a29a-920d99698fa9
ms.openlocfilehash: 8e42e0df7b97fb96f468ce37dd4cfc52fad8c596
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84358290"
---
# <a name="redim-can-only-change-the-right-most-dimension"></a>'ReDim' só pode alterar a dimensão mais à direita
Uma `ReDim` instrução tentou usar a `Preserve` palavra-chave para alterar uma dimensão de uma matriz que não é a última dimensão. Ao usar `Preserve` o, você pode redimensionar apenas a última dimensão de uma matriz. Para todas as outras dimensões, você deve especificar o mesmo tamanho de para a matriz existente.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Remova a `Preserve` palavra-chave.  
  
## <a name="see-also"></a>Confira também

- [Matrizes no Visual Basic](../programming-guide/language-features/arrays/index.md)
- [Dimensões de matriz no Visual Basic](../programming-guide/language-features/arrays/array-dimensions.md)
- [Instrução ReDim](../language-reference/statements/redim-statement.md)
- [Instrução Dim](../language-reference/statements/dim-statement.md)
