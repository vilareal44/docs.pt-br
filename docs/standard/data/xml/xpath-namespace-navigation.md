---
title: Navegação do namespace XPath
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 06cc7abb-7416-415c-9dd6-67751b8cabd5
ms.openlocfilehash: dce7d81d4249cb40c3be6dee4b8bd25951ccb10a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84283202"
---
# <a name="xpath-namespace-navigation"></a>Navegação do namespace XPath
Para usar consultas XPath com documentos XML, você precisará endereçar corretamente namespaces XML e os elementos contidos por espaços. Namespaces impedem ambiguidades que podem ocorrer quando os nomes são usados em mais de um contexto; por exemplo, o nome `ID` pode referir-se a mais de um identificador associado com os diferentes elementos de um documento XML. A sintaxe do namespace especifica URIs, nomes, e prefixos que distinguiem os elementos de um documento XML.  
  
 O exemplo neste tópico demonstra o uso dos prefixos em navegar em um documento XML com <xref:System.Xml.XPath.XPathNavigator>. Para obter mais informações sobre namespaces e sintaxe, consulte [arquivos XML: Noções básicas sobre namespaces XML](https://docs.microsoft.com/previous-versions/dotnet/articles/bb986013(v=msdn.10)).  
  
## <a name="namespace-declarations"></a>Declarações de namespace  
 As declarações namespace tornam os elementos de um documento XML distinguíveis e endereçáveis ao usar uma instância de <xref:System.Xml.XPath.XPathNavigator>. Prefixos de namespace fornece uma breve sintaxe para namespaces de resolução.  
  
 Os prefixos são definidos pelo formulário: `<e:Envelope xmlns:e=http://schemas.xmlsoap.org/soap/envelope/>.` nesta sintaxe o prefixo “`e`” é uma abreviação para o URI formal de namespace. Você pode identificar o elemento de `Body` como um membro de namespace de `Envelope` usando a sintaxe: `e:Body`.  
  
 O seguinte documento XML será referenciado como `response.xml` no exemplo de navegação na próxima seção.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<e:Envelope xmlns:e="http://schemas.xmlsoap.org/soap/envelope/">  
  <e:Body>  
    <s:Search xmlns:s="http://schemas.microsoft.com/v1/Search">  
      <r:request xmlns:r="http://schemas.microsoft.com/v1/Search/metadata"
                 xmlns:i="http://www.w3.org/2001/XMLSchema-instance">  
      </r:request>  
    </s:Search>  
  </e:Body>  
</e:Envelope>  
```  
  
## <a name="navigation-by-namespace-prefix"></a>Navegação pelo prefixo de namespace  
 O código nesta seção usa <xref:System.Xml.XPath.XPathNavigator> e objetos de <xref:System.Xml.XmlNamespaceManager> para selecionar o elemento de `Search` de documento XML na seção anterior. A consulta `xpath` inclui prefixos de namespace em cada elemento no caminho. Especificando a identidade precisa namespaces que contêm cada elemento garante a navegação correta para o elemento de `Search` pelo método de <xref:System.Xml.XPath.XPathNavigator.SelectSingleNode%2A> .  
  
```csharp  
using (XmlReader reader = XmlReader.Create("response.xml"))  
{  
    XPathDocument doc = new XPathDocument(reader);  
    XPathNavigator nav = doc.CreateNavigator();
  
    XmlNamespaceManager nsmgr = new XmlNamespaceManager(nav.NameTable);  
    nsmgr.AddNamespace("e", @"http://schemas.xmlsoap.org/soap/envelope/");  
    nsmgr.AddNamespace("s", @"http://schemas.microsoft.com/v1/Search");  
    nsmgr.AddNamespace("r", @"http://schemas.microsoft.com/v1/Search/metadata");  
    nsmgr.AddNamespace("i", @"http://www.w3.org/2001/XMLSchema-instance");  
  
    string xpath = "/e:Envelope/e:Body/s:Search";  
  
    XPathNavigator element = nav.SelectSingleNode(xpath, nsmgr);  
  
    Console.WriteLine("Element Prefix:" + element.Prefix +
    " Local name:" + element.LocalName);  
    Console.WriteLine("Namespace URI: " + element.NamespaceURI);  
}  
```  
  
 A precisão de namespaces totalmente aplicáveis e de nomes é mais de uma conveniência. Pouca experimentação com a definição de documento e o código nos exemplos anteriores verificarão a navegação sem exceções totalmente qualificados de gera os nomes de elemento. Por exemplo, a definição de elemento: `<Search xmlns="http://schemas.microsoft.com/v1/Search">`, e consulta: a cadeia de caracteres `xpath = "/s:Envelope/s:Body/Search";` sem o prefixo do namespace no elemento de `Search` retorna `null` em vez do elemento de `Search` .  
  
## <a name="see-also"></a>Veja também

- [Acessando dados XML usando XPathNavigator](accessing-xml-data-using-xpathnavigator.md)
- [Selecionando, avaliando e correspondente de dados XML usando XPathNavigator](selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)
