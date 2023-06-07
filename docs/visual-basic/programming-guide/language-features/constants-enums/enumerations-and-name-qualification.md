---
title: Enumerações e qualificação de nome
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- Imports statement [Visual Basic], namespace declarations
- declaring namespaces [Visual Basic], enumerations
- name collisions
- ambiguous names [Visual Basic], enumerations
- enumerations [Visual Basic], name qualification
- names [Visual Basic], avoiding conflicts
- namespaces [Visual Basic], declaring
- naming conflicts, enumerations
- naming conflicts, qualifying names
- declaring enumerations
- references [Visual Basic], enumeration members
- naming conventions [Visual Basic], naming conflicts
- declarations [Visual Basic], namespaces
ms.assetid: 08ba2738-df52-4140-bc55-f57c871c9b73
ms.openlocfilehash: 4b09284b8282c481e406050d37cbdb2f3c8686bc
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414499"
---
# <a name="enumerations-and-name-qualification-visual-basic"></a>Enumerações e qualificação de nome (Visual Basic)
Normalmente, ao fazer referência a um membro de uma enumeração, você deve qualificar o nome do membro com o nome da enumeração. Por exemplo, para se referir ao `Sunday` membro da sua `Days` enumeração, você usaria a seguinte sintaxe:  
  
 [!code-vb[VbEnumsTask#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#18)]  
  
## <a name="using-the-imports-statement"></a>Usando a instrução Imports  
 Você pode evitar o uso de nomes totalmente qualificados adicionando uma `Imports` instrução à seção de declarações de namespace do seu código, como no exemplo a seguir:  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 Uma `Imports` instrução importa nomes de namespace de projetos e assemblies referenciados e de dentro do mesmo projeto que o módulo no qual a instrução é exibida. Depois que essa instrução for adicionada, você poderá consultar seus membros de enumeração sem qualificação, como no exemplo a seguir:  
  
 [!code-vb[VbEnumsTask#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#24)]  
  
 Ao organizar conjuntos de constantes relacionadas em enumerações, você pode usar os mesmos nomes de constantes em diferentes contextos. Por exemplo, você pode usar os mesmos nomes para as constantes do dia da semana nas `Days` `WorkDays` enumerações e. Se você usar a `Imports` instrução com suas enumerações, deverá ter cuidado para evitar referências ambíguas. Considere o exemplo a seguir:  
  
 [!code-vb[VbEnumsTask#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#22)]  
  
 [!code-vb[VbEnumsTask#25](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#25)]  
  
 Supondo que `Monday` seja um membro da `Days` enumeração e da `Workdays` enumeração, esse código gera um erro do compilador. Para evitar referências ambíguas ao fazer referência a uma constante individual, qualifique o nome da constante com sua enumeração. O código a seguir refere-se às `Saturday` constantes `Days` nas `WorkDays` enumerações e.  
  
 [!code-vb[VbEnumsTask#32](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#32)]  
  
## <a name="see-also"></a>Confira também

- [Constantes e enumerações](../../../language-reference/constants-and-enumerations.md)
- [Como declarar uma enumeração](how-to-declare-enumerations.md)
- [Como fazer referência a um membro de enumeração](how-to-refer-to-an-enumeration-member.md)
- [Como iterar em uma enumeração no Visual Basic](how-to-iterate-through-an-enumeration.md)
- [Como determinar a cadeia de caracteres associada a um valor de enumeração](how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [Quando usar uma enumeração](when-to-use-an-enumeration.md)
- [Tipos de dados constante e literal](constant-and-literal-data-types.md)
- [Instrução Enum](../../../language-reference/statements/enum-statement.md)
- [Instrução Imports (tipo e namespace .NET)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)
- [Tipos de dados](../../../language-reference/data-types/index.md)
