---
title: O acessador 'Set' da propriedade '<propertyname>' não está acessível
ms.date: 07/20/2015
f1_keywords:
- vbc31102
- bc31102
helpviewer_keywords:
- BC31102
ms.assetid: 6f7b31b7-3656-4ae1-8851-90f5f4c6950a
ms.openlocfilehash: 077533a5b1fe241b61ded9516ad8f450d7dbbf5e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400338"
---
# <a name="set-accessor-of-property-propertyname-is-not-accessible"></a>O acessador 'Set' da propriedade '\<propertyname>' não está acessível
Uma instrução tenta armazenar o valor de uma propriedade quando não tem acesso ao procedimento da propriedade `Set` .  
  
 Se a [instrução SET](../statements/set-statement.md) for marcada com um nível de acesso mais restritivo do que sua [instrução Property](../statements/property-statement.md), uma tentativa de definir o valor da propriedade poderá falhar nos seguintes casos:  
  
- A `Set` instrução é marcada como [particular](../modifiers/private.md) e o código de chamada está fora da classe ou estrutura na qual a propriedade é definida.  
  
- A `Set` instrução é marcada como [protegida](../modifiers/protected.md) e o código de chamada não está na classe ou estrutura na qual a propriedade é definida, nem em uma classe derivada.  
  
- A `Set` instrução é marcada como [Friend](../modifiers/friend.md) e o código de chamada não está no mesmo assembly no qual a propriedade é definida.  
  
 **ID do erro:** BC31102  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se você tiver controle do código-fonte que define a propriedade, considere declarar o `Set` procedimento com o mesmo nível de acesso que a própria propriedade.  
  
- Se você não tem controle do código-fonte que define a propriedade ou deve restringir o nível de `Set` acesso do procedimento mais do que a propriedade em si, tente mover a instrução que define o valor da propriedade para uma região de código que tenha acesso melhor à propriedade.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos de propriedade](../../programming-guide/language-features/procedures/property-procedures.md)
- [Como declarar uma propriedade com níveis de acesso mistos](../../programming-guide/language-features/procedures/how-to-declare-a-property-with-mixed-access-levels.md)
