---
title: Modulo<keyword>
ms.date: 07/20/2015
f1_keywords:
- vb.ModuleAttribute
helpviewer_keywords:
- Module keyword [Visual Basic]
- Module modifier
- attribute blocks, Module keyword
ms.assetid: d971b940-05ab-4d56-8485-e3b8a661906b
ms.openlocfilehash: 0cb009c22dada7b92956e113d33505923a92f2b3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84362418"
---
# <a name="module-keyword-visual-basic"></a>Módulo \<keyword> (Visual Basic)
Especifica que um atributo no início de um arquivo de origem se aplica ao módulo do assembly atual.  
  
## <a name="remarks"></a>Comentários  
 Muitos atributos pertencem a um elemento de programação individual, como uma classe ou propriedade. Você aplica esse atributo, anexando o bloco de atributo, entre colchetes angulares ( `< >` ), diretamente à instrução de declaração.  
  
 Se um atributo pertencer não apenas ao elemento a seguir, mas ao módulo do assembly atual, você coloca o bloco de atributo no início do arquivo de origem e identifica o atributo com a `Module` palavra-chave. Se ele se aplicar a todo o assembly, você usará a palavra-chave [assembly](assembly.md) .  
  
 O `Module` modificador não é o mesmo que a [instrução do módulo](../statements/module-statement.md).  
  
## <a name="see-also"></a>Confira também

- [Assembly](assembly.md)
- [Instrução Module](../statements/module-statement.md)
- [Visão geral de atributos](../../programming-guide/concepts/attributes/index.md)
