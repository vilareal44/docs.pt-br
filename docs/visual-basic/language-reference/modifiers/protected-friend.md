---
title: Amigo Protegido
ms.date: 05/10/2018
f1_keywords:
- vb.ProtectedFriend
helpviewer_keywords:
- Protected Friend keyword [Visual Basic]
- Protected Friend keyword [Visual Basic], syntax
ms.openlocfilehash: 27fc993ca0b94d406261d5e6275de8cd619eb6a8
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303446"
---
# <a name="protected-friend-visual-basic"></a>Amigo protegido (Visual Basic)

A combinação de palavras-chave `Protected Friend` é um modificador de acesso de membro. Ele confere o acesso [Friend](friend.md) e o acesso [protegido](protected.md) nos elementos declarados, para que eles possam ser acessados de qualquer lugar no mesmo assembly, de sua própria classe e de classes derivadas. Você pode especificar `Protected Friend` somente membros de classes; não é possível aplicar `Protected Friend` a membros de uma estrutura porque as estruturas não podem ser herdadas.

> [!NOTE]
> No Visual Studio, a seleção de ajuda F1 no `protected friend` fornece ajuda para o [protegido](protected.md) ou [amigo](friend.md). O IDE escolhe o único token sob o cursor em vez da palavra composta.

## <a name="rules"></a>Regras

**Contexto de declaração.** Você pode usar `Protected Friend` somente no nível de classe. Isso significa que o contexto de declaração para um `Protected` elemento deve ser uma classe e não pode ser um arquivo de origem, namespace, interface, módulo, estrutura ou procedimento.

## <a name="see-also"></a>Veja também

- [Pública](public.md)
- [Protected](protected.md)
- [Público](friend.md)
- [Privada](private.md)
- [Particular protegido](./private-protected.md)
- [Níveis de acesso no Visual Basic](../../programming-guide/language-features/declared-elements/access-levels.md)
- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Estruturas](../../programming-guide/language-features/data-types/structures.md)
- [Objetos e classes](../../programming-guide/language-features/objects-and-classes/index.md)
