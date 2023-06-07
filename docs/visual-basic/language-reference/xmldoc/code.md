---
title: <code>
ms.date: 07/20/2015
helpviewer_keywords:
- code XML tag
- <code> XML tag
ms.assetid: 925e5342-be05-45f2-bf66-7398bbd6710e
ms.openlocfilehash: aa65fed863718f1f00b510f82051a13e764e1b23
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400132"
---
# <a name="code-visual-basic"></a>\<code> (Visual Basic)
Indica que o texto tem várias linhas de código.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<code>content</code>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `content`  
 O texto a ser marcado como código.  
  
## <a name="remarks"></a>Comentários  
 Use a `<code>` marca para indicar várias linhas como código. Use [\<c>](c.md) para indicar que o texto dentro de uma descrição deve ser marcado como código.  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a \<code> marca para incluir o código de exemplo para usar o `ID` campo.  
  
 [!code-vb[VbVbcnXmlDocComments#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#2)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
