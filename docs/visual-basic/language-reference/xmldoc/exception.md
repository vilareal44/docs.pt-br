---
title: <exception>
ms.date: 07/20/2015
helpviewer_keywords:
- <exception> XML tag
- exception XML tag
ms.assetid: c0517549-171e-4dae-ab88-a9c1700b6eee
ms.openlocfilehash: 3a2452ec60a2182adfee365777d9824001ff006a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400118"
---
# <a name="exception-visual-basic"></a>\<exception> (Visual Basic)
Especifica quais exceções podem ser geradas.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<exception cref="member">description</exception>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `member`  
 Uma referência a uma exceção que está disponível no ambiente de compilação atual. O compilador verifica se a exceção apresentada existe e move o `member` para o nome de elemento canônico no XML de saída. `member` deve ser exibido entre aspas duplas (" ").  
  
 `description`  
 Uma descrição.  
  
## <a name="remarks"></a>Comentários  
 Use a `<exception>` marca para especificar quais exceções podem ser geradas. Essa marcação é aplicada a uma definição de método.  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<exception>` marca para descrever uma exceção que a `IntDivide` função pode gerar.  
  
 [!code-vb[VbVbcnXmlDocComments#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
