---
title: Propriedade de Eixo do Atributo XML
ms.date: 07/20/2015
f1_keywords:
- vb.XmlPropertyAttributeAxis
helpviewer_keywords:
- attribute axis property [Visual Basic]
- Visual Basic code, accessing XML
- XML attribute axis property [Visual Basic]
- XML axis [Visual Basic], attribute
- XML [Visual Basic], accessing
ms.assetid: 7a4777e1-0618-4de9-9510-fb9ace2bf4db
ms.openlocfilehash: 3f60190a949856cb2bbc2eba09d097c09089bea7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408426"
---
# <a name="xml-attribute-axis-property-visual-basic"></a>Propriedade de eixo do atributo XML (Visual Basic)
Fornece acesso ao valor de um atributo para um <xref:System.Xml.Linq.XElement> objeto ou para o primeiro elemento em uma coleção de <xref:System.Xml.Linq.XElement> objetos.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
object.@attribute  
' -or-  
object.@<attribute>  
```  
  
## <a name="parts"></a>Partes  
 `object`  
 Obrigatórios. Um <xref:System.Xml.Linq.XElement> objeto ou uma coleção de <xref:System.Xml.Linq.XElement> objetos.  
  
 .@  
 Obrigatórios. Denota o início de uma propriedade de eixo de atributo.  
  
 <  
 Opcional. Indica o início do nome do atributo quando `attribute` não é um identificador válido no Visual Basic.  
  
 `attribute`  
 Obrigatórios. Nome do atributo a ser acessado, do formato [ `prefix` :] `name` .  
  
|Parte|Descrição|  
|----------|-----------------|  
|`prefix`|Opcional. Prefixo do namespace XML para o atributo. Deve ser um namespace XML global definido com uma `Imports` instrução.|  
|`name`|Obrigatórios. Nome do atributo local. Consulte [nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md).|  
  
 \>  
 Opcional. Indica o fim do nome do atributo quando `attribute` não é um identificador válido no Visual Basic.  
  
## <a name="return-value"></a>Valor Retornado  
 Uma cadeia de caracteres que contém o valor de `attribute` . Se o nome do atributo não existir, `Nothing` será retornado.  
  
## <a name="remarks"></a>Comentários  
 Você pode usar uma propriedade de eixo de atributo XML para acessar o valor de um atributo por nome de um <xref:System.Xml.Linq.XElement> objeto ou do primeiro elemento em uma coleção de <xref:System.Xml.Linq.XElement> objetos. Você pode recuperar um valor de atributo por nome ou adicionar um novo atributo a um elemento especificando um novo nome precedido pelo identificador @.  
  
 Quando você faz referência a um atributo XML usando o identificador @, o valor do atributo é retornado como uma cadeia de caracteres e você não precisa especificar explicitamente a <xref:System.Xml.Linq.XAttribute.Value%2A> propriedade.  
  
 As regras de nomenclatura para atributos XML diferem das regras de nomenclatura para identificadores de Visual Basic. Para acessar um atributo XML que tem um nome que não é um identificador de Visual Basic válido, coloque o nome entre colchetes angulares ( \< and > ).  
  
## <a name="xml-namespaces"></a>Namespaces XML  
 O nome em uma propriedade de eixo de atributo pode usar somente os prefixos de namespace XML declarados globalmente usando a `Imports` instrução. Ele não pode usar prefixos de namespace XML declarados localmente em literais de elemento XML. Para obter mais informações, consulte [instrução Imports (namespace XML)](../statements/imports-statement-xml-namespace.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como obter os valores dos atributos XML nomeados `type` de uma coleção de elementos XML que são nomeados `phone` .  
  
 [!code-vb[VbXMLSamples#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples5.vb#12)]  
  
 Esse código exibe o seguinte texto:  
  
 `<phoneTypes>`  
  
 `<type>home</type>`  
  
 `<type>work</type>`  
  
 `</phoneTypes>`  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como criar atributos para um elemento XML de forma declarativa, como parte do XML, e dinamicamente adicionando um atributo a uma instância de um <xref:System.Xml.Linq.XElement> objeto. O `type` atributo é criado declarativamente e o `owner` atributo é criado dinamicamente.  
  
 [!code-vb[VbXMLSamples#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples5.vb#44)]  
  
 Esse código exibe o seguinte texto:  
  
```xml  
<phone type="home" owner="Harris, Phyllis">206-555-0144</phone>  
```  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a sintaxe de colchete angular para obter o valor do atributo XML chamado `number-type` , que não é um identificador válido no Visual Basic.  
  
 [!code-vb[VbXMLSamples#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples5.vb#13)]  
  
 Esse código exibe o seguinte texto:  
  
 `Phone type: work`  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir declara `ns` como um prefixo de namespace XML. Em seguida, ele usa o prefixo do namespace para criar um literal XML e acessar o primeiro nó filho com o nome qualificado " `ns:name` ".  
  
 [!code-vb[VbXMLSamples#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbXMLSamples/VB/XMLSamples6.vb#14)]  
  
 Esse código exibe o seguinte texto:  
  
 `Phone type: home`  
  
## <a name="see-also"></a>Confira também

- <xref:System.Xml.Linq.XElement>
- [Propriedades do eixo XML](index.md)
- [Literais XML](../xml-literals/index.md)
- [Criando XML no Visual Basic](../../programming-guide/language-features/xml/creating-xml.md)
- [Nomes de elementos e atributos XML declarados](../../programming-guide/language-features/xml/names-of-declared-xml-elements-and-attributes.md)
