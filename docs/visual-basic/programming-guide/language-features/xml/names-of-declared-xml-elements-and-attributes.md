---
title: Nomes de elementos e atributos XML declarados
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [XML in Visual Basic]
- element names [XML in Visual Basic]
- names in XML literals [Visual Basic]
- attribute names [XML in Visual Basic]
- XML literals [Visual Basic], element names
ms.assetid: cc110118-b6cf-4ff9-a4e4-6233c90c9fbf
ms.openlocfilehash: 043243eeee7c24d8c63105047fa3e7e0ed58c7b0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84374662"
---
# <a name="names-of-declared-xml-elements-and-attributes-visual-basic"></a>Nomes de elementos e atributos XML declarados (Visual Basic)
Este tópico fornece diretrizes de Visual Basic para nomear elementos XML e atributos em literais XML.  Em um literal XML, você pode especificar um nome local ou um nome qualificado. Um nome qualificado consiste em um prefixo de namespace XML, dois-pontos e um nome local. Para obter mais informações sobre prefixos de namespace XML, consulte [XML Element literal](../../../language-reference/xml-literals/xml-element-literal.md).  
  
## <a name="rules"></a>Regras  
 Um nome local de um elemento ou atributo em Visual Basic deve aderir às regras a seguir.  
  
- Ele pode começar com um namespace. Ele deve começar com um caractere alfabético ou um sublinhado ( `_` ).  
  
- Ele deve conter apenas caracteres alfabéticos, dígitos decimais, sublinhados, pontos (.) e hifens (-).  
  
- Ele não deve ter mais de 1.024 caracteres.  
  
- Dois pontos que aparecem em nomes indicam a demarcação do namespace. Portanto, você pode usar somente dois-pontos para especificar um namespace XML para um nome específico.  
  
 Além disso, você deve aderir à seguinte diretriz.  
  
- A especificação XML 1,0 reserva todos os nomes que começam com a cadeia de caracteres "XML", de qualquer variação de capitalização. Portanto, não use esses nomes para seus nomes de elementos e atributos.  
  
### <a name="name-length-guidelines"></a>Diretrizes de comprimento do nome  
 Como uma questão prática, um nome deve ser o mais curto possível e, ao mesmo tempo, identificar claramente a natureza do elemento. Isso melhora a legibilidade do seu código e reduz a duração da linha e o tamanho do arquivo de origem.  
  
 No entanto, seu nome não deve ser tão curto que não Descreva adequadamente o elemento ou como seu código o utiliza. Isso é importante para a legibilidade do seu código. Se outra pessoa estiver tentando entender isso, ou se você mesmo estiver olhando para ele um longo tempo depois de ter escrito, os nomes de elemento apropriados poderão economizar tempo.  
  
## <a name="case-sensitivity-in-names"></a>Distinção de maiúsculas e minúsculas em nomes  
 Os nomes de elementos XML diferenciam maiúsculas de minúsculas. Isso significa que, quando o compilador de Visual Basic compara dois nomes que diferem apenas em maiúsculas e minúsculas, ele os interpreta como nomes diferentes. Por exemplo, ele interpreta `ABC` e `abc` como se referir a elementos separados.  
  
## <a name="xml-namespaces"></a>Namespaces XML  
 Ao criar um literal de elemento XML, você pode especificar o prefixo do namespace XML para o nome do elemento. Para obter mais informações, consulte [literal de elemento XML](../../../language-reference/xml-literals/xml-element-literal.md).  
  
## <a name="see-also"></a>Confira também

- [Criando XML no Visual Basic](creating-xml.md)
- [Literal do Elemento XML](../../../language-reference/xml-literals/xml-element-literal.md)
