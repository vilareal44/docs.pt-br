---
title: <include>
ms.date: 07/20/2015
helpviewer_keywords:
- include XML tag
- <include> XML tag
ms.assetid: ba8e9173-82cd-460b-8938-a075a2dfb36d
ms.openlocfilehash: 78a10624107cea349b01f484c641190a945dbd7e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400105"
---
# <a name="include-visual-basic"></a>\<include> (Visual Basic)
Refere-se a outro arquivo que descreve os tipos e membros em seu código-fonte.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<include file="filename" path="tagpath[@name='id']" />  
```  
  
## <a name="parameters"></a>Parâmetros  
 `filename`  
 Obrigatórios. O nome do arquivo que contém a documentação. O nome do arquivo pode ser qualificado com um caminho. Coloque entre `filename` aspas duplas ("").  
  
 `tagpath`  
 Obrigatórios. O caminho das marcas em `filename` que leva à marca `name`. Coloque o caminho entre aspas duplas ("").  
  
 `name`  
 Obrigatórios. O especificador de nome na marca que precede os comentários. `Name`terá um `id` .  
  
 `id`  
 Obrigatórios. A ID da marca que precede os comentários. Coloque a ID entre aspas simples (' ').  
  
## <a name="remarks"></a>Comentários  
 Use a `<include>` marca para fazer referência a comentários em outro arquivo que descreva os tipos e membros em seu código-fonte. Essa é uma alternativa para inserir comentários de documentação diretamente em seu arquivo de código-fonte.  
  
 A `<include>` marca usa a recomendação da versão 1,0 do W3C XML Path Language (XPath). Para obter mais informações sobre maneiras de personalizar seu `<include>` uso, consulte <https://www.w3.org/TR/xpath> .  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `<include>` marca para importar comentários de documentação de membro de um arquivo chamado `commentFile.xml` .  
  
 [!code-vb[VbVbcnXmlDocComments#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#4)]  
  
 O formato do `commentFile.xml` é o seguinte.  
  
```xml  
<Docs>  
<Members name="Open">  
<summary>Opens a file.</summary>  
<param name="filename">File name to open.</param>  
</Members>  
<Members name="Close">  
<summary>Closes a file.</summary>  
<param name="filename">File name to close.</param>  
</Members>  
</Docs>  
```  
  
## <a name="see-also"></a>Confira também

- [Marcações de Comentário XML](index.md)
