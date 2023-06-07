---
title: O namespace ou o tipo especificado em Imports '<qualifiedelementname>' não contém nenhum membro público ou não pode ser localizado
ms.date: 07/20/2015
f1_keywords:
- bc40056
- vbc40056
helpviewer_keywords:
- BC40056
ms.assetid: b59f5754-444f-4378-9272-9678b437e84a
ms.openlocfilehash: 8675d9c3b202200c89e12e7a5f51a19d9e3e0e64
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409459"
---
# <a name="namespace-or-type-specified-in-the-imports-qualifiedelementname-doesnt-contain-any-public-member-or-cannot-be-found"></a>O namespace ou o tipo especificado em Imports '\<qualifiedelementname>' não contém nenhum membro público ou não pode ser localizado

O namespace ou o tipo especificado em Imports ' \<qualifiedelementname> ' não contém nenhum membro público ou não pode ser encontrado. Verifique se o namespace ou o tipo está definido e contém pelo menos um membro público. Verifique se o nome do alias não contém outros aliases.

Uma `Imports` instrução especifica um elemento contido que não pode ser encontrado ou não define nenhum `Public` membro.

Um *elemento recipiente* pode ser um namespace, uma classe, uma estrutura, um módulo, uma interface ou uma enumeração. O elemento contentor contém membros, como variáveis, procedimentos ou outros elementos que os contêm.

A finalidade da importação é permitir que seu código acesse namespace ou membros de tipo sem precisar qualificá-los. Seu projeto também pode precisar adicionar uma referência ao namespace ou ao tipo. Para obter mais informações, consulte "importando elementos continentes" em [referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md).

Se o compilador não encontrar o elemento contido especificado, ele não poderá resolver referências que o utilizam. Se encontrar o elemento, mas o elemento não expor nenhum `Public` membro, nenhuma referência poderá ser bem-sucedida. Em ambos os casos, não há sentido para importar o elemento.

Tenha em mente que, se você importar um elemento recipiente e atribuir um alias de importação a ele, não poderá usar esse alias de importação para importar outro elemento. O código a seguir gera um erro do compilador.

```vb
Imports winfrm = System.Windows.Forms

' The following statement is INVALID  because it reuses an import alias.

Imports behave = winfrm.Design.Behavior`
```

**ID do erro:** BC40056

## <a name="to-correct-this-error"></a>Para corrigir este erro

1. Verifique se o elemento que o contém pode ser acessado do seu projeto.

2. Verifique se a especificação do elemento recipiente não inclui nenhum alias de importação de outra importação.

3. Verifique se o elemento que o contém expõe pelo menos um `Public` membro.

## <a name="see-also"></a>Confira também

- [Instrução Imports (tipo e namespace .NET)](../statements/imports-statement-net-namespace-and-type.md)
- [Instrução Namespace](../statements/namespace-statement.md)
- [Pública](../modifiers/public.md)
- [Namespaces no Visual Basic](../../programming-guide/program-structure/namespaces.md)
- [Referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
