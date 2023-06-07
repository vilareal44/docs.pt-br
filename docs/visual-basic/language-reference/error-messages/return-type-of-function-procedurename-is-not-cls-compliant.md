---
title: O tipo de retorno da função '<procedurename>' não é compatível com CLS
ms.date: 07/20/2015
f1_keywords:
- bc40027
- vbc40027
helpviewer_keywords:
- BC40027
ms.assetid: 33c088c7-48e7-400c-920e-6d8967e1f3fc
ms.openlocfilehash: 9cc7e25ef1be21ff2f6a71dcb61bc29ec92da30f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400351"
---
# <a name="return-type-of-function-procedurename-is-not-cls-compliant"></a>O tipo de retorno da função '\<procedurename>' não é compatível com CLS
Um `Function` procedimento é marcado como, `<CLSCompliant(True)>` mas retorna um tipo marcado como `<CLSCompliant(False)>` , não está marcado ou não está qualificado porque é um tipo não compatível.  
  
 Para que um procedimento seja compatível com a [independência de linguagem e com os componentes independentes de linguagem](../../../standard/language-independence-and-language-independent-components.md) (CLS), ele deve usar somente tipos em conformidade com CLS. Isso se aplica aos tipos de parâmetros, ao tipo de retorno e aos tipos de todas as suas variáveis locais.  
  
 Os tipos de dados a seguir Visual Basic não são compatíveis com CLS:  
  
- [Tipo de Dados SByte](../data-types/sbyte-data-type.md)  
  
- [Tipo de Dados UInteger](../data-types/uinteger-data-type.md)  
  
- [Tipo de Dados ULong](../data-types/ulong-data-type.md)  
  
- [Tipo de Dados UShort](../data-types/ushort-data-type.md)  
  
 Quando você aplica o a <xref:System.CLSCompliantAttribute> um elemento de programação, você define o parâmetro do atributo `isCompliant` como `True` ou `False` para indicar conformidade ou não conformidade. Não há nenhum padrão para esse parâmetro, e você deve fornecer um valor.  
  
 Se você não aplicar o <xref:System.CLSCompliantAttribute> a um elemento, será considerado como não compatível.  
  
 Por padrão, esta mensagem é um aviso. Para obter informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC40027  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se o `Function` procedimento deve retornar esse tipo específico, remova o <xref:System.CLSCompliantAttribute> . O procedimento não pode ser compatível com CLS.  
  
- Se o `Function` procedimento deve ser compatível com CLS, altere o tipo de retorno para o tipo em conformidade com CLS mais próximo. Por exemplo, no lugar de `UInteger` você pode ser capaz de usar `Integer` se não precisar do intervalo de valores acima de 2.147.483.647. Se você precisar do intervalo estendido, poderá substituir `UInteger` por `Long` .  
  
- Se você estiver fazendo a interface com automação ou objetos COM, tenha em mente que alguns tipos têm larguras de dados diferentes das .NET Framework. Por exemplo, `int` geralmente é 16 bits em outros ambientes. Se você estiver retornando um inteiro de 16 bits para esse componente, declare-o como `Short` em vez de `Integer` em seu código de Visual Basic gerenciado.
