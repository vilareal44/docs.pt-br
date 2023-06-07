---
title: <permission>
ms.date: 07/20/2015
helpviewer_keywords:
- <permission> XML tag
- permission XML tag
ms.assetid: 0edf0500-5cd7-49c0-9255-64c48f972b77
ms.openlocfilehash: b3acec04060367a0b9e54b19c0106644d028357b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400028"
---
# <a name="permission-visual-basic"></a>\<permission> (Visual Basic)
Especifica uma permissão necessária para o membro.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<permission cref="member">description</permission>  
```  
  
## <a name="parameters"></a>Parâmetros  
 `member`  
 Uma referência a um membro ou campo disponível para ser chamado do ambiente de compilação atual. O compilador verifica se o elemento de código fornecido existe e converte `member` no nome de elemento canônico no XML de saída. Coloque `member` entre aspas ("").  
  
 `description`  
 Uma descrição do acesso ao membro.  
  
## <a name="remarks"></a>Comentários  
 Use a `<permission>` marca para documentar o acesso de um membro. Use a <xref:System.Security.PermissionSet> classe para especificar o acesso a um membro.  
  
 Compile com [-Doc](../../reference/command-line-compiler/doc.md) para processar comentários de documentação em um arquivo.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<permission>` marca para descrever que o <xref:System.Security.Permissions.FileIOPermission> é exigido pelo `ReadFile` método.  
  
 [!code-vb[VbVbcnXmlDocComments#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#7)]  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
