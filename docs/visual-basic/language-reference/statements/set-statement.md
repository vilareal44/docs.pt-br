---
title: Instrução Set
ms.date: 07/20/2015
f1_keywords:
- vb.Set
helpviewer_keywords:
- property procedures [Visual Basic], Set statements
- Set statement [Visual Basic]
- Set statement [Visual Basic], syntax
- write-only properties
- properties [Visual Basic], write-only
ms.assetid: 9ecc27b4-df84-420d-9075-db25455fb3cd
ms.openlocfilehash: 49d4c36805b64d7232a94e818256723a0437b6ef
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404181"
---
# <a name="set-statement-visual-basic"></a>Instrução Set (Visual Basic)
Declara um `Set` procedimento de propriedade usado para atribuir um valor a uma propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
[ <attributelist> ] [ accessmodifier ] Set (ByVal value [ As datatype ])  
    [ statements ]  
End Set  
```  
  
## <a name="parts"></a>Partes  
 `attributelist`  
 Opcional. Consulte a [lista de atributos](attribute-list.md).  
  
 `accessmodifier`  
 Opcional em no máximo uma das `Get` instruções e `Set` nesta propriedade. Pode ser um dos seguintes:  
  
- [Protected](../modifiers/protected.md)  
  
- [Público](../modifiers/friend.md)  
  
- [Privada](../modifiers/private.md)  
  
- `Protected Friend`  
  
 Consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).  
  
 `value`  
 Obrigatórios. Parâmetro que contém o novo valor para a propriedade.  
  
 `datatype`  
 Obrigatório se `Option Strict` for `On` . Tipo de dados do `value` parâmetro. O tipo de dados especificado deve ser o mesmo que o tipo de dados da propriedade em que essa `Set` instrução é declarada.  
  
 `statements`  
 Opcional. Uma ou mais instruções que são executadas quando o `Set` procedimento de propriedade é chamado.  
  
 `End Set`  
 Obrigatórios. Encerra a definição do procedimento de `Set` propriedade.  
  
## <a name="remarks"></a>Comentários  
 Cada propriedade deve ter um `Set` procedimento de propriedade, a menos que a propriedade esteja marcada `ReadOnly` . O `Set` procedimento é usado para definir o valor da propriedade.  
  
 Visual Basic chama automaticamente o procedimento de uma propriedade `Set` quando uma instrução de atribuição fornece um valor a ser armazenado na propriedade.  
  
 Visual Basic passa um parâmetro para o `Set` procedimento durante as atribuições de propriedade. Se você não fornecer um parâmetro para `Set` , o IDE (ambiente de desenvolvimento integrado) usará um parâmetro implícito chamado `value` . O parâmetro contém o valor a ser atribuído à propriedade. Normalmente, você armazena esse valor em uma variável local privada e retorna-o sempre que o `Get` procedimento é chamado.  
  
 O corpo da declaração de propriedade pode conter somente as propriedades `Get` e os `Set` procedimentos entre a [instrução de propriedade](property-statement.md) e a `End Property` instrução. Ele não pode armazenar nada além desses procedimentos. Em particular, ele não pode armazenar o valor atual da propriedade. Você deve armazenar esse valor fora da propriedade, porque se armazená-lo dentro de qualquer um dos procedimentos de propriedade, o outro procedimento de propriedade não poderá acessá-lo. A abordagem usual é armazenar o valor em uma variável [privada](../modifiers/private.md) declarada no mesmo nível da propriedade. Você deve definir um `Set` procedimento dentro da propriedade à qual ele se aplica.  
  
 O `Set` procedimento assume como padrão o nível de acesso de sua propriedade contendo, a menos que você use `accessmodifier` na `Set` instrução.  
  
## <a name="rules"></a>Regras  
  
- **Níveis de acesso misto.** Se você estiver definindo uma propriedade de leitura/gravação, poderá opcionalmente especificar um nível de acesso diferente para o `Get` ou o `Set` procedimento, mas não ambos. Se você fizer isso, o nível de acesso do procedimento deverá ser mais restritivo do que o nível de acesso da propriedade. Por exemplo, se a propriedade for declarada `Friend` , você poderá declarar o `Set` procedimento `Private` , mas não `Public` .  
  
     Se você estiver definindo uma `WriteOnly` propriedade, o `Set` procedimento representará a propriedade inteira. Não é possível declarar um nível de acesso diferente para `Set` , pois isso definiria dois níveis de acesso para a propriedade.  
  
## <a name="behavior"></a>Comportamento  
  
- **Retornando de um procedimento de propriedade.** Quando o `Set` procedimento retorna ao código de chamada, a execução continua seguindo a instrução que forneceu o valor a ser armazenado.  
  
     `Set`os procedimentos de propriedade podem ser retornados usando a [instrução return](return-statement.md) ou a [instrução Exit](exit-statement.md).  
  
     As `Exit Property` `Return` instruções e causam uma saída imediata de um procedimento de propriedade. Qualquer número de `Exit Property` `Return` instruções and pode aparecer em qualquer lugar no procedimento, e você pode misturar `Exit Property` e `Return` demonstrativos.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Set` instrução para definir o valor de uma propriedade.  
  
 [!code-vb[VbVbalrStatements#55](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#55)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Get](get-statement.md)
- [Instrução Property](property-statement.md)
- [Instrução Sub](sub-statement.md)
- [Procedimentos de propriedade](../../programming-guide/language-features/procedures/property-procedures.md)
