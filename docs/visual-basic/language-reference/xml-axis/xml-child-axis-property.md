---
title: Propriedade do Eixo Filho XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyChildAxis
helpviewer_keywords:
- Visual Basic code, accessing XML
- XML axis [Visual Basic], child
- child axis property [Visual Basic]
- XML child axis property [Visual Basic]
- XML [Visual Basic], accessing
ms.assetid: 89a59d00-985e-4f5c-b59f-29b47bad11cb
ms.openlocfilehash: 90dc22d12be5566fa1ee40f6b0e48eff8088e67b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400260"
---
# <a name="xml-child-axis-property-visual-basic"></a>Propriedade do eixo filho XML (Visual Basic)
Fornece acesso aos descendentes de um dos seguintes: um objeto de <xref:System.Xml.Linq.XElement> , um objeto de <xref:System.Xml.Linq.XDocument> , uma coleção de objetos <xref:System.Xml.Linq.XElement> , ou uma coleção de <xref:System.Xml.Linq.XDocument> objeto.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
object.<child>  
```  
  
## <a name="parts"></a>Partes  
  
|Termo|Definição|  
|---|---|  
|`object`|Obrigatórios. Um <xref:System.Xml.Linq.XElement> objeto, um <xref:System.Xml.Linq.XDocument> objeto, uma coleção de <xref:System.Xml.Linq.XElement> objetos ou uma coleção de <xref:System.Xml.Linq.XDocument> objetos.|  
|. <|Obrigatórios. Denota o início de uma propriedade de eixo filho.|  
|`child`|Obrigatórios. Nome dos nós filho a serem acessados, do formulário `[prefix:]name` .<br /><br /> -   `Prefix`Adicional. Prefixo do namespace XML para o nó filho. Deve ser um namespace XML global definido com uma `Imports` instrução.<br />-   `Name`Necessária. Nome do nó filho local. Consulte [nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md).|  
|>|Obrigatórios. Denota o final de uma propriedade do eixo filho.|  
  
## <a name="return-value"></a>Valor Retornado  
 Uma coleção de objetos <xref:System.Xml.Linq.XElement>.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar uma propriedade de eixo filho XML para acessar nós filho pelo nome de <xref:System.Xml.Linq.XElement> um <xref:System.Xml.Linq.XDocument> objeto ou ou de uma coleção de <xref:System.Xml.Linq.XElement> <xref:System.Xml.Linq.XDocument> objetos ou. Use a `Value` propriedade XML para acessar o valor do primeiro nó filho na coleção retornada. Para obter mais informações, consulte [propriedade de valor XML](xml-value-property.md).  
  
 O compilador Visual Basic converte Propriedades de eixo filho em chamadas para o <xref:System.Xml.Linq.XContainer.Elements%2A> método.  
  
## <a name="xml-namespaces"></a>Namespaces XML  
 O nome em uma propriedade de eixo filho pode usar somente os prefixos de namespace XML declarados globalmente com a `Imports` instrução. Ele não pode usar prefixos de namespace XML declarados localmente em literais de elemento XML. Para obter mais informações, consulte [instrução Imports (namespace XML)](../statements/imports-statement-xml-namespace.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como acessar os nós filho chamados `phone` a partir do `contact` objeto.  
  
 [!code-vb[VbXMLSamples#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples7.vb#17)]  
  
 Esse código exibe o seguinte texto:  
  
 `Home Phone = 206-555-0144`  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como acessar os nós filho chamados `phone` da coleção retornada pela `contact` Propriedade eixo filho do `contacts` objeto.  
  
 [!code-vb[VbXMLSamples#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples7.vb#18)]  
  
 Esse código exibe o seguinte texto:  
  
 `Home Phone = 206-555-0144`  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir declara `ns` como um prefixo de namespace XML. Em seguida, ele usa o prefixo do namespace para criar um literal XML e acessar o primeiro nó filho com o nome qualificado `ns:name` .  
  
 [!code-vb[VbXMLSamples#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples8.vb#19)]  
  
 Esse código exibe o seguinte texto:  
  
 `Patrick Hines`  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement>
- [Propriedades do eixo XML](index.md)
- [Literais XML](../xml-literals/index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
- [Nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
