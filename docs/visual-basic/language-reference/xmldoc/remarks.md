---
title: <remarks>
ms.date: 07/20/2015
helpviewer_keywords:
- <remarks> XML tag
- remarks XML tag
ms.assetid: c6241773-a7ed-41c9-9a8b-9722a0c606a9
ms.openlocfilehash: c57ddb870192bd94301f99eb71ad29526e8efc28
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400015"
---
# <a name="remarks-visual-basic"></a>\<remarks> (Visual Basic)
Especifica uma seção de comentários para o membro.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<remarks>description</remarks>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `description`  
 Uma descrição do membro.  
  
## <a name="remarks"></a>Comentários  
 Use a `<remarks>` marca para adicionar informações sobre um tipo, complementando as informações especificadas com [\<summary>](summary.md) .  
  
 Essas informações são exibidas no Pesquisador de objetos. Para obter informações sobre o pesquisador de objetos, consulte [exibindo a estrutura do código](/visualstudio/ide/viewing-the-structure-of-code).  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<remarks>` marca para explicar o que o `UpdateRecord` método faz.  
  
 [!code-vb[VbVbcnXmlDocComments#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
