---
title: ParamArray
ms.date: 07/20/2015
f1_keywords:
- vb.ParamArray
- ParamArray
helpviewer_keywords:
- ParamArray keyword [Visual Basic]
- ParamArray keyword [Visual Basic], syntax
ms.assetid: a5f18789-92bd-488f-9c7e-cf3719963635
ms.openlocfilehash: e3c24818ea87884a0dd9b42c604e13e16ca6d3d7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84391814"
---
# <a name="paramarray-visual-basic"></a>ParamArray (Visual Basic)
Especifica que um parâmetro de procedimento usa uma matriz opcional de elementos do tipo especificado. `ParamArray`pode ser usado somente no último parâmetro de uma lista de parâmetros.  
  
## <a name="remarks"></a>Comentários  
 `ParamArray`permite que você passe um número arbitrário de argumentos para o procedimento. Um `ParamArray` parâmetro é sempre declarado usando [ByVal](byval.md).  
  
 Você pode fornecer um ou mais argumentos a um `ParamArray` parâmetro passando uma matriz do tipo de dados apropriado, uma lista de valores separados por vírgula ou nada. Para obter detalhes, consulte "chamando um ParamArray" em [matrizes de parâmetros](../../programming-guide/language-features/procedures/parameter-arrays.md).  
  
> [!IMPORTANT]
> Sempre que você lida com uma matriz que pode ser indefinidamente grande, há um risco de sobreexecutar alguma capacidade interna de seu aplicativo. Se você aceitar uma matriz de parâmetros do código de chamada, deverá testar seu comprimento e tomar as medidas apropriadas se for muito grande para seu aplicativo.  
  
 O `ParamArray` modificador pode ser usado nesses contextos:  
  
 [Instrução Declare](../statements/declare-statement.md)  
  
 [Instrução Function](../statements/function-statement.md)  
  
 [Instrução Property](../statements/property-statement.md)  
  
 [Instrução Sub](../statements/sub-statement.md)  
  
## <a name="see-also"></a>Confira também

- [Palavras-chave](../keywords/index.md)
- [Matrizes de parâmetros](../../programming-guide/language-features/procedures/parameter-arrays.md)
