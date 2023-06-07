---
title: Instrução REM
ms.date: 07/20/2015
f1_keywords:
- vb.'
- vb.Rem
- Rem
- "'"
helpviewer_keywords:
- REM statement [Visual Basic]
- comments, Visual Basic code
- code comments, Visual Basic
- Visual Basic code, comments
- "' comment marker character [Visual Basic]"
ms.assetid: 34126d7f-e0f9-476d-91e6-b31b398615dc
ms.openlocfilehash: 68c898145bd8845c657b6ebb8776a3a9027c359c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404259"
---
# <a name="rem-statement-visual-basic"></a>Instrução REM (Visual Basic)
Usado para incluir comentários explicativos no código-fonte de um programa.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
REM comment  
' comment  
```  
  
## <a name="parts"></a>Partes  
 `comment`  
 Opcional. O texto de qualquer comentário que você deseja incluir. Um espaço é necessário entre a `REM` palavra-chave e `comment` .  
  
## <a name="remarks"></a>Comentários  
 Você pode colocar uma `REM` instrução sozinha em uma linha ou pode colocá-la em uma linha após outra instrução. A `REM` instrução deve ser a última instrução na linha. Se ele seguir outra instrução, o `REM` deverá ser separado dessa instrução por um espaço.  
  
 Você pode usar uma aspa simples ( `'` ) em vez de `REM` . Isso é verdadeiro se o comentário segue outra instrução na mesma linha ou fica sozinho em uma linha.  
  
> [!NOTE]
> Você não pode continuar uma `REM` instrução usando uma sequência de continuação de linha ( `_` ). Depois que um comentário é iniciado, o compilador não examina os caracteres para obter um significado especial. Para um comentário de várias linhas, use outra `REM` instrução ou um símbolo de comentário ( `'` ) em cada linha.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir ilustra a `REM` instrução, que é usada para incluir comentários explicativos em um programa. Ele também mostra a alternativa de usar o caractere de aspa simples ( `'` ) em vez de `REM` .  
  
 [!code-vb[VbVbalrStatements#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Comentários no código](../../programming-guide/program-structure/comments-in-code.md)
- [Como: Quebrar e combinar instruções no código](../../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
