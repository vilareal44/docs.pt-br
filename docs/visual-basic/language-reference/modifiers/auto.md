---
title: Automático
ms.date: 07/20/2015
f1_keywords:
- vb.Auto
helpviewer_keywords:
- Auto keyword [Visual Basic], external references
- Declare statement [Visual Basic], marshaling strings
- Auto keyword [Visual Basic]
- Auto keyword [Visual Basic], marshaling strings
ms.assetid: bf79ba95-a62c-48a5-916f-0ac7a52c13ec
ms.openlocfilehash: b9bdeed55788252c71b8fb1c995c140cbfdf60eb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84373122"
---
# <a name="auto-visual-basic"></a>Auto (Visual Basic)
Especifica que Visual Basic deve empacotar cadeias de caracteres de acordo com .NET Framework regras com base no nome externo do procedimento externo que está sendo declarado.  
  
 Quando você chama um procedimento definido fora do seu projeto, o compilador Visual Basic não tem acesso às informações que ele deve ter para chamar o procedimento corretamente. Essas informações incluem onde o procedimento está localizado, como ele é identificado, sua sequência de chamada e tipo de retorno e o conjunto de caracteres da cadeia de caracteres que ele usa. A [instrução Declare](../statements/declare-statement.md) cria uma referência a um procedimento externo e fornece essas informações necessárias.  
  
 A `charsetmodifier` parte na `Declare` instrução fornece as informações do conjunto de caracteres para o marshaling de cadeias durante uma chamada para o procedimento externo. Ele também afeta como Visual Basic pesquisa o nome do procedimento externo no arquivo externo. O `Auto` modificador especifica que Visual Basic deve empacotar cadeias de caracteres de acordo com .NET Framework regras e que ele deve determinar o conjunto de caractere base da plataforma de tempo de execução e possivelmente modificar o nome do procedimento externo se a pesquisa inicial falhar. Para obter mais informações, consulte "conjuntos de caracteres" na [instrução Declare](../statements/declare-statement.md).  
  
 Se nenhum modificador de conjunto de caracteres for especificado, `Ansi` será o padrão.  
  
## <a name="remarks"></a>Comentários  
 O `Auto` modificador pode ser usado neste contexto:  
  
 [Instrução Declare](../statements/declare-statement.md)  
  
## <a name="smart-device-developer-notes"></a>Notas para desenvolvedores de dispositivos inteligentes  
 Não há suporte para essa palavra-chave.  
  
## <a name="see-also"></a>Confira também

- [Ansi](ansi.md)
- [Unicode](unicode.md)
- [Palavras-chave](../keywords/index.md)
