---
title: Não foi possível obter informações sobre memória devido a um erro interno
ms.date: 07/20/2015
f1_keywords:
- vbrDiagnosticInfo_Memory
ms.assetid: 1ba8f774-5858-438e-914e-99fddc9e5e7e
ms.openlocfilehash: dff6ee2f0f46052efae557e1216f73b9f249b464
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402246"
---
# <a name="could-not-obtain-memory-information-due-to-internal-error"></a>Não foi possível obter informações sobre memória devido a um erro interno
Falha em uma chamada para uma das propriedades de informações de memória do `My.Computer.Info` objeto.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Adicione um `Try...Catch` bloco em volta da chamada à propriedade Memory-Information do `My.Computer.Info` objeto.  
  
## <a name="see-also"></a>Confira também

- [My.Computer.Info](xref:Microsoft.VisualBasic.Devices.ComputerInfo)
- [Instrução Try...Catch...Finally](../language-reference/statements/try-catch-finally-statement.md)
