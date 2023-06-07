---
title: Propriedade do Indexador de Extensão
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyExtensionIndexer
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML extension indexer [Visual Basic]
- extension indexer [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: a16a4b13-54be-432c-82b3-a87091464ada
ms.openlocfilehash: c91061d49e22e648b7bf75a812071b352793abcb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401336"
---
# <a name="extension-indexer-property-visual-basic"></a>Propriedade do indexador de extensão (Visual Basic)
Fornece acesso aos elementos individuais em uma coleção.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
object(index)  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`object`|Obrigatórios. Uma coleção passível de consulta. Ou seja, uma coleção que implementa <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Linq.IQueryable%601> .|  
|(|Obrigatórios. Denota o início da Propriedade do indexador.|  
|`index`|Obrigatórios. Uma expressão de inteiro que especifica a posição de base zero de um elemento da coleção.|  
|)|Obrigatórios. Denota o final da Propriedade do indexador.|  
  
## <a name="return-value"></a>Valor Retornado  
 O objeto do local especificado na coleção ou `Nothing` se o índice está fora do intervalo.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar a propriedade do indexador de extensão para acessar elementos individuais em uma coleção. Essa propriedade do indexador normalmente é usada na saída das propriedades do eixo XML. As propriedades do eixo XML filho e do XML descendente retornam coleções de <xref:System.Xml.Linq.XElement> objetos ou um valor de atributo.  
  
 O compilador Visual Basic converte Propriedades do indexador de extensão em chamadas para o `ElementAtOrDefault` método. Ao contrário de um indexador de matriz, o `ElementAtOrDefault` método retorna `Nothing` se o índice está fora do intervalo. Esse comportamento é útil quando você não pode determinar facilmente o número de elementos em uma coleção.  
  
 Essa propriedade do indexador é como uma propriedade de extensão para coleções que implementam <xref:System.Collections.Generic.IEnumerable%601> ou <xref:System.Linq.IQueryable%601> : ela será usada somente se a coleção não tiver um indexador ou uma propriedade padrão.  
  
 Para acessar o valor do primeiro elemento em uma coleção de <xref:System.Xml.Linq.XElement> objetos ou <xref:System.Xml.Linq.XAttribute> , você pode usar a propriedade XML `Value` . Para obter mais informações, consulte [propriedade de valor XML](xml-value-property.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como usar o indexador de extensão para acessar o segundo nó filho em uma coleção de <xref:System.Xml.Linq.XElement> objetos. A coleção é acessada usando a propriedade do eixo filho, que obtém todos os elementos filho nomeados `phone` no `contact` objeto.  
  
 [!code-vb[VbXMLSamples#24](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples11.vb#24)]  
  
 Esse código exibe o seguinte texto:  
  
 `Second phone number: 425-555-0145`  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement>
- [Propriedades do eixo XML](index.md)
- [Literais XML](../xml-literals/index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
- [Propriedade do Valor XML](xml-value-property.md)
