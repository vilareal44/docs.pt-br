---
title: Instrução Implements
ms.date: 07/20/2015
f1_keywords:
- vb.Implements
- Implements
helpviewer_keywords:
- Implements statement [Visual Basic], syntax
- Implements statement [Visual Basic]
- interface implementation [Visual Basic], Implements statement
ms.assetid: 1fafb83f-f55a-4215-8ea9-681e8622613d
ms.openlocfilehash: 7fb43934d8c200ff29b1caf63cec830b2c6633ce
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404544"
---
# <a name="implements-statement"></a>Instrução Implements
Especifica uma ou mais interfaces, ou membros de interface, que devem ser implementados na definição de classe ou estrutura na qual ele aparece.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Implements interfacename [, ...]  
' -or-  
Implements interfacename.interfacemember [, ...]  
```  
  
## <a name="parts"></a>Partes  
 `interfacename`  
 Obrigatórios. Uma interface cujas propriedades, procedimentos e eventos devem ser implementados por membros correspondentes na classe ou estrutura.  
  
 `interfacemember`  
 Obrigatórios. O membro de uma interface que está sendo implementada.  
  
## <a name="remarks"></a>Comentários  
 Uma interface é uma coleção de protótipos que representa os membros (Propriedades, procedimentos e eventos) que a interface encapsula. As interfaces contêm apenas as declarações para membros; classes e estruturas implementam esses membros. Para obter mais informações, consulte [interfaces](../../programming-guide/language-features/interfaces/index.md).  
  
 A `Implements` instrução deve seguir imediatamente a `Class` `Structure` instrução ou.  
  
 Ao implementar uma interface, você deve implementar todos os membros declarados na interface. A omissão de qualquer membro é considerada um erro de sintaxe. Para implementar um membro individual, você especifica a palavra-chave [Implements](implements-clause.md) (que é separada da `Implements` instrução) quando você declara o membro na classe ou estrutura. Para obter mais informações, consulte [interfaces](../../programming-guide/language-features/interfaces/index.md).  
  
 As classes podem usar implementações [privadas](../modifiers/private.md) de propriedades e procedimentos, mas esses membros são acessíveis somente por meio da conversão de uma instância da classe de implementação em uma variável declarada para ser do tipo da interface.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar a `Implements` instrução para implementar membros de uma interface. Ele define uma interface chamada `ICustomerInfo` com um evento, uma propriedade e um procedimento. A classe `customerInfo` implementa todos os membros definidos na interface.  
  
 [!code-vb[VbVbalrStatements#33](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#33)]  
  
 Observe que a classe `customerInfo` usa a `Implements` instrução em uma linha de código-fonte separada para indicar que a classe implementa todos os membros da `ICustomerInfo` interface. Em seguida, cada membro na classe usa a `Implements` palavra-chave como parte de sua declaração de membro para indicar que ela implementa esse membro de interface.  
  
## <a name="example"></a>Exemplo  
 Os dois procedimentos a seguir mostram como você pode usar a interface implementada no exemplo anterior. Para testar a implementação, adicione esses procedimentos ao seu projeto e chame o `testImplements` procedimento.  
  
 [!code-vb[VbVbalrStatements#34](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#34)]  
  
## <a name="see-also"></a>Confira também

- [Implementações](implements-clause.md)
- [Instrução Interface](interface-statement.md)
- [Interfaces](../../programming-guide/language-features/interfaces/index.md)
