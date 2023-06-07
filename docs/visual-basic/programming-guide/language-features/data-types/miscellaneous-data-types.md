---
title: Tipos de dados diversos
ms.date: 07/20/2015
helpviewer_keywords:
- Object data type [Visual Basic], data types
- data types [Visual Basic], choosing
ms.assetid: 64c71a12-9057-4dbf-baca-7379c4aada69
ms.openlocfilehash: 0a08c0e7087c3efb0e5ffce51182defae45629ea
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84393747"
---
# <a name="miscellaneous-data-types-visual-basic"></a>Tipos de dados diversos (Visual Basic)
O Visual Basic fornece vários tipos de dados que não são orientados para números ou caracteres. Em vez disso, eles lidam com dados especializados, como valores Sim/Não, valores de data/hora e endereços de objeto.  
  
 Para uma tabela que mostra uma comparação lado a lado do Visual Basic tipos de dados, consulte [tipos de dados](../../../language-reference/data-types/index.md).  
  
## <a name="boolean-type"></a>Tipo booliano  
 O [tipo de dados booliano](../../../language-reference/data-types/boolean-data-type.md) é um valor não assinado que é interpretado como `True` ou `False` . Sua largura de dados depende da plataforma de implementação. Se uma variável puder conter apenas valores de dois Estados, como true/false, yes/no ou on/off, declare-a como `Boolean` .  
  
## <a name="date-type"></a>Tipo de data  
 O [tipo de dados Date](../../../language-reference/data-types/date-data-type.md) é um valor de 64 bits que contém as informações de data e hora. Cada incremento representa 100 nanossegundos de tempo decorrido desde o início (12:00 AM) de 1º de Janeiro do ano 1 no calendário gregoriano. Se uma variável puder conter um valor de data, um valor de hora ou ambos, declare-a como `Date` .  
  
## <a name="object-type"></a>Tipo de objeto  
 O [tipo de dados Object](../../../language-reference/data-types/object-data-type.md) é um endereço de 32 bits que aponta para uma instância de objeto dentro de seu aplicativo ou em algum outro aplicativo. Uma `Object` variável pode se referir a qualquer objeto que seu aplicativo reconhece ou a dados de qualquer tipo de dados. Isso inclui os dois *tipos de valor*, como `Integer` , `Boolean` e instâncias de estrutura, e *tipos de referência*, que são instâncias de objetos criados a partir de classes como e e instâncias de `String` <xref:System.Windows.Forms.Form> matriz.  
  
 Se uma variável armazena um ponteiro para uma instância de uma classe que você não conhece no momento da compilação, ou se ele pode apontar para dados de vários tipos de dados, declare-o como `Object` .  
  
 A vantagem do `Object` tipo de dados é que você pode usá-lo para armazenar dados de qualquer tipo de dados. A desvantagem é que você incorre em operações extras que levam mais tempo de execução e tornam seu aplicativo mais lento. Se você usar uma `Object` variável para tipos de valor, você incorrerá em *Boxing* e *unboxing*. Se você usá-lo para tipos de referência, você incorrerá em *associação tardia*.  
  
## <a name="see-also"></a>Confira também

- [Caracteres de tipo](type-characters.md)
- [Tipos de dados elementares](elementary-data-types.md)
- [Tipos de dados numéricos](numeric-data-types.md)
- [Tipos de dados de caractere](character-data-types.md)
- [Solução de problemas de tipos de dados](troubleshooting-data-types.md)
- [Associação antecipada e tardia](../early-late-binding/index.md)
