---
title: "'ReDim' não pode alterar o número de dimensões"
ms.date: 07/20/2015
f1_keywords:
- vbrArray_RankMismatch
ms.assetid: 52505298-9985-4682-8f6e-ff7d56077f34
ms.openlocfilehash: b2404927c2faf30d1d058d2163fd5d67e1a52e1e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84358161"
---
# <a name="redim-cannot-change-the-number-of-dimensions"></a>'ReDim' não pode alterar o número de dimensões
Uma operação tenta usar a `ReDim` instrução para alterar a classificação (número de dimensões) de uma matriz. `ReDim`pode alterar o tamanho de uma ou mais dimensões de uma matriz que já foi formalmente declarada, mas não pode alterar a classificação de uma matriz.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Certifique-se de que você pretende alterar a classificação da matriz e não os tamanhos de suas dimensões e, se possível, use `Dim` para declarar uma nova matriz com a classificação desejada.  
  
## <a name="see-also"></a>Confira também

- [Matrizes no Visual Basic](../programming-guide/language-features/arrays/index.md)
- [Dimensões de matriz no Visual Basic](../programming-guide/language-features/arrays/array-dimensions.md)
- [Instrução ReDim](../language-reference/statements/redim-statement.md)
- [Instrução Dim](../language-reference/statements/dim-statement.md)
