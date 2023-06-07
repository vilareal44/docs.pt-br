---
title: O tipo subjacente <typename> de Enum não é compatível com CLS
ms.date: 07/20/2015
f1_keywords:
- vbc40032
- bc40032
helpviewer_keywords:
- BC40032
ms.assetid: 32bf1949-fd73-456c-a323-bf1ffe1320ed
ms.openlocfilehash: 79faf0038b2b313bdc21e12c8ae76854bcd6957f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406567"
---
# <a name="underlying-type-typename-of-enum-is-not-cls-compliant"></a>O tipo subjacente \<typename> de Enum não é compatível com CLS
O tipo de dados especificado para essa enumeração não faz parte da [independência de linguagem e dos componentes independentes de linguagem](../../../standard/language-independence-and-language-independent-components.md) (CLS). Isso não é um erro no seu componente, porque o .NET Framework e Visual Basic oferecem suporte a esse tipo de dados. No entanto, outro componente escrito em código estritamente compatível com CLS pode não dar suporte a esse tipo de dados. Esse componente pode não ser capaz de interagir com êxito com seu componente.  
  
 Os tipos de dados a seguir Visual Basic não são compatíveis com CLS:  
  
- [Tipo de Dados SByte](../data-types/sbyte-data-type.md)  
  
- [Tipo de Dados UInteger](../data-types/uinteger-data-type.md)  
  
- [Tipo de Dados ULong](../data-types/ulong-data-type.md)  
  
- [Tipo de Dados UShort](../data-types/ushort-data-type.md)  
  
 Por padrão, esta mensagem é um aviso. Para obter mais informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC40032  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se o componente se desassociar apenas com outros componentes .NET Framework, ou não fizer interface com nenhum outro componente, você não precisará alterar nada.  
  
- Se você estiver fazendo a interface com um componente não escrito para o .NET Framework, talvez seja possível determinar, por meio de reflexão ou da documentação, se ele dá suporte a esse tipo de dados. Se tiver, você não precisará alterar nada.  
  
- Se você estiver fazendo a interface com um componente que não oferece suporte a esse tipo de dados, deverá substituí-lo pelo tipo mais próximo em conformidade com CLS. Por exemplo, no lugar de `UInteger` você pode ser capaz de usar `Integer` se não precisar do intervalo de valores acima de 2.147.483.647. Se você precisar do intervalo estendido, poderá substituir `UInteger` por `Long` .  
  
- Se você estiver fazendo a interface com automação ou objetos COM, tenha em mente que alguns tipos têm larguras de dados diferentes das .NET Framework. Por exemplo, `uint` geralmente é 16 bits em outros ambientes. Se você estiver passando um argumento de 16 bits para esse componente, declare-o como `UShort` em vez de `UInteger` em seu código de Visual Basic gerenciado.  
  
## <a name="see-also"></a>Confira também

- [Reflexão (Visual Basic)](../../programming-guide/concepts/reflection.md)
- [Reflexão](../../../framework/reflection-and-codedom/reflection.md)
