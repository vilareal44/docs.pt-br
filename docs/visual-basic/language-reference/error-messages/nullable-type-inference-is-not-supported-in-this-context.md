---
title: Inferência de tipo que permite valor nulo não suportada neste contexto
ms.date: 07/20/2015
f1_keywords:
- vbc36629
- bc36629
helpviewer_keywords:
- BC36629
ms.assetid: 0a1e2dbc-d9a4-433d-9306-c5540782b81d
ms.openlocfilehash: 52e5391fbcf30a4dada4d64a0e810c900ea85806
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409380"
---
# <a name="nullable-type-inference-is-not-supported-in-this-context"></a>Inferência de tipo que permite valor nulo não suportada neste contexto
Tipos e estruturas de valor podem ser declarados como anuláveis.  
  
```vb  
Dim a? As Integer  
Dim b As Integer?  
```  
  
 No entanto, você não pode usar a declaração anulável em combinação com a inferência de tipos. Os exemplos a seguir causam esse erro.  
  
```vb  
' Not valid.  
' Dim c? = 10  
' Dim d? = a  
```  
  
 **ID do erro:** BC36629  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Use uma `As` cláusula para declarar a variável como um tipo de valor anulável.  
  
## <a name="see-also"></a>Confira também

- [Tipos de valor anulável](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [Inferência de Tipo de Variável Local](../../programming-guide/language-features/variables/local-type-inference.md)
