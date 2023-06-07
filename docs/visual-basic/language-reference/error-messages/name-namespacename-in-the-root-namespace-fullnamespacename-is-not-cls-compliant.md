---
title: O nome <namespacename> no namespace raiz <fullnamespacename> não é compatível com CLS
ms.date: 07/20/2015
f1_keywords:
- vbc40039
- bc40039
helpviewer_keywords:
- BC40039
ms.assetid: c5bd5914-ae71-416a-8bed-f76f644f78be
ms.openlocfilehash: b03a50365122c17fa311a284bd6995d1af2631c3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401531"
---
# <a name="name-namespacename-in-the-root-namespace-fullnamespacename-is-not-cls-compliant"></a>O nome \<namespacename> no namespace raiz \<fullnamespacename> não é compatível com CLS
Um assembly é marcado como `<CLSCompliant(True)>` , mas um elemento do nome do namespace raiz começa com um sublinhado ( `_` ).  
  
 Um elemento de programação pode conter um ou mais sublinhados, mas para ser compatível com a [independência de linguagem e com os componentes independentes de linguagem](../../../standard/language-independence-and-language-independent-components.md) (CLS), ele não deve começar com um sublinhado. Consulte [nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md).  
  
 Quando você aplica o a <xref:System.CLSCompliantAttribute> um elemento de programação, você define o parâmetro do atributo `isCompliant` como `True` ou `False` para indicar conformidade ou não conformidade. Não há nenhum padrão para esse parâmetro, e você deve fornecer um valor.  
  
 Se você não aplicar o <xref:System.CLSCompliantAttribute> a um elemento, será considerado como não compatível.  
  
 Por padrão, esta mensagem é um aviso. Para obter informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC40039  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se você precisar de conformidade com CLS, altere o nome do namespace raiz para que nenhum de seus elementos comece com um sublinhado.  
  
- Se você precisar que o nome do namespace permaneça inalterado, remova o <xref:System.CLSCompliantAttribute> do assembly ou marque-o como `<CLSCompliant(False)>` .  
  
## <a name="see-also"></a>Confira também

- [Instrução Namespace](../statements/namespace-statement.md)
- [Namespaces no Visual Basic](../../programming-guide/program-structure/namespaces.md)
- [-rootnamespace](../../reference/command-line-compiler/rootnamespace.md)
- [Página de Aplicativo, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
- [Nomes de elementos declarados](../../programming-guide/language-features/declared-elements/declared-element-names.md)
- [Convenções de nomenclatura do Visual Basic](../../programming-guide/program-structure/naming-conventions.md)
