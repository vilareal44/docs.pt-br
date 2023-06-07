---
title: Ansi
ms.date: 07/20/2015
f1_keywords:
- vb.Ansi
helpviewer_keywords:
- Declare statement [Visual Basic], marshaling strings [Visual Basic]
- ANSI, Visual Basic
- ANSI
ms.assetid: 4f1fa6ff-5557-41ab-b6da-90baf4c15917
ms.openlocfilehash: 67792e52c21555bef46548e9ab0a6ebd32061071
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84373194"
---
# <a name="ansi-visual-basic"></a>Ansi (Visual Basic)
Especifica que Visual Basic deve realizar marshaling de todas as cadeias de caracteres para valores de American National Standards Institute (ANSI), independentemente do nome do procedimento externo que está sendo declarado.  
  
 Quando você chama um procedimento definido fora do seu projeto, o compilador Visual Basic não tem acesso às informações necessárias para chamar o procedimento corretamente. Essas informações incluem onde o procedimento está localizado, como ele é identificado, sua sequência de chamada e tipo de retorno e o conjunto de caracteres da cadeia de caracteres que ele usa. A [instrução Declare](../statements/declare-statement.md) cria uma referência a um procedimento externo e fornece essas informações necessárias.  
  
 A `charsetmodifier` parte na `Declare` instrução fornece as informações do conjunto de caracteres para o marshaling de cadeias durante uma chamada para o procedimento externo. Ele também afeta como Visual Basic pesquisa o nome do procedimento externo no arquivo externo. O `Ansi` modificador especifica que Visual Basic deve empacotar todas as cadeias de caracteres para valores ANSI e deve procurar o procedimento sem modificar seu nome durante a pesquisa.  
  
 Se nenhum modificador de conjunto de caracteres for especificado, `Ansi` será o padrão.  
  
## <a name="remarks"></a>Comentários  
 O `Ansi` modificador pode ser usado neste contexto:  
  
 [Instrução Declare](../statements/declare-statement.md)  
  
## <a name="smart-device-developer-notes"></a>Notas para desenvolvedores de dispositivos inteligentes  
 Não há suporte para essa palavra-chave.  
  
## <a name="see-also"></a>Confira também

- [Automático](auto.md)
- [Unicode](unicode.md)
- [Palavras-chave](../keywords/index.md)
