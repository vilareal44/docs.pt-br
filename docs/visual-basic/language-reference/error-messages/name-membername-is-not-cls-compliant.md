---
title: O nome <membername> não é compatível com CLS
ms.date: 07/20/2015
f1_keywords:
- bc40031
- vbc40031
helpviewer_keywords:
- BC40031
ms.assetid: e2b885dc-cbf9-49ff-bbbe-531657ea99f7
ms.openlocfilehash: 45c9332237dffc7311daeedaf36035d9e9958415
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84397175"
---
# <a name="name-membername-is-not-cls-compliant"></a>O nome \<membername> não é compatível com CLS
Um assembly é marcado como `<CLSCompliant(True)>` , mas expõe um membro com um nome que começa com um sublinhado ( `_` ).  
  
 Um elemento de programação pode conter um ou mais sublinhados, mas para ser compatível com a [independência de linguagem e com os componentes independentes de linguagem](../../../standard/language-independence-and-language-independent-components.md) (CLS), ele não deve começar com um sublinhado. Consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).  
  
 Quando você aplica o a <xref:System.CLSCompliantAttribute> um elemento de programação, você define o parâmetro do atributo `isCompliant` como `True` ou `False` para indicar conformidade ou não conformidade. Não há nenhum padrão para esse parâmetro, e você deve fornecer um valor.  
  
 Se você não aplicar o <xref:System.CLSCompliantAttribute> a um elemento, será considerado como não compatível.  
  
 Por padrão, esta mensagem é um aviso. Para obter informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC40031  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se você tiver controle sobre o código-fonte, altere o nome do membro para que ele não comece com um sublinhado.  
  
- Se você precisar que o nome do membro permaneça inalterado, remova o <xref:System.CLSCompliantAttribute> de sua definição ou marque-o como `<CLSCompliant(False)>` . Você ainda pode marcar o assembly como `<CLSCompliant(True)>` .  
  
## <a name="see-also"></a>Confira também

- [Nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [Convenções de nomenclatura do Visual Basic](../../programming-guide/program-structure/naming-conventions.md)
