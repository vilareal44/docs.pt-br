---
title: Visão geral de classes LINQ to XML
ms.date: 07/20/2015
ms.assetid: f11b62b5-d522-4c23-92ae-23186dc16447
ms.openlocfilehash: 468435a0dfac64c85b3447f15f9cde63eb98aeb0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84368875"
---
# <a name="linq-to-xml-classes-overview-visual-basic"></a>Visão geral de classes de LINQ to XML (Visual Basic)
Este tópico fornece uma lista das classes [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] no namespace <xref:System.Xml.Linq> e uma breve descrição de cada.  
  
## <a name="linq-to-xml-classes"></a>Classes LINQ to XML  
  
### <a name="xattribute-class"></a>Classe XAttribute  
 <xref:System.Xml.Linq.XAttribute> representa um atributo XML. Para obter informações detalhadas e exemplos, consulte [visão geral da classe XAttribute (Visual Basic)](xattribute-class-overview.md).  
  
### <a name="xcdata-class"></a>Classe XCData  
 <xref:System.Xml.Linq.XCData> representa um nó de texto CDATA.  
  
### <a name="xcomment-class"></a>Classe XComment  
 <xref:System.Xml.Linq.XComment> representa um comentário XML.  
  
### <a name="xcontainer-class"></a>Classe XContainer  
 <xref:System.Xml.Linq.XContainer> é uma classe base abstrata para todos os nós que podem ter nós filho. As seguintes classes derivam da classe <xref:System.Xml.Linq.XContainer>:  
  
- <xref:System.Xml.Linq.XElement>  
  
- <xref:System.Xml.Linq.XDocument>  
  
### <a name="xdeclaration-class"></a>Classe XDeclaration  
 <xref:System.Xml.Linq.XDeclaration> representa uma declaração XML. Uma declaração XML é usada para declarar a versão XML e a codificação de um documento. Além disso, uma declaração XML especifica se o documento XML é autônomo. Se um documento é autônomo, não há nenhuma declaração de marcação externa, em um DTD externo, ou uma entidade de parâmetro externo referenciada do subconjunto interno.  
  
### <a name="xdocument-class"></a>Classe XDocument  
 <xref:System.Xml.Linq.XDocument> representa um documento XML. Para obter informações detalhadas e exemplos, consulte [visão geral da classe XDocument (Visual Basic)](xdocument-class-overview.md).  
  
### <a name="xdocumenttype-class"></a>Classe XDocumentType  
 <xref:System.Xml.Linq.XDocumentType> representa um DTD (definição de tipo de documento) de XML.  
  
### <a name="xelement-class"></a>Classe XElement  
 <xref:System.Xml.Linq.XElement> representa um elemento XML. Para obter informações detalhadas e exemplos, consulte [visão geral da classe XElement (Visual Basic)](xelement-class-overview.md).  
  
### <a name="xname-class"></a>Classe XName  
 <xref:System.Xml.Linq.XName> representa nomes de elementos (<xref:System.Xml.Linq.XElement>) e atributos (<xref:System.Xml.Linq.XAttribute>). Para obter informações detalhadas e exemplos, consulte [visão geral da classe XDocument (Visual Basic)](xdocument-class-overview.md).  
  
 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] é criado para simplificar ao máximo os nomes XML. Devido à sua complexidade, nomes XML geralmente são considerados um tópico avançado em XML. Pode-se afirmar que essa complexidade vem não dos namespaces, que os desenvolvedores usam regularmente em programação, mas dos prefixos de namespace. Prefixos de namespace podem ser úteis para reduzir os pressionamentos de teclas necessários para inserir XML ou para facilitar a leitura de XML. No entanto, os prefixos frequentemente são apenas um atalho para usar o namespace de XML completo e não são necessários na maioria dos casos. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] simplifica os nomes XML resolvendo todos os prefixos para o namespace de XML correspondente. Caso necessários, os prefixos estão disponíveis por meio do método <xref:System.Xml.Linq.XElement.GetPrefixOfNamespace%2A>.  
  
 É possível, se necessário, controlar os prefixos de namespace. Em algumas circunstâncias, se você estiver trabalhando com outros sistemas XML, como XSLT ou XAML, você precisará controlar os prefixos de namespace. Por exemplo, se você tiver uma expressão XPath que usa prefixos de namespace e está inserida em uma folha de estilos XSLT, verifique se o documento XML está serializado com prefixos de namespace que coincidem com os usados na expressão XPath.  
  
### <a name="xnamespace-class"></a>Classe XNamespace  
 <xref:System.Xml.Linq.XNamespace> representa um namespace para <xref:System.Xml.Linq.XElement> ou <xref:System.Xml.Linq.XAttribute>. Namespaces são um componente de um <xref:System.Xml.Linq.XName>.  
  
### <a name="xnode-class"></a>Classe XNode  
 <xref:System.Xml.Linq.XNode> é uma classe abstrata que representa os nós de uma árvore XML. As seguintes classes derivam da classe <xref:System.Xml.Linq.XNode>:  
  
- <xref:System.Xml.Linq.XText>  
  
- <xref:System.Xml.Linq.XContainer>  
  
- <xref:System.Xml.Linq.XComment>  
  
- <xref:System.Xml.Linq.XProcessingInstruction>  
  
- <xref:System.Xml.Linq.XDocumentType>  
  
### <a name="xnodedocumentordercomparer-class"></a>Classe XNodeDocumentOrderComparer  
 <xref:System.Xml.Linq.XNodeDocumentOrderComparer> fornece a funcionalidade para comparar nós para sua ordem do documento.  
  
### <a name="xnodeequalitycomparer-class"></a>Classe XNodeEqualityComparer  
 <xref:System.Xml.Linq.XNodeEqualityComparer> fornece a funcionalidade para comparar nós para igualdade de valor.  
  
### <a name="xobject-class"></a>Classe XObject  
 <xref:System.Xml.Linq.XObject> é uma classe base abstrata de <xref:System.Xml.Linq.XNode> e <xref:System.Xml.Linq.XAttribute>. Ele fornece a anotação e a funcionalidade de evento.  
  
### <a name="xobjectchange-class"></a>Classe XObjectChange  
 <xref:System.Xml.Linq.XObjectChange> especifica o tipo de evento quando um evento é gerado para um <xref:System.Xml.Linq.XObject>.  
  
### <a name="xobjectchangeeventargs-class"></a>Classe XObjectChangeEventArgs  
 <xref:System.Xml.Linq.XObjectChangeEventArgs> fornece dados para os eventos de <xref:System.Xml.Linq.XObject.Changing> e de <xref:System.Xml.Linq.XObject.Changed>.  
  
### <a name="xprocessinginstruction-class"></a>Classe XProcessingInstruction  
 <xref:System.Xml.Linq.XProcessingInstruction> representa uma instrução de processamento XML. Uma instrução de processamento comunica informações a um aplicativo que processa o XML.  
  
### <a name="xtext-class"></a>Classe XText  
 <xref:System.Xml.Linq.XText> representa um nó de texto. Na maioria dos casos, você não tem que usar esta classe. Essa classe é usada principalmente para conteúdo misto.  
  
## <a name="see-also"></a>Confira também

- [Visão geral da programação de LINQ to XML (Visual Basic)](linq-to-xml-programming-overview.md)
