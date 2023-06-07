---
title: Conversão implícita de '<typename1>' em '<typename2>' ao copiar o valor do parâmetro 'ByRef' '<parametername>' para o argumento correspondente.
ms.date: 07/20/2015
f1_keywords:
- vbc41999
- bc41999
helpviewer_keywords:
- BC41999
ms.assetid: ae48c738-dff8-4c0f-8931-bbb70b2c8b03
ms.openlocfilehash: 4d0f9aac795f683cf58210ea38b3783e451ccfc3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402856"
---
# <a name="implicit-conversion-from-typename1-to-typename2-in-copying-the-value-of-byref-parameter-parametername-back-to-the-matching-argument"></a>Conversão implícita de '\<typename1>' em '\<typename2>' ao copiar o valor do parâmetro 'ByRef' '\<parametername>' para o argumento correspondente.
Um procedimento é chamado com um argumento [ByRef](../modifiers/byref.md) de um tipo diferente daquele do parâmetro correspondente.  
  
 Se você passar um argumento `ByRef` , o Visual Basic às vezes copiará o valor do argumento em uma variável local no procedimento em vez de passar uma referência. Nesse caso, quando o procedimento retorna, Visual Basic deve copiar o valor da variável local de volta para o argumento no código de chamada.  
  
 Se um `ByRef` valor de argumento for copiado para o procedimento e o argumento e o parâmetro forem do mesmo tipo, nenhuma conversão será necessária. Mas se os tipos forem diferentes, Visual Basic deverá converter em ambas as direções. Como você não pode usar `CType` ou qualquer uma das outras palavras-chave de conversão em um argumento ou parâmetro de procedimento, tal conversão é sempre implícita.  
  
 Por padrão, esta mensagem é um aviso. Para obter informações sobre como ocultar avisos ou tratar avisos como erros, consulte [Configurando avisos no Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic).  
  
 **ID do erro:** BC41999  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se possível, use um argumento de chamada do mesmo tipo que o parâmetro de procedimento, portanto Visual Basic não precisa fazer nenhuma conversão.  
  
- Se você precisar chamar o procedimento com um tipo de argumento diferente do tipo de parâmetro, mas não precisar retornar um valor para o argumento de chamada, defina o parâmetro como [ByVal](../modifiers/byval.md) em vez de `ByRef` .  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](../../programming-guide/language-features/procedures/index.md)
- [Parâmetros e Argumentos de Procedimento](../../programming-guide/language-features/procedures/procedure-parameters-and-arguments.md)
- [Passar argumentos por valor e por referência](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
- [Conversões implícitas e explícitas](../../programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
