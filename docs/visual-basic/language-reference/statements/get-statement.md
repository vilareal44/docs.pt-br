---
title: Instrução Get
ms.date: 07/20/2015
f1_keywords:
- vb.Get
helpviewer_keywords:
- Get statement [Visual Basic], syntax
- Get statement [Visual Basic]
- properties [Visual Basic], read-only
- read-only properties
- Get keyword [Visual Basic]
- property procedures [Visual Basic], Get statements
ms.assetid: 56b05cdc-bd64-4dfd-bb12-824eacec6f94
ms.openlocfilehash: 31936fb2c8f658203a43702a2b5fa4ee2481beb5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404596"
---
# <a name="get-statement"></a>Instrução Get
Declara um `Get` procedimento de propriedade usado para recuperar o valor de uma propriedade.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
[ <attributelist> ] [ accessmodifier ] Get()  
    [ statements ]  
End Get  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`attributelist`|Opcional. Consulte a [lista de atributos](attribute-list.md).|  
|`accessmodifier`|Opcional em no máximo uma das `Get` instruções e `Set` nesta propriedade. Pode ser um dos seguintes:<br /><br /> -   [Protected](../modifiers/protected.md)<br />-   [Público](../modifiers/friend.md)<br />-   [Pessoal](../modifiers/private.md)<br />-   `Protected Friend`<br /><br /> Consulte [níveis de acesso em Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md).|  
|`statements`|Opcional. Uma ou mais instruções que são executadas quando o `Get` procedimento de propriedade é chamado.|  
|`End Get`|Obrigatórios. Encerra a definição do procedimento de `Get` propriedade.|  
  
## <a name="remarks"></a>Comentários  
 Cada propriedade deve ter um `Get` procedimento de propriedade, a menos que a propriedade esteja marcada `WriteOnly` . O `Get` procedimento é usado para retornar o valor atual da propriedade.  
  
 Visual Basic chama automaticamente o procedimento de uma propriedade `Get` quando uma expressão solicita o valor da propriedade.  
  
 O corpo da declaração de propriedade pode conter somente as propriedades `Get` e os `Set` procedimentos entre a [instrução de propriedade](property-statement.md) e a `End Property` instrução. Ele não pode armazenar nada além desses procedimentos. Em particular, ele não pode armazenar o valor atual da propriedade. Você deve armazenar esse valor fora da propriedade, porque se armazená-lo dentro de qualquer um dos procedimentos de propriedade, o outro procedimento de propriedade não poderá acessá-lo. A abordagem usual é armazenar o valor em uma variável [privada](../modifiers/private.md) declarada no mesmo nível da propriedade. Você deve definir um `Get` procedimento dentro da propriedade à qual ele se aplica.  
  
 O `Get` procedimento assume como padrão o nível de acesso de sua propriedade contendo, a menos que você use `accessmodifier` na `Get` instrução.  
  
## <a name="rules"></a>Regras  
  
- **Níveis de acesso misto.** Se você estiver definindo uma propriedade de leitura/gravação, poderá opcionalmente especificar um nível de acesso diferente para o `Get` ou o `Set` procedimento, mas não ambos. Se você fizer isso, o nível de acesso do procedimento deverá ser mais restritivo do que o nível de acesso da propriedade. Por exemplo, se a propriedade for declarada `Friend` , você poderá declarar o `Get` procedimento `Private` , mas não `Public` .  
  
     Se você estiver definindo uma `ReadOnly` propriedade, o `Get` procedimento representará a propriedade inteira. Não é possível declarar um nível de acesso diferente para `Get` , pois isso definiria dois níveis de acesso para a propriedade.  
  
- **Tipo de retorno.** A [instrução Property](property-statement.md) pode declarar o tipo de dados do valor que ele retorna. O `Get` procedimento retorna automaticamente esse tipo de dados. Você pode especificar qualquer tipo de dados ou o nome de uma enumeração, estrutura, classe ou interface.  
  
     Se a `Property` instrução não especificar `returntype` , o procedimento retornará `Object` .  
  
## <a name="behavior"></a>Comportamento  
  
- **Retornando de um procedimento.** Quando o `Get` procedimento retorna ao código de chamada, a execução continua dentro da instrução que solicitou o valor da propriedade.  
  
     `Get`os procedimentos de propriedade podem retornar um valor usando a [instrução return](return-statement.md) ou atribuindo o valor de retorno ao nome da propriedade. Para obter mais informações, consulte "valor de retorno" na [instrução function](function-statement.md).  
  
     As `Exit Property` `Return` instruções e causam uma saída imediata de um procedimento de propriedade. Qualquer número de `Exit Property` `Return` instruções and pode aparecer em qualquer lugar no procedimento, e você pode misturar `Exit Property` e `Return` demonstrativos.  
  
- **Valor de retorno.** Para retornar um valor de um `Get` procedimento, você pode atribuir o valor ao nome da propriedade ou incluí-lo em uma [instrução return](return-statement.md). A `Return` instrução atribui simultaneamente o valor de `Get` retorno do procedimento e sai do procedimento.  
  
     Se você usar `Exit Property` sem atribuir um valor ao nome da propriedade, o `Get` procedimento retornará o valor padrão para o tipo de dados da propriedade. Para obter mais informações, consulte "valor de retorno" na [instrução function](function-statement.md).  
  
     O exemplo a seguir ilustra duas maneiras como a propriedade somente leitura `quoteForTheDay` pode retornar o valor mantido na variável privada `quoteValue` .  
  
     [!code-vb[VbVbalrStatements#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#27)]  
  
     [!code-vb[VbVbalrStatements#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#28)]  
  
     [!code-vb[VbVbalrStatements#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#29)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Get` instrução para retornar o valor de uma propriedade.  
  
 [!code-vb[VbVbalrStatements#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#30)]  
  
## <a name="see-also"></a>Confira também

- [Instrução SET](set-statement.md)
- [Instrução Property](property-statement.md)
- [Instrução Exit](exit-statement.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
- [Instruções passo a passo: definindo classes](../../programming-guide/language-features/objects-and-classes/walkthrough-defining-classes.md)
