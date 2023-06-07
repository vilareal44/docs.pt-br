---
title: "'<eventname>' é um evento, e não pode ser chamado diretamente"
ms.date: 07/20/2015
f1_keywords:
- bc32022
- vbc32022
helpviewer_keywords:
- BC32022
ms.assetid: 4dcfcb8d-a9fa-46a7-a034-29d9ff3a59b3
ms.openlocfilehash: 510fff5370e63a31ee271421c0ab6f154518899f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409589"
---
# <a name="eventname-is-an-event-and-cannot-be-called-directly"></a>'\<eventname>' é um evento, e não pode ser chamado diretamente
' <`eventname`> ' é um evento e, portanto, não pode ser chamado diretamente. Use uma `RaiseEvent` instrução para gerar um evento.  
  
 Uma chamada de procedimento especifica um evento para o nome do procedimento. Um manipulador de eventos é um procedimento, mas o evento em si é um dispositivo de sinalização, que deve ser gerado e manipulado.  
  
 **ID do erro:** BC32022  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Use uma `RaiseEvent` instrução para sinalizar um evento e invocar o procedimento ou procedimentos que o manipulam.  
  
## <a name="see-also"></a>Confira também

- [Instrução RaiseEvent](../statements/raiseevent-statement.md)
