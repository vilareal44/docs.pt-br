---
title: Padrão
ms.date: 07/20/2015
f1_keywords:
- vb.Default
helpviewer_keywords:
- defaults, properties
- properties [Visual Basic], default
- default properties, in Visual Basic
- Default keyword [Visual Basic]
- default properties
ms.assetid: 45fce9b9-d212-4b2d-ab86-6e359b8b57af
ms.openlocfilehash: 0c2808795d6fcbad7892369fd7f460ebf0406093
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372966"
---
# <a name="default-visual-basic"></a>Padrão (Visual Basic)
Identifica uma propriedade como a propriedade padrão de sua classe, estrutura ou interface.  
  
## <a name="remarks"></a>Comentários  
 Uma classe, estrutura ou interface pode designar, no máximo, uma de suas propriedades como a *propriedade padrão*, desde que a propriedade aceite pelo menos um parâmetro. Se o código fizer uma referência a uma classe ou estrutura sem especificar um membro, Visual Basic resolverá essa referência à propriedade padrão.  
  
 As propriedades padrão podem resultar em uma pequena redução em caracteres de código-fonte, mas podem tornar seu código mais difícil de ler. Se o código de chamada não estiver familiarizado com sua classe ou estrutura, quando ele fizer uma referência ao nome da classe ou da estrutura, ele não poderá ter certeza se essa referência acessa a classe ou estrutura em si ou uma propriedade padrão. Isso pode levar a erros de compilador ou erros de lógica de tempo de execução sutis.  
  
 Você pode, de certa forma, reduzir a chance de erros de propriedade padrão sempre usando a [instrução Option Strict](../statements/option-strict-statement.md) para definir a verificação de tipo de compilador como `On` .  
  
 Se você estiver planejando usar uma classe ou estrutura predefinida em seu código, você deve determinar se ela tem uma propriedade padrão e, em caso afirmativo, qual é seu nome.  
  
 Devido a essas desvantagens, você deve considerar não definir propriedades padrão. Para legibilidade de código, você também deve considerar sempre referir-se a todas as propriedades explicitamente, até mesmo propriedades padrão.  
  
 O `Default` modificador pode ser usado neste contexto:  
  
 [Instrução Property](../statements/property-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Como declarar e chamar uma propriedade padrão no Visual Basic](../../programming-guide/language-features/procedures/how-to-declare-and-call-a-default-property.md)
- [Palavras-chave](../keywords/index.md)
