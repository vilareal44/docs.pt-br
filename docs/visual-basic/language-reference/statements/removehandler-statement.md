---
title: Instrução RemoveHandler
ms.date: 07/20/2015
f1_keywords:
- vb.RemoveHandlerMethod
- vb.RemoveHandler
- RemoveHandler
helpviewer_keywords:
- RemoveHandler keyword [Visual Basic]
- RemoveHandler statement [Visual Basic]
ms.assetid: 647cd825-e877-4910-b4f1-8d168beebe6a
ms.openlocfilehash: 3514a79f2430b148e6a3727b83029b4e207a677b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404246"
---
# <a name="removehandler-statement"></a>Instrução RemoveHandler
Remove a associação entre um evento e um manipulador de eventos.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
RemoveHandler event, AddressOf eventhandler  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`event`|O nome do evento que está sendo manipulado.|  
|`eventhandler`|O nome do procedimento que está manipulando o evento no momento.|  
  
## <a name="remarks"></a>Comentários  
 As `AddHandler` `RemoveHandler` instruções e permitem que você inicie e interrompa a manipulação de eventos para um evento específico a qualquer momento durante a execução do programa.  
  
> [!NOTE]
> Para eventos personalizados, a `RemoveHandler` instrução invoca o `RemoveHandler` acessador do evento. Para obter mais informações sobre eventos personalizados, consulte [Event Statement](event-statement.md).  
  
## <a name="example"></a>Exemplo  
 [!code-vb[VbVbalrEvents#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrEvents/VB/Class1.vb#17)]  
  
## <a name="see-also"></a>Confira também

- [Instrução AddHandler](addhandler-statement.md)
- [Alças](handles-clause.md)
- [Instrução Event](event-statement.md)
- [Eventos](../../programming-guide/language-features/events/index.md)
