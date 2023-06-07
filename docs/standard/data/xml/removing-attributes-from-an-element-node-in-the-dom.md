---
title: Removendo os atributos de um nó no elemento DOM
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 7ede6f9e-a3ac-49a4-8488-ab8360a44aa4
ms.openlocfilehash: 0ac8528d52ef09aab99c76aea9378c0188fa66d4
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84292014"
---
# <a name="removing-attributes-from-an-element-node-in-the-dom"></a>Removendo os atributos de um nó no elemento DOM
Há várias maneiras para remover os atributos. Uma técnica é removê-los de coleção de atributo. Para fazer isso, as seguintes etapas são executadas:  
  
1. Obter a coleção de atributo do elemento usando `XmlAttributeCollection attrs = elem.Attributes;`.  
  
2. Remova o atributo da coleção de atributo usando um dos três métodos:  
  
    - Use <xref:System.Xml.XmlAttributeCollection.Remove%2A> para remover um atributo específico.  
  
    - Use <xref:System.Xml.XmlAttributeCollection.RemoveAll%2A> para remover todos os atributos de coleção e para permitir que o elemento sem atributos.  
  
    - Use <xref:System.Xml.XmlAttributeCollection.RemoveAt%2A> para remover um atributo da coleção de atributo usando o número de índice.  
  
 Os seguintes métodos remove os atributos do nó do elemento.  
  
- Use <xref:System.Xml.XmlElement.RemoveAllAttributes%2A> para remover a coleção de atributo.  
  
- Use <xref:System.Xml.XmlElement.RemoveAttribute%2A> para remover por nome um único atributo de coleção.  
  
- Use <xref:System.Xml.XmlElement.RemoveAttributeAt%2A> para remover um único atributo pelo número de índice da coleção.  
  
 Uma alternativa é mais obter o elemento, obter o atributo de coleção de atributo, e remova diretamente o nó de atributo. Para obter o atributo de coleção de atributo, você pode usar um nome, `XmlAttribute attr = attrs["attr_name"];`, um índice `XmlAttribute attr = attrs[0];`, ou qualificando totalmente o nome com o namespace `XmlAttribute attr = attrs["attr_localName", "attr_namespace"]`.  
  
 Independentemente do método usado para remover os atributos, há limitações especiais em remover os atributos que são definidos como atributos padrão em Document type definition (DTD). Os atributos padrão não podem ser removidos a menos que o elemento que pertencem a for removido. Os atributos padrão são sempre atual para elementos que têm atributos padrões declarados. Removendo um padrão do atributo <xref:System.Xml.XmlAttributeCollection> ou de resultados de <xref:System.Xml.XmlElement> em um atributo de substituição inserido em <xref:System.Xml.XmlAttributeCollection> do elemento inicializado, o valor padrão que foi declarado. Se você tiver um elemento definido como `<book att1="1" att2="2" att3="3"></book>`, então você tiver um elemento de `book` com os três atributos padrões declarados. A implementação DOM (Modelo de Objeto de Documento) de XML garante que, contanto que esse elemento `book` exista, ele tenha esses três atributos padrões de `att1`, `att2` e `att3`.  
  
 Quando chamado com <xref:System.Xml.XmlAttribute>, o método de <xref:System.Xml.XmlAttributeCollection.RemoveAll%2A> define o valor do atributo para String.Empty, como um atributo não pode existir sem um valor.  
  
## <a name="see-also"></a>Veja também

- [XML Document Object Model (DOM)](xml-document-object-model-dom.md)
