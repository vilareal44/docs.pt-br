---
title: Quando usar uma enumeração
ms.date: 07/20/2015
helpviewer_keywords:
- enumerations [Visual Basic]
ms.assetid: e6e47b5b-3ed9-452d-a481-9c3fed88519a
ms.openlocfilehash: ba69249e16b8c0ee06d57d06d192874a283b295e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403531"
---
# <a name="when-to-use-an-enumeration-visual-basic"></a>Quando usar uma enumeração (Visual Basic)
As enumerações oferecem uma maneira fácil de trabalhar com conjuntos de constantes relacionadas. Uma enumeração, ou `Enum` , é um nome simbólico para um conjunto de valores. As enumerações são tratadas como tipos de dados e você pode usá-las para criar conjuntos de constantes para uso com variáveis e propriedades.  
  
## <a name="when-to-use-an-enumeration"></a>Quando usar uma enumeração  
 Sempre que um procedimento aceita um conjunto limitado de variáveis, considere o uso de uma enumeração. As enumerações tornam-se um código mais claro e legível, especialmente quando são usados nomes significativos.  
  
 Os benefícios do uso de enumerações incluem:  
  
- Reduz os erros causados pela transposição ou números incorretas de digitação.  
  
- Torna mais fácil alterar valores no futuro.  
  
- Torna o código mais fácil de ler, o que significa que é menos provável que os erros possam surgir nele.  
  
- Garante a compatibilidade com o futuro. Com as enumerações, o código é menos provável de falhar se, posteriormente, alguém alterar os valores correspondentes aos nomes dos membros.  
  
## <a name="naming-enumerations"></a>Enumerações de nomenclatura  
 Use uma Convenção de nomenclatura para membros de enumeração. Quando Visual Basic encontrar um nome de membro de enumeração, uma exceção poderá ser gerada se outras bibliotecas de tipos referenciadas contiverem o mesmo nome. Use um prefixo exclusivo que identifique os valores de seu aplicativo ou componente.  
  
 Ao fazer referência a um membro de uma enumeração, você deve qualificar o nome do membro com o nome da enumeração ou, caso contrário, usar a `Imports` instrução. Para obter mais informações, consulte [enumerações e qualificação de nome](enumerations-and-name-qualification.md).  
  
## <a name="predefined-enumerations"></a>Enumerações predefinidas  
 Visual Basic fornece várias enumerações predefinidas, como `FirstDayOfWeek` e `MsgBoxResult` , para facilitar seu código. Para obter uma lista desses [, consulte constantes e enumerações](../../../language-reference/constants-and-enumerations.md).  
  
## <a name="see-also"></a>Confira também

- [Como declarar uma enumeração](how-to-declare-enumerations.md)
- [Como fazer referência a um membro de enumeração](how-to-refer-to-an-enumeration-member.md)
- [Enumerações e qualificação de nome](enumerations-and-name-qualification.md)
- [Como iterar em uma enumeração no Visual Basic](how-to-iterate-through-an-enumeration.md)
- [Como determinar a cadeia de caracteres associada a um valor de enumeração](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Instrução Enum](../../../language-reference/statements/enum-statement.md)
- [Constantes e enumerações](../../../language-reference/constants-and-enumerations.md)
