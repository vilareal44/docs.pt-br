---
title: Lendo dados XML usando XPathDocument e XmlDocument
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 5711b225-6aa2-4e4f-9898-19f2d518ad1a
ms.openlocfilehash: da1cb81c819e55f572e9faaabef4dd49ee7397de
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288674"
---
# <a name="reading-xml-data-using-xpathdocument-and-xmldocument"></a>Lendo dados XML usando XPathDocument e XmlDocument
Há duas maneiras para ler um documento XML no namespace <xref:System.Xml.XPath?displayProperty=nameWithType>. Uma é ler um documento XML usando a classe <xref:System.Xml.XPath.XPathDocument> somente leitura e a outra é ler um documento XML usando a classe <xref:System.Xml.XmlDocument> editável no namespace <xref:System.Xml?displayProperty=nameWithType>.  
  
## <a name="reading-xml-documents-using-the-xpathdocument-class"></a>Lendo documentos XML usando a classe XPathDocument  
 A classe <xref:System.Xml.XPath.XPathDocument> fornece uma representação rápida, somente leitura e na memória de um documento XML usando o modelo de dados XPath. As instâncias da classe <xref:System.Xml.XPath.XPathDocument> são criadas usando um de seus seis construtores. Esses construtores permitem que você leia um documento XML usando um objeto <xref:System.IO.Stream>, <xref:System.IO.TextReader> ou <xref:System.Xml.XmlReader>, bem como o caminho `string` para um arquivo XML.  
  
 O exemplo a seguir ilustra o uso do construtor <xref:System.Xml.XPath.XPathDocument> da classe `string` para ler um documento XML.  
  
```vb  
Dim document As XPathDocument = New XPathDocument("books.xml")  
```  
  
```csharp  
XPathDocument document = new XPathDocument("books.xml");  
```  
  
## <a name="reading-xml-documents-using-the-xmldocument-class"></a>Lendo documentos XML usando a classe XmlDocument  
 A classe <xref:System.Xml.XmlDocument> é uma representação editável e na memória de um documento XML que implementa o núcleo de nível 1 do DOM (Modelo de Objeto de Documento) do W3C e o nível 2 do DOM principal. As instâncias da classe <xref:System.Xml.XmlDocument> são criadas usando um de seus três construtores. Você pode criar um objeto <xref:System.Xml.XmlDocument> novo e vazio chamando o construtor de classe <xref:System.Xml.XmlDocument> sem parâmetros. Após chamar o construtor, use o método <xref:System.Xml.XmlDocument.Load%2A> para carregar dados XML no novo objeto <xref:System.Xml.XmlDocument> de um <xref:System.IO.Stream>, <xref:System.IO.TextReader> ou objeto <xref:System.Xml.XmlReader>, assim como o caminho `string` para um arquivo XML.  
  
 O exemplo a seguir ilustra o uso do construtor da classe <xref:System.Xml.XmlDocument> sem parâmetros e o método <xref:System.Xml.XmlDocument.Load%2A> para ler um documento XML.  
  
```vb  
Dim document As XmlDocument = New XmlDocument()  
document.Load("books.xml")  
```  
  
```csharp  
XmlDocument document = new XmlDocument();  
document.Load("books.xml");  
```  
  
## <a name="determining-the-encoding-of-an-xml-document"></a>Determinando a codificação de um documento XML  
 Um objeto <xref:System.Xml.XmlReader> pode ser usado para ler um documento XML e criar objetos <xref:System.Xml.XPath.XPathDocument> e <xref:System.Xml.XmlDocument> conforme mostrado nas seções anteriores. No entanto, um objeto <xref:System.Xml.XmlReader> pode ler os dados que não são codificados e, como um resultado, não fornece nenhuma informação de codificação.  
  
 A classe <xref:System.Xml.XmlTextReader> herda da classe <xref:System.Xml.XmlReader>, fornece informações de codificação usando a propriedade <xref:System.Xml.XmlTextReader.Encoding%2A> e pode ser usada para criar um objeto <xref:System.Xml.XPath.XPathDocument> ou objeto <xref:System.Xml.XmlDocument>.  
  
 Para obter mais informações sobre as informações de codificação fornecidas pela classe <xref:System.Xml.XmlTextReader>, consulte a propriedade <xref:System.Xml.XmlTextReader.Encoding%2A> na documentação de referência da classe <xref:System.Xml.XmlTextReader>.  
  
## <a name="creating-xpathnavigator-objects"></a>Criando objetos XPathNavigator  
 Depois que você tiver lido um documento XML em um objeto <xref:System.Xml.XPath.XPathDocument> ou <xref:System.Xml.XmlDocument>, poderá criar um objeto <xref:System.Xml.XPath.XPathNavigator> para selecionar, avaliar, navegar e, em alguns casos, editar os dados de XML subjacentes.  
  
 As classes <xref:System.Xml.XPath.XPathDocument> e <xref:System.Xml.XmlDocument>, além da classe <xref:System.Xml.XmlNode>, implementam a interface <xref:System.Xml.XPath.IXPathNavigable> do namespace <xref:System.Xml.XPath?displayProperty=nameWithType>. Como resultado, todas as três classes fornecem um método <xref:System.Xml.XPath.IXPathNavigable.CreateNavigator%2A> que retorna um objeto <xref:System.Xml.XPath.XPathNavigator>.  
  
### <a name="editing-xml-documents-using-the-xpathnavigator-class"></a>Editando documentos XML usando a classe XPathNavigator  
 Além de selecionar, avaliar e navegar dados XML, a classe <xref:System.Xml.XPath.XPathNavigator> pode ser usada para editar um documento XML em alguns casos, com base no objeto que o criou.  
  
 A classe <xref:System.Xml.XPath.XPathDocument> será somente leitura quando a classe <xref:System.Xml.XmlDocument> for editável e, como resultado, os objetos <xref:System.Xml.XPath.XPathNavigator> criados de um objeto <xref:System.Xml.XPath.XPathDocument> não podem ser usados para editar um documento XML, enquanto que os criados de um objeto <xref:System.Xml.XmlDocument> podem. A classe <xref:System.Xml.XPath.XPathDocument> deve ser usada para ler um documento XML somente. Em casos onde você precisa editar um documento XML ou precisa de acesso à funcionalidade adicional fornecida pela classe <xref:System.Xml.XmlDocument>, como tratamento de evento, a classe <xref:System.Xml.XmlDocument> deverá ser usada.  
  
 A propriedade <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> da classe <xref:System.Xml.XPath.XPathNavigator> especifica se um objeto <xref:System.Xml.XPath.XPathNavigator> pode editar dados XML.  
  
 A tabela a seguir descreve o valor da propriedade <xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> para cada classe.  
  
|Implementação de <xref:System.Xml.XPath.IXPathNavigable>|<xref:System.Xml.XPath.XPathNavigator.CanEdit%2A> Valor|  
|--------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|  
|<xref:System.Xml.XPath.XPathDocument>|`false`|  
|<xref:System.Xml.XmlDocument>|`true`|  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [Processar dados XML usando o modelo de dados XPath](process-xml-data-using-the-xpath-data-model.md)
- [Acessando dados XML usando XPathNavigator](accessing-xml-data-using-xpathnavigator.md)
- [Editando dados XML usando XPathNavigator](editing-xml-data-using-xpathnavigator.md)
- [Validação de esquema usando XPathNavigator](schema-validation-using-xpathnavigator.md)
