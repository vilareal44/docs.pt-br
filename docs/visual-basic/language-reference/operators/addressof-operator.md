---
title: Operador AddressOf
ms.date: 07/20/2015
f1_keywords:
- AddressOf
- vb.AddressOf
helpviewer_keywords:
- AddressOf operator [Visual Basic]
- addresses, passing to API procedures
ms.assetid: 8105a59d-60d8-4ab5-b221-5899cdfacbf4
ms.openlocfilehash: 3e7db8e7329ce8d21b6e07863e6f1673a6389608
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84372058"
---
# <a name="addressof-operator-visual-basic"></a>Operador AddressOf (Visual Basic)
Cria uma instância delegada que faz referência ao procedimento específico.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
AddressOf procedurename  
```  
  
## <a name="parts"></a>Partes  
 `procedurename`  
 Obrigatórios. Especifica o procedimento a ser referenciado pelo delegado recém-criado.  
  
## <a name="remarks"></a>Comentários  
 O `AddressOf` operador cria um delegado que aponta para a sub-rotina ou função especificada por `procedurename` . Quando o procedimento especificado é um método de instância, o delegado se refere à instância e ao método. Em seguida, quando o delegado é invocado, o método especificado da instância especificada é chamado.  
  
 O `AddressOf` operador pode ser usado como o operando de um construtor delegado ou pode ser usado em um contexto no qual o tipo do delegado possa ser determinado pelo compilador.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa o `AddressOf` operador para designar um delegado para manipular o `Click` evento de um botão.  
  
 [!code-vb[VbVbalrDelegates#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#8)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa o `AddressOf` operador para designar a função de inicialização para um thread.  
  
 [!code-vb[VbVbalrDelegates#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrDelegates/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Declare](../statements/declare-statement.md)
- [Instrução Function](../statements/function-statement.md)
- [Instrução Sub](../statements/sub-statement.md)
- [Delegados](../../programming-guide/language-features/delegates/index.md)
