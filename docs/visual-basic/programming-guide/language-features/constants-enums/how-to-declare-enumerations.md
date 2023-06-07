---
title: Como Declarar Enumerações
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- enumerations [Visual Basic], declaring
- declaring enumerations [Visual Basic]
ms.assetid: db4ca1c3-f429-4c81-ae81-29e0157b29fd
ms.openlocfilehash: c8f228c205c93adf7f2f555dc840a7daac61950b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414447"
---
# <a name="how-to-declare-enumerations-visual-basic"></a>Como declarar enumerações (Visual Basic)
Você cria uma enumeração com a `Enum` instrução na seção declarações de uma classe ou módulo. Você não pode declarar uma enumeração dentro de um método. Para especificar o nível apropriado de acesso, use `Private` , `Protected` , `Friend` ou `Public` .  
  
 Um `Enum` tipo tem um nome, um tipo subjacente e um conjunto de campos, cada um representando uma constante. O nome deve ser um qualificador de Visual Basic .NET válido. O tipo subjacente deve ser um dos tipos inteiros — `Byte` , `Short` `Long` ou `Integer` . O padrão é `Integer`. As enumerações são sempre fortemente tipadas e não são intercambiáveis com tipos de números inteiros.  
  
 Enumerações não podem ter valores de ponto flutuante. Se uma enumeração for atribuída a um valor de ponto flutuante com `Option Strict On` , ocorrerá um erro de compilador. Se `Option Strict` for `Off` , o valor será convertido automaticamente para o `Enum` tipo.  
  
 Para obter informações sobre nomes e como usar a `Imports` instrução para tornar a qualificação de nome desnecessária, consulte [enumerações e qualificação de nome](enumerations-and-name-qualification.md).  
  
### <a name="to-declare-an-enumeration"></a>Para declarar uma enumeração  
  
1. Escreva uma declaração que inclua um nível de acesso de código, a `Enum` palavra-chave e um nome válido, como nos exemplos a seguir, cada um deles declara um diferente `Enum` .  
  
     [!code-vb[VbEnumsTask#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#3)]  
  
2. Defina as constantes na enumeração. Por padrão, a primeira constante em uma enumeração é inicializada para `0` e as constantes subsequentes são inicializadas para um valor mais um que a constante anterior. Por exemplo, a seguinte enumeração, `Days` , contém uma constante chamada `Sunday` com o valor `0` , uma constante chamada `Monday` com o valor `1` , uma constante chamada `Tuesday` com o valor de `2` e assim por diante.  
  
     [!code-vb[VbEnumsTask#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#4)]  
  
3. Você pode atribuir explicitamente valores a constantes em uma enumeração usando uma instrução de atribuição. Você pode atribuir qualquer valor inteiro, incluindo números negativos. Por exemplo, você pode querer que constantes com valores menores que zero representem condições de erro. Na enumeração a seguir, a constante `Invalid` atribuiu explicitamente o valor `–1` e a constante `Sunday` é atribuída ao valor `0` . Como é a primeira constante na enumeração, `Saturday` também é inicializada para o valor `0` . O valor de `Monday` é `1` (um maior que o valor de `Sunday` ); o valor de `Tuesday` é `2` e assim por diante.  
  
     [!code-vb[VbEnumsTask#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#5)]  
  
### <a name="to-declare-an-enumeration-as-an-explicit-type"></a>Para declarar uma enumeração como um tipo explícito  
  
- Especifique o tipo de enumeração usando a `As` cláusula, conforme mostrado no exemplo a seguir.  
  
     [!code-vb[VbEnumsTask#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Enumerações e qualificação de nome](enumerations-and-name-qualification.md)
- [Como fazer referência a um membro de enumeração](how-to-refer-to-an-enumeration-member.md)
- [Como iterar em uma enumeração no Visual Basic](how-to-iterate-through-an-enumeration.md)
- [Como determinar a cadeia de caracteres associada a um valor de enumeração](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Quando usar uma enumeração](when-to-use-an-enumeration.md)
- [Visão geral de constantes](constants-overview.md)
- [Tipos de dados constante e literal](constant-and-literal-data-types.md)
- [Constantes e enumerações](../../../language-reference/constants-and-enumerations.md)
