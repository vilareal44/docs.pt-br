---
title: Um nome inválido foi especificado para o log de evento
ms.date: 07/20/2015
ms.assetid: b1b158bd-f13f-4371-a8af-31c0e86ae6be
ms.openlocfilehash: 70b1de2a3776a9c68260cc431b65e754d7247a0c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84412917"
---
# <a name="an-invalid-name-was-specified-for-the-event-log"></a>Um nome inválido foi especificado para o log de evento
Um nome inválido foi especificado para o log de eventos. Normalmente, isso é resultado de caracteres inválidos no nome, um nome de arquivo em branco ou um nome de arquivo muito longo.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se o nome especificado tiver mais de oito caracteres, verifique se não há conflito com os nomes de outros logs de eventos. Somente os oito primeiros caracteres são avaliados ao determinar se o nome é exclusivo.  
  
- Se fornecer um caminho, verifique se ele foi analisado corretamente.  
  
- Verifique se não há caracteres inválidos no nome. Os caracteres que não podem ser usados em um nome de arquivo incluem `<` , `>` ,,,, `:` `"` `/` `\` e `|` .  
  
## <a name="see-also"></a>Confira também

- [Como: analisar caminhos de arquivo](../developing-apps/programming/drives-directories-files/how-to-parse-file-paths.md)
- [Como: renomear um arquivo](../developing-apps/programming/drives-directories-files/how-to-rename-a-file.md)
