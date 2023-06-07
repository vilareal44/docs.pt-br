---
title: <paramref>
ms.date: 07/20/2015
helpviewer_keywords:
- paramref XML tag
- <paramref> XML tag
ms.assetid: 8979d53b-beb1-41b7-b41e-6bbea1c17a03
ms.openlocfilehash: 3ca08016d3cef0c44e4f3c0bd0d017628eda606d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400041"
---
# <a name="paramref-visual-basic"></a>\<paramref> (Visual Basic)
Formata uma palavra como um parâmetro.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<paramref name="name"/>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `name`  
 O nome do parâmetro ao qual você deseja se referir. Coloque o nome entre aspas duplas (" ").  
  
## <a name="remarks"></a>Comentários  
 A `<paramref>` marca fornece uma maneira de indicar que uma palavra é um parâmetro. O arquivo XML pode ser processado para formatar esse parâmetro de uma maneira distinta.  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<paramref>` marca para fazer referência ao `id` parâmetro.  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
