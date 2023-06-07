---
title: Instrução Erase
ms.date: 07/20/2015
f1_keywords:
- vb.Erase
helpviewer_keywords:
- Erase keyword [Visual Basic]
- Erase statement [Visual Basic]
ms.assetid: 7a8133d7-b750-4d74-8b66-ba1dd9778d4b
ms.openlocfilehash: 31aeaf822bc9c1de59a5c5f68406c6521216ae0e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404713"
---
# <a name="erase-statement-visual-basic"></a>Instrução Erase (Visual Basic)
Usado para liberar variáveis de matriz e desalocar a memória usada para seus elementos.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Erase arraylist  
```  
  
## <a name="parts"></a>Partes  
 `arraylist`  
 Obrigatórios. Lista de variáveis de matriz a serem apagadas. Várias variáveis são separadas por vírgulas.  
  
## <a name="remarks"></a>Comentários  
 A `Erase` instrução pode aparecer somente no nível do procedimento. Isso significa que você pode liberar matrizes dentro de um procedimento, mas não no nível de classe ou de módulo.  
  
 A `Erase` instrução é equivalente a atribuir `Nothing` a cada variável de matriz.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Erase` instrução para limpar duas matrizes e liberar sua memória (1000 e 100 elementos de armazenamento, respectivamente). `ReDim`Em seguida, a instrução atribui uma nova instância de matriz à matriz tridimensional.  
  
 [!code-vb[VbVbalrStatements#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#19)]  
  
## <a name="see-also"></a>Confira também

- [Nada](../nothing.md)
- [Instrução ReDim](redim-statement.md)
