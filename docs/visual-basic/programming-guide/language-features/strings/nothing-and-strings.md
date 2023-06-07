---
title: Nothing e cadeias de caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], Nothing
ms.assetid: 261380af-2024-4ecf-823b-43b1034d92cd
ms.openlocfilehash: 392de45b0ee1688224f3e8170b0144f1acdb0912
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360560"
---
# <a name="nothing-and-strings-in-visual-basic"></a>Nada e cadeias de caracteres no Visual Basic
O tempo de execução Visual Basic e o .NET Framework são avaliados de `Nothing` maneira diferente quando se trata de cadeias de caracteres.  
  
## <a name="visual-basic-runtime-and-the-net-framework"></a>Tempo de execução de Visual Basic e o .NET Framework  
 Considere o exemplo a seguir:  
  
 [!code-vb[VbVbalrStrings#47](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#47)]  
  
 O tempo de execução de Visual Basic geralmente é avaliado `Nothing` como uma cadeia de caracteres vazia (""). O .NET Framework não, no entanto, e gera uma exceção sempre que é feita uma tentativa de executar uma operação de cadeia de caracteres no `Nothing` .  
  
## <a name="see-also"></a>Confira também

- [Introdução a cadeias de caracteres no Visual Basic](introduction-to-strings.md)
