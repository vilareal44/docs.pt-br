---
title: <param>
ms.date: 07/20/2015
helpviewer_keywords:
- param XML tag
- <param> XML tag
ms.assetid: 4e32e86f-f6f3-4301-b7fc-2f321fb54368
ms.openlocfilehash: d325d5f9fbfd132630cf280653be214a267a7a80
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400054"
---
# <a name="param-visual-basic"></a>\<param> (Visual Basic)
Define um nome de parâmetro e uma descrição.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<param name="name">description</param>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `name`  
 O nome do parâmetro de um método. Coloque o nome entre aspas duplas (" ").  
  
 `description`  
 Uma descrição do parâmetro.  
  
## <a name="remarks"></a>Comentários  
 A `<param>` marca deve ser usada no comentário para uma declaração de método para descrever um dos parâmetros para o método.  
  
 O texto da `<param>` marca aparecerá nos seguintes locais:  
  
- Informações de parâmetro do IntelliSense. Para obter mais informações, veja [Usando o IntelliSense](/visualstudio/ide/using-intellisense).  
  
- Pesquisador de objetos. Para obter mais informações, consulte [exibindo a estrutura do código](/visualstudio/ide/viewing-the-structure-of-code).  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<param>` marca para descrever o `id` parâmetro.  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
