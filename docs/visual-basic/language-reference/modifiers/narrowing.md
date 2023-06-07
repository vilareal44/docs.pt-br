---
title: Narrowing
ms.date: 07/20/2015
f1_keywords:
- vb.narrowing
helpviewer_keywords:
- conversions [Visual Basic], type
- type conversion [Visual Basic]
- conversions [Visual Basic], data type
- Narrowing keyword [Visual Basic]
- data type conversion [Visual Basic]
ms.assetid: a207ee91-aca4-4771-b4e2-713f029bf2bb
ms.openlocfilehash: f7724053e3732c909523e4e2d3b65bb1918c29d3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84362353"
---
# <a name="narrowing-visual-basic"></a>Narrowing (Visual Basic)
Indica que um operador de conversão ( `CType` ) converte uma classe ou estrutura em um tipo que pode não ser capaz de conter alguns dos valores possíveis da classe ou estrutura original.  
  
## <a name="converting-with-the-narrowing-keyword"></a>Convertendo com a palavra-chave Narrowing  
 O procedimento de conversão deve especificar, `Public Shared` além de `Narrowing` .  
  
 As conversões redutoras nem sempre são bem sucedidas em tempo de execução e podem falhar ou incorrer em perda de dados. Os exemplos são `Long` to, `Integer` `String` to `Date` e um tipo base para um tipo derivado. Essa última conversão é restrita porque o tipo base pode não conter todos os membros do tipo derivado e, portanto, não é uma instância do tipo derivado.  
  
 Se `Option Strict` for `On` , o código de consumo deve usar `CType` para todas as conversões redutoras.  
  
 A `Narrowing` palavra-chave pode ser usada neste contexto:  
  
 [Instrução Operator](../statements/operator-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Instrução Operator](../statements/operator-statement.md)
- [Widening](widening.md)
- [Conversões de Widening e Narrowing](../../programming-guide/language-features/data-types/widening-and-narrowing-conversions.md)
- [Como definir um operador](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [Função CType](../functions/ctype-function.md)
- [Instrução Option Strict](../statements/option-strict-statement.md)
