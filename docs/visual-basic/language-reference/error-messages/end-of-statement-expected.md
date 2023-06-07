---
title: Fim de instrução esperado
ms.date: 07/20/2015
f1_keywords:
- bc30205
- vbc30205
helpviewer_keywords:
- BC30205
ms.assetid: 53c7f825-a737-4b76-a1fa-f67745b8bd40
ms.openlocfilehash: 169f01b02df377ba6cc21ffad53c36f5d4537140
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409641"
---
# <a name="end-of-statement-expected"></a>Fim de instrução esperado
A instrução está sintaticamente concluída, mas um elemento de programação adicional segue o elemento que conclui a instrução. Um terminador de linha é necessário no final de cada instrução.
  
 Um terminador de linha divide os caracteres de um Visual Basic arquivo de origem em linhas. Exemplos de terminadores de linha são o caractere de retorno de carro Unicode (&HD), o caractere de alimentação Unicode (&HA) e o caractere de retorno de carro Unicode seguido pelo caractere de alimentação de linha Unicode. Para obter mais informações sobre terminadores de linha, consulte a [especificação da linguagem Visual Basic](~/_vblang/spec/lexical-grammar.md#line-terminators).
  
 **ID do erro:** BC30205
  
## <a name="to-correct-this-error"></a>Para corrigir este erro
  
1. Verifique se duas instruções diferentes foram acidentalmente colocadas na mesma linha.
  
2. Insira um terminador de linha após o elemento que conclui a instrução.
  
## <a name="see-also"></a>Confira também

- [Como: Quebrar e combinar instruções no código](../../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
- [Instruções](../../programming-guide/language-features/statements.md)
