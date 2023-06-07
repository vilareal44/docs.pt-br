---
title: Não é possível chamar a função friend no objeto que não é uma instância da classe de definição
ms.date: 07/20/2015
f1_keywords:
- vbrID97
ms.assetid: b9d821f0-8565-4f15-bb35-184789c69662
ms.openlocfilehash: c18c04470c4c49113e8195b92185326c5c4197c1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400842"
---
# <a name="cannot-call-friend-function-on-object-which-is-not-an-instance-of-defining-class"></a>Não é possível chamar a função friend no objeto que não é uma instância da classe de definição
Você tentou chamar o `Friend` procedimento de uma classe ou tentou acessar uma `Friend` propriedade ou um método entre processos ou entre threads. Um `Friend` procedimento é chamado de um módulo fora da classe, mas faz parte do projeto no qual a classe é definida.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Certifique-se de que você está chamando ou acessando o procedimento a partir de um módulo que faz parte do projeto no qual a classe está definida.  
  
## <a name="see-also"></a>Confira também

- [Público](../language-reference/modifiers/friend.md)
