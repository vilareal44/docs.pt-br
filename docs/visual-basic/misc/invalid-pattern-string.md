---
title: Cadeia de caracteres de padrão inválida
ms.date: 07/20/2015
ms.assetid: ec1aecdb-5339-4a93-be71-eec56b1d7438
ms.openlocfilehash: aa408f4cecc2a2774cb98cba96cd04a67afcc546
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402207"
---
# <a name="invalid-pattern-string"></a>Cadeia de caracteres de padrão inválida
A cadeia de caracteres de padrão especificada na `Like` operação de uma pesquisa é inválida.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Examine os caracteres válidos para expressões de lista.  
  
2. No intervalo de padrões, verifique se o caractere de intervalo inicial é menor que o caractere de intervalo final, como em `[a-z]` .  
  
3. No intervalo de padrões, verifique se não há vários hifens próximos um do outro, como em `[a--z]` .  
  
4. Intervalos de padrão de término com um colchete de fechamento.  
  
## <a name="see-also"></a>Confira também

- [Operador Like](../language-reference/operators/like-operator.md)
