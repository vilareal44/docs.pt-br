---
title: Palavras-chave como nomes de elemento no código
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, naming conventions
- keywords [Visual Basic], in code
- name conflicts [Visual Basic]
- element names [Visual Basic], in code
ms.assetid: 2e4e8e02-23f7-49b9-a1c8-2b0402b6b525
ms.openlocfilehash: a98f0b027700717b414d58e1284ddec655eb25f7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403220"
---
# <a name="keywords-as-element-names-in-code-visual-basic"></a>Palavras-chave como nomes de elemento em código (Visual Basic)
Qualquer elemento Program, como uma variável, uma classe ou um membro, pode ter o mesmo nome que uma palavra-chave restrita. Por exemplo, você pode criar uma variável chamada `Loop` . No entanto, para se referir à sua versão dele — que tem o mesmo nome que a `Loop` palavra-chave Restricted — você deve precedê-la com uma cadeia de caracteres de qualificação completa ou colocá-la entre colchetes ( `[ ]` ), como mostra o exemplo a seguir.  
  
 [!code-vb[VbVbcnConventions#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#8)]  
  
 Se você não fizer isso, o Visual Basic assumirá o uso da `Loop` palavra-chave intrínseca e produzirá um erro, como no exemplo a seguir:  
  
 `' The following statement causes a compiler error.`  
  
 `Loop.Visible = True`  
  
 Você pode usar colchetes ao fazer referência a formulários e controles e ao declarar uma variável ou definir um procedimento com o mesmo nome de uma palavra-chave restrita. Pode ser fácil esquecer de qualificar nomes ou incluir colchetes e, portanto, introduzir erros em seu código e torná-lo mais difícil de ler. Por esse motivo, recomendamos que você não use palavras-chave restritas como os nomes dos elementos do programa. No entanto, se uma versão futura do Visual Basic definir uma nova palavra-chave que esteja em conflito com um formulário ou nome de controle existente, você poderá usar essa técnica ao atualizar seu código para trabalhar com a nova versão.  
  
> [!NOTE]
> Seu programa também pode incluir nomes de elementos fornecidos por outros assemblies referenciados. Se esses nomes entrarem em conflito com palavras-chave restritas, colocar colchetes em relação a eles fará com que Visual Basic interpretá-los como seus elementos definidos.  
  
## <a name="see-also"></a>Confira também

- [Convenções de nomenclatura do Visual Basic](naming-conventions.md)
- [Estrutura do programa e convenções de código](program-structure-and-code-conventions.md)
- [Palavras-chave](../../language-reference/keywords/index.md)
