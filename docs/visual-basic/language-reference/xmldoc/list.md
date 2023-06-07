---
title: <list>
ms.date: 07/20/2015
helpviewer_keywords:
- listheader XML tag
- <item> XML tag
- <list> XML tag
- <listheader> XML tag
- term XML tag
- list XML tag
- <description> XML tag
- description XML tag
- item XML tag
- <term> XML tag
ms.assetid: ec35fced-d58e-4520-a764-0691256e014b
ms.openlocfilehash: 955c1a4c5c5619f908b8d03dbf12360c23574478
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400080"
---
# <a name="list-visual-basic"></a>\<list> (Visual Basic)
Define uma lista ou tabela.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<list type="type">  
   <listheader>  
      <term>term</term>  
      <description>description</description>  
   </listheader>  
   <item>  
      <term>term</term>  
      <description>description</description>  
   </item>  
</list>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `type`  
 O tipo da lista. Deve ser um "Bullet" para uma lista com marcadores, "Number" para uma lista numerada ou "Table" para uma tabela de duas colunas.  
  
 `term`  
 Usado somente quando `type` é "Table". Um termo a definir, que é definido na marca de descrição.  
  
 `description`  
 Quando `type` é "Bullet" ou "Number", `description` é um item da lista quando `type` é "Table", `description` é a definição de `term` .  
  
## <a name="remarks"></a>Comentários  
 O `<listheader>` bloco define o título de uma tabela ou de uma lista de definições. Ao definir uma tabela, você só precisa fornecer uma entrada para `term` no título.  
  
 Cada item na lista é especificado com um `<item>` bloco. Ao criar uma lista de definições, você deve especificar `term` e `description` . No entanto, para uma tabela, lista com marcadores ou lista numerada, você só precisa fornecer uma entrada para `description` .  
  
 Uma lista ou tabela pode ter tantos `<item>` blocos quantos forem necessários.  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<list>` marca para definir uma lista com marcadores na seção de comentários.  
  
 [!code-vb[VbVbcnXmlDocComments#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#5)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
