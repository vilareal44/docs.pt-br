---
title: O namespace ou o tipo especificado no nível de projeto Imports '<qualifiedelementname>' não contém nenhum membro público ou não pode ser localizado.
ms.date: 07/20/2015
f1_keywords:
- vbc40057
- bc40057
helpviewer_keywords:
- BC40057
ms.assetid: 4ae3506e-2ebe-4ff3-995d-14ac60db5e9f
ms.openlocfilehash: 0ee235252d69e6f77ce53b048f45e73d0969e864
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409446"
---
# <a name="namespace-or-type-specified-in-the-project-level-imports-qualifiedelementname-doesnt-contain-any-public-member-or-cannot-be-found"></a>O namespace ou o tipo especificado no nível de projeto Imports '\<qualifiedelementname>' não contém nenhum membro público ou não pode ser localizado.
O namespace ou tipo especificado nas importações de nível de projeto ' \<qualifiedelementname> ' não contém nenhum membro público ou não pode ser encontrado. Verifique se o namespace ou o tipo está definido e contém pelo menos um membro público. Verifique se o nome do alias não contém outros aliases.  
  
 Uma propriedade de importação de um projeto especifica um elemento contido que não pode ser encontrado ou não define nenhum `Public` membro.  
  
 Um *elemento recipiente* pode ser um namespace, uma classe, uma estrutura, um módulo, uma interface ou uma enumeração. O elemento contentor contém membros, como variáveis, procedimentos ou outros elementos que os contêm.  
  
 A finalidade da importação é permitir que seu código acesse namespace ou membros de tipo sem precisar qualificá-los. Seu projeto também pode precisar adicionar uma referência ao namespace ou ao tipo. Para obter mais informações, consulte "importando elementos continentes" em [referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md).  
  
 Se o compilador não encontrar o elemento contido especificado, ele não poderá resolver referências que o utilizam. Se encontrar o elemento, mas o elemento não expor nenhum `Public` membro, nenhuma referência poderá ser bem-sucedida. Em ambos os casos, não há sentido para importar o elemento.  
  
 Você usa o **Designer de projeto** para especificar elementos a serem importados. Use a seção **namespaces importados** da página **referências** . Você pode acessar o **Designer de projeto** clicando duas vezes no ícone **meu projeto** em **Gerenciador de soluções**.  
  
 **ID do erro:** BC40057  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Abra o **Designer de projeto** e alterne para a página de **referência** .  
  
2. Na seção **namespaces importados** , verifique se o elemento que o contém pode ser acessado do seu projeto.  
  
3. Verifique se o elemento que o contém expõe pelo menos um `Public` membro.  
  
## <a name="see-also"></a>Confira também

- [Página Referências, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/references-page-project-designer-visual-basic)
- [Gerenciando propriedades de solução e de projeto](/visualstudio/ide/managing-project-and-solution-properties)
- [Pública](../modifiers/public.md)
- [Namespaces no Visual Basic](../../programming-guide/program-structure/namespaces.md)
- [Referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)
