---
title: Como criar uma coleção usada por um inicializador de coleção
ms.date: 07/20/2015
helpviewer_keywords:
- collection initializers [Visual Basic]
ms.assetid: c858db10-424d-47e0-92cd-e08087cc5ebc
ms.openlocfilehash: 7cba290b92f41125a93d1ed022e4db5ef62da789
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414550"
---
# <a name="how-to-create-a-collection-used-by-a-collection-initializer-visual-basic"></a>Como criar uma coleção usada por um inicializador de coleção (Visual Basic)
Quando você usa um inicializador de coleção para criar uma coleção, o compilador Visual Basic procura um `Add` método do tipo de coleção para o qual os parâmetros do `Add` método correspondem aos tipos de valores no inicializador de coleção. Esse `Add` método é usado para popular a coleção com os valores do inicializador de coleção.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra uma `OrderCollection` coleção que contém um `Add` método público que um inicializador de coleção pode usar para adicionar objetos do tipo `Order` . O `Add` método permite que você use a sintaxe de inicializador de coleção abreviada.  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#4)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#1)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#2)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo2#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo2/VB/Module1.vb#3)]  
  
## <a name="see-also"></a>Confira também

- [Inicializadores de coleção](index.md)
- [Como criar um método para adicionar extensão usado por um inicializador de coleção](how-to-create-an-add-extension-method-used-by-a-collection-initializer.md)
