---
title: Processar dados XML usando o modelo de dados XPath
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 536c6fce-1453-4654-9c72-bca54d47e081
ms.openlocfilehash: d449fe19640b19b1417c41b3a1ac7bd3a4de907a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84291286"
---
# <a name="process-xml-data-using-the-xpath-data-model"></a>Processar dados XML usando o modelo de dados XPath
O namespace <xref:System.Xml?displayProperty=nameWithType> fornece uma representação programática de documentos XML, fragmentos, nós ou conjuntos de nós na memória, usando as classes <xref:System.Xml.XmlDocument> ou <xref:System.Xml.XPath.XPathDocument>.  
  
 A classe <xref:System.Xml.XPath.XPathDocument> fornece uma representação rápida, somente leitura e na memória de um documento XML usando o modelo de dados XPath. A classe <xref:System.Xml.XmlDocument> fornece uma representação editável e na memória de um documento XML que implementa o núcleo de nível 1 do DOM (Modelo de Objeto de Documento) do W3C e o nível 2 do DOM principal. Ambas as classes implementam a interface <xref:System.Xml.XPath.IXPathNavigable> e retornam um objeto <xref:System.Xml.XPath.XPathNavigator> usado para selecionar, avaliar, navegar e, em alguns casos, editar os dados XML subjacentes.  
  
 As seções a seguir descrevem a funcionalidade da classe <xref:System.Xml.XPath.XPathNavigator> com base na classe que ela retorna.  
  
## <a name="in-this-section"></a>Nesta seção  
 [Lendo dados XML usando XPathDocument e XmlDocument](reading-xml-data-using-xpathdocument-and-xmldocument.md)  
 Descreve como criar um objeto da classe <xref:System.Xml.XPath.XPathDocument> somente leitura para ler um documento XML e como criar um objeto da classe <xref:System.Xml.XmlDocument> editável para ler e editar um documento XML. Este tópico também descreve como retornar um objeto <xref:System.Xml.XPath.XPathNavigator> de cada classe para navegar e editar um documento XML.  
  
 [Selecionando, avaliando e correspondente de dados XML usando XPathNavigator](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)  
 Descreve os métodos da classe <xref:System.Xml.XPath.XPathNavigator> usada para selecionar nós em um objeto <xref:System.Xml.XPath.XPathDocument> ou <xref:System.Xml.XmlDocument> usando uma consulta XPath, avaliar e examinar os resultados de uma expressão XPath e determinar se um nó em um documento XML corresponde uma expressão XPath determinada.  
  
 [Acessando dados XML usando XPathNavigator](accessing-xml-data-using-xpathnavigator.md)  
 Descreve os métodos da classe <xref:System.Xml.XPath.XPathNavigator> usada para navegar nós, extrair dados XML e acessar dados XML fortemente tipados em um objeto <xref:System.Xml.XPath.XPathDocument> ou <xref:System.Xml.XmlDocument>.  
  
 [Editando dados XML usando XPathNavigator](editing-xml-data-using-xpathnavigator.md)  
 Descreve os métodos da classe <xref:System.Xml.XPath.XPathNavigator> usada para inserir, modificar e remover os nós e os valores de um documento XML contido em um objeto <xref:System.Xml.XmlDocument>.  
  
 [Validação de esquema usando XPathNavigator](schema-validation-using-xpathnavigator.md)  
 Descreve as maneiras para validar o conteúdo XML contido em um objeto <xref:System.Xml.XPath.XPathDocument> ou de <xref:System.Xml.XmlDocument>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo DOM](process-xml-data-using-the-dom-model.md)
