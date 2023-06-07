---
title: A codificação não pode ser definida como Nothing
ms.date: 07/20/2015
ms.assetid: 59f7c731-8291-4a85-bf51-c225e48cdc84
ms.openlocfilehash: 41565d1aa3b69f6ad92d4bbf2b2f2170014aef87
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84394473"
---
# <a name="encoding-cannot-be-set-to-nothing"></a>A codificação não pode ser definida como Nothing
Falha na tentativa de ler ou gravar em um arquivo porque o parâmetro foi `encoding` definido como `Nothing` , mas requer um valor válido.  
  
 <xref:System.Text.Encoding>é usado para determinar qual codificação usar ao gravar em um arquivo. O padrão é UTF-8.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Forneça um valor válido para o `encoding` parâmetro.  
  
## <a name="see-also"></a>Confira também

- [Codificações de arquivo](../developing-apps/programming/drives-directories-files/file-encodings.md)
- [Ler arquivos](../developing-apps/programming/drives-directories-files/reading-from-files.md)
- [Gravar em arquivos](../developing-apps/programming/drives-directories-files/writing-to-files.md)
- [My. Computer. FileSystem. ReadAllText](xref:Microsoft.VisualBasic.FileIO.FileSystem.ReadAllText%2A)
- [My. Computer. FileSystem. WriteAllText](xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A)
