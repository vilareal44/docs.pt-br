---
title: Nomes de elementos declarados
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], case sensitivity
- names [Visual Basic], Visual Basic rules
- naming conventions [Visual Basic]
- element names [Visual Basic]
- declared elements [Visual Basic], identifiers
- declarations [Visual Basic], elements
- declared elements [Visual Basic], valid names
- '[] escape characters [Visual Basic]'
- names [Visual Basic], elements
- declaration statements [Visual Basic], declared elements
- declaring elements [Visual Basic]
- identifiers [Visual Basic], declared elements
- case sensitivity, declared element names
- escape characters [Visual Basic]
- names [Visual Basic], declared elements
- declared elements [Visual Basic], about declared elements
- escaped names [Visual Basic]
- declared elements [Visual Basic], names
- names [Visual Basic], naming conventions
- identifiers [Visual Basic], elements
ms.assetid: 09d8843b-c0dc-4afe-9dab-87c439a69e66
ms.openlocfilehash: cdba2b5f3e17fc6666ca653abd7f4bd7dfb31c4a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84392916"
---
# <a name="declared-element-names-visual-basic"></a>Nomes de elemento declarados (Visual Basic)
Cada elemento declarado tem um nome, também chamado de *identificador*, que é o que o código usa para se referir a ele.  
  
## <a name="rules"></a>Regras  
 Um nome de elemento em Visual Basic deve observar as seguintes regras:  
  
- Ele deve começar com um caractere alfabético ou um sublinhado ( `_` ).  
  
- Ele deve conter apenas caracteres alfabéticos, dígitos decimais e sublinhados.  
  
- Ele deve conter pelo menos um caractere alfabético ou dígito decimal se começar com um sublinhado.  
  
- Ele não deve ter mais de 1023 caracteres.  
  
 O limite de comprimento de 1023 caracteres também se aplica à cadeia de caracteres inteira de um nome totalmente qualificado, como `outerNamespace.middleNamespace.innerNamespace.thisClass.thisElement` .  
  
 O exemplo a seguir mostra alguns nomes de elemento válidos.  
  
 `aB123__45`  
  
 `_567`  
  
 O exemplo a seguir mostra alguns nomes de elemento inválidos. O primeiro contém apenas um sublinhado, o segundo começa com um dígito decimal e o terceiro contém um caractere inválido ($).  
  
 `' Three INVALID element names`  
  
 `_`  
  
 `12ABC`  
  
 `xyz$wv`  
  
> [!CAUTION]
> Nomes de elementos que começam com um sublinhado ( `_` ) não fazem parte da [independência de linguagem e dos componentes independentes de linguagem](../../../../standard/language-independence-and-language-independent-components.md) (CLS), portanto, o código em conformidade com CLS não pode usar um componente que define tais nomes. No entanto, um sublinhado em qualquer outra posição em um nome de elemento é compatível com CLS.  
  
### <a name="name-length-guidelines"></a>Diretrizes de comprimento do nome  
 Como uma questão prática, seu nome deve ser o mais curto possível e, ao mesmo tempo, identificar claramente a natureza do elemento. Isso melhora a legibilidade do seu código e reduz a duração da linha e o tamanho do arquivo de origem.  
  
 Por outro lado, seu nome não deve ser tão curto que não Descreva adequadamente o que o elemento representa e como seu código o utiliza. Isso é importante para a legibilidade do seu código. Se alguém estiver tentando entender isso, ou se você mesmo estiver olhando para ele um longo tempo depois de ter escrito, nomes de elemento adequados podem economizar um tempo considerável.  
  
## <a name="escaped-names"></a>Nomes com escape  
 Em geral, um nome de elemento não deve corresponder a nenhuma das palavras-chave reservadas por Visual Basic, como `Case` ou `Friend` . No entanto, você pode definir um *nome de escape*, que é colocado entre colchetes ( `[ ]` ). Um nome de escape pode corresponder a qualquer palavra-chave Visual Basic, já que os colchetes removem qualquer ambiguidade. Você também usará os colchetes quando se referir ao nome posteriormente em seu código.  
  
 Em geral, você deve usar nomes com escape somente quando:  
  
- Seu código foi migrado de uma versão anterior do Visual Basic que não reservava a palavra-chave que está sendo usada como um nome; or  
  
- Você está trabalhando com código escrito em outro idioma no qual a palavra-chave fornecida não está reservada.  
  
 Caso contrário, você deve considerar renomear o elemento se seu nome entrar em conflito com uma palavra-chave. O IDE (ambiente de desenvolvimento integrado) fornece uma maneira fácil de fazer isso. Para obter mais informações, consulte [refatoração](/visualstudio/ide/refactoring-in-visual-studio).  
  
## <a name="case-sensitivity-in-names"></a>Distinção de maiúsculas e minúsculas em nomes  
 Os nomes de elementos em Visual Basic não diferenciam maiúsculas de minúsculas. Isso significa que, quando o compilador compara dois nomes que diferem somente em maiúsculas e minúsculas, ele os interpreta como o mesmo nome. Por exemplo, ele considera `ABC` e `abc` se refere ao mesmo elemento declarado.  
  
 No entanto, o Common Language Runtime (CLR) usa a associação que diferencia maiúsculas de minúsculas. Portanto, quando você produz um assembly ou uma DLL e o disponibiliza para outros assemblies, seus nomes não são mais sensíveis a maiúsculas e minúsculas. Por exemplo, se você definir uma classe com um elemento chamado `ABC` , e outros assemblies fizerem uso de sua classe por meio da Common Language Runtime, eles deverão se referir ao elemento como `ABC` . Se, posteriormente, você recompilar sua classe e alterar o nome do elemento para `abc` , os outros assemblies que usam sua classe não poderão mais acessar esse elemento. Portanto, quando você libera uma versão atualizada de um assembly, não deve alterar o caso alfabético de quaisquer elementos públicos.  
  
## <a name="names-and-locales"></a>Nomes e localidades  
 A comparação de nomes é independente da localidade. Se dois nomes corresponderem em uma localidade, eles terão a garantia de corresponder em todas as localidades.  
  
## <a name="see-also"></a>Confira também

- [Elementos declarados](index.md)
- [Características do Elemento Declarado](declared-element-characteristics.md)
- [Referências a elementos declarados](references-to-declared-elements.md)
- [Instruções](../../../language-reference/statements/index.md)
