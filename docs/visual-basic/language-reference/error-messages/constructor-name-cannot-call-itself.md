---
title: O construtor '<name>' não pode se chamar
ms.date: 07/20/2015
f1_keywords:
- bc30298
- vbc30298
helpviewer_keywords:
- BC30298
ms.assetid: 2d77b7f4-0640-4f89-9c65-f101fd2847c0
ms.openlocfilehash: 6abb6dde624e129b52fefecf8c51e6cde2567ae1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409797"
---
# <a name="constructor-name-cannot-call-itself"></a>O construtor '\<name>' não pode se chamar
Um `Sub New` procedimento em uma classe ou estrutura chama a si mesmo.  
  
 A finalidade de um construtor é inicializar uma instância de uma classe ou estrutura quando ela é criada pela primeira vez. Uma classe ou estrutura pode ter vários construtores, desde que todos tenham diferentes listas de parâmetros. Um construtor tem permissão para chamar outro construtor para executar sua funcionalidade, além de seu próprio. Mas não faz sentido para um Construtor chamar a si mesmo e, de fato, resultaria em recursão infinita, se permitido.  
  
 **ID do erro:** BC30298  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Verifique a lista de parâmetros do construtor que está sendo chamado. Ele deve ser diferente do construtor que faz a chamada.  
  
2. Se você não pretende chamar um Construtor diferente, remova `Sub New` totalmente a chamada.  
  
## <a name="see-also"></a>Confira também

- [Tempo de vida do objeto: como os objetos são criados e destruídos](../../programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
