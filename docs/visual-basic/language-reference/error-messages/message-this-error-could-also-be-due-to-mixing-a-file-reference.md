---
title: <message> Este erro também poderia ocorrer devido à mistura de uma referência de arquivo com uma referência de projeto ao assembly '<assemblyname>'
ms.date: 07/20/2015
f1_keywords:
- bc30971
- vbc30971
helpviewer_keywords:
- BC30971
ms.assetid: 75d2e8b5-2fdc-4623-8b32-cba805dab7db
ms.openlocfilehash: 263e30f992ef58b0053acb2fd749d0d9186ef6b8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397253"
---
# <a name="message-this-error-could-also-be-due-to-mixing-a-file-reference-with-a-project-reference-to-assembly-assemblyname"></a>\<message> Este erro também poderia ocorrer devido à mistura de uma referência de arquivo com uma referência de projeto ao assembly '\<assemblyname>'
\<message>Esse erro também pode ser devido à combinação de uma referência de arquivo com uma referência de projeto para o assembly ' \<assemblyname> . Nesse caso, tente substituir a referência de arquivo para ' \<assemblyfilename> ' no projeto ' \<projectname1> ' por uma referência de projeto para ' \<projectname2> '.  
  
 O código em seu projeto acessa um membro de outro projeto, mas a configuração da sua solução não permite que o compilador Visual Basic resolva a referência.  
  
 Para acessar um tipo definido em outro assembly, o compilador Visual Basic deve ter uma referência a esse assembly. Essa deve ser uma referência única e não ambígua que não cause referências circulares entre projetos.  
  
 **ID do erro:** BC30971  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Determine qual projeto produz o melhor assembly para referência do seu projeto. Para essa decisão, você pode usar critérios como a facilidade de acesso a arquivos e a frequência de atualizações.  
  
2. Nas propriedades do projeto, adicione uma referência ao projeto que contém o assembly que define o tipo que você está usando.  
  
## <a name="see-also"></a>Confira também

- [Gerenciando referências em um projeto](/visualstudio/ide/managing-references-in-a-project)
- [Referências a elementos declarados](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)

- [Gerenciando propriedades de solução e de projeto](/visualstudio/ide/managing-project-and-solution-properties)
- [Solução de Problemas de Referências Quebradas](/visualstudio/ide/troubleshooting-broken-references)
