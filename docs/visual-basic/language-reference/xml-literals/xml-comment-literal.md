---
title: Literal de Comentário XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlLiteralComment
helpviewer_keywords:
- comment literal [Visual Basic]
- XML comments, adding [Visual Basic]
- XML comment literal [Visual Basic]
- XML literals [Visual Basic], comment
ms.assetid: 634c1cee-5e01-48d0-88d7-2dd55e4a9e52
ms.openlocfilehash: 93c1346e54106b93f3932a494dea85d082ec994d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400209"
---
# <a name="xml-comment-literal-visual-basic"></a>Literal de comentário XML (Visual Basic)
Um literal que representa um <xref:System.Xml.Linq.XComment> objeto.  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<!-- content -->  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`<!--`|Obrigatórios. Denota o início do comentário XML.|  
|`content`|Obrigatórios. Texto a ser exibido no comentário XML. Não pode conter uma série de dois hifens (--) ou terminar com um hífen adjacente à marca de fechamento.|  
|`-->`|Obrigatórios. Denota o final do comentário XML.|  
  
## <a name="return-value"></a>Valor Retornado  
 Um objeto <xref:System.Xml.Linq.XComment>.  
  
## <a name="remarks"></a>Comentários  
 Literais de comentário XML não contêm conteúdo de documento; Eles contêm informações sobre o documento. A seção de comentário XML termina com a sequência "-->". Isso implica nos seguintes pontos:  
  
- Você não pode usar uma expressão inserida em um literal de comentário XML porque os delimitadores de expressão inseridos são conteúdo de comentário XML válido.  
  
- As seções de comentário XML não podem ser aninhadas porque `content` não podem conter o valor "-->".  
  
 Você pode atribuir um literal de comentário XML a uma variável ou pode incluí-lo em um literal de elemento XML.  
  
> [!NOTE]
> Um literal XML pode abranger várias linhas sem usar caracteres de continuação de linha. Esse recurso permite que você copie o conteúdo de um documento XML e cole-o diretamente em um programa Visual Basic.  
  
 O compilador Visual Basic converte o literal de comentário XML em uma chamada para o <xref:System.Xml.Linq.XComment.%23ctor%2A> Construtor.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria um comentário XML que contém o texto "Este é um comentário".  
  
 [!code-vb[VbXMLSamples#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples4.vb#9)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XComment>
- [Literal do Elemento XML](xml-element-literal.md)
- [Literais XML](index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
