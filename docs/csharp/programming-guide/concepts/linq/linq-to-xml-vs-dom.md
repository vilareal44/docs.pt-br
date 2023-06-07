---
title: LINQ to XML vs. DOM (C#)
description: Saiba mais sobre as principais diferenças entre LINQ to XML e a API de programação XML do W3C Modelo de Objeto do Documento (DOM).
ms.date: 07/20/2015
ms.assetid: 51c0e3d2-c047-4e6a-a423-d61a882400b7
ms.openlocfilehash: f5fc3fd7869079d47d7c9031e3668afeed7a117b
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165341"
---
# <a name="linq-to-xml-vs-dom-c"></a>LINQ to XML vs. DOM (C#)
Esta seção descreve algumas das principais diferenças entre o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] e a atual API de programação de XML predominante, o W3C DOM (Modelo de Objeto do Documento).  
  
## <a name="new-ways-to-construct-xml-trees"></a>Novas maneiras de criar árvores XML  
 No W3C DOM, você cria uma árvore XML de baixo para cima; ou seja, você cria um documento, cria elementos e, em seguida, adiciona elementos ao documento.  
  
 Por exemplo, veja a seguir uma maneira comum de criar uma árvore XML usando a implementação da Microsoft do DOM, <xref:System.Xml.XmlDocument>:  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlElement name = doc.CreateElement("Name");  
name.InnerText = "Patrick Hines";  
XmlElement phone1 = doc.CreateElement("Phone");  
phone1.SetAttribute("Type", "Home");  
phone1.InnerText = "206-555-0144";
XmlElement phone2 = doc.CreateElement("Phone");  
phone2.SetAttribute("Type", "Work");  
phone2.InnerText = "425-555-0145";
XmlElement street1 = doc.CreateElement("Street1");
street1.InnerText = "123 Main St";  
XmlElement city = doc.CreateElement("City");  
city.InnerText = "Mercer Island";  
XmlElement state = doc.CreateElement("State");  
state.InnerText = "WA";  
XmlElement postal = doc.CreateElement("Postal");  
postal.InnerText = "68042";  
XmlElement address = doc.CreateElement("Address");  
address.AppendChild(street1);  
address.AppendChild(city);  
address.AppendChild(state);  
address.AppendChild(postal);  
XmlElement contact = doc.CreateElement("Contact");  
contact.AppendChild(name);  
contact.AppendChild(phone1);  
contact.AppendChild(phone2);  
contact.AppendChild(address);  
XmlElement contacts = doc.CreateElement("Contacts");  
contacts.AppendChild(contact);  
doc.AppendChild(contacts);  
```  
  
 Este estilo de codificação não fornece visualmente muitas informações sobre a estrutura da árvore XML. O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dá suporte a esta abordagem para construir uma árvore XML, mas também dá suporte a uma abordagem alternativa, a *construção funcional*. A construção funcional usa os construtores <xref:System.Xml.Linq.XElement> e de <xref:System.Xml.Linq.XAttribute> para criar uma árvore XML.  
  
 Veja como você construiria a mesma árvore XML usando a construção funcional [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]:  
  
```csharp  
XElement contacts =  
    new XElement("Contacts",  
        new XElement("Contact",  
            new XElement("Name", "Patrick Hines"),  
            new XElement("Phone", "206-555-0144",
                new XAttribute("Type", "Home")),  
            new XElement("phone", "425-555-0145",  
                new XAttribute("Type", "Work")),  
            new XElement("Address",  
                new XElement("Street1", "123 Main St"),  
                new XElement("City", "Mercer Island"),  
                new XElement("State", "WA"),  
                new XElement("Postal", "68042")  
            )  
        )  
    );  
```  
  
 Observe que o recuo do código para construir a árvore XML mostra a estrutura do XML subjacente.  
  
 Para obter mais informações, consulte [Criando árvores XML (C#)](./linq-to-xml-overview.md).  
  
## <a name="working-directly-with-xml-elements"></a>Trabalhando diretamente com Elementos XML  
 Quando você programa com XML, o foco principal é geralmente em elementos XML e talvez em atributos. No [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], você pode trabalhar diretamente com elementos e atributos XML. Por exemplo, você pode fazer o seguinte:  
  
- Criar elementos XML sem usar nenhum objeto de documento. Isso simplifica a programação quando você tem que trabalhar com partes de árvores XML.  
  
- Carregue objetos `T:System.Xml.Linq.XElement` diretamente de um arquivo XML.  
  
- Serialize objetos `T:System.Xml.Linq.XElement` para um arquivo ou fluxo.  
  
 Compare isso para o W3C DOM, no qual o documento XML é usado como um contêiner lógico para a árvore XML. No DOM, os nós XML, incluindo elementos e atributos, devem ser criados no contexto de um documento XML. Aqui está um fragmento de código para criar um elemento de nome DOM:  
  
```csharp  
XmlDocument doc = new XmlDocument();  
XmlElement name = doc.CreateElement("Name");  
name.InnerText = "Patrick Hines";  
doc.AppendChild(name);  
```  
  
 Se você quiser usar um elemento em vários documentos, deverá importar os nós nos documentos. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] evita essa camada de complexidade.  
  
 Ao usar LINQ to XML, você usará a classe <xref:System.Xml.Linq.XDocument> somente se você desejar adicionar um comentário ou uma instrução de processamento no nível raiz do documento.  
  
## <a name="simplified-handling-of-names-and-namespaces"></a>Tratamento simplificado de nomes e namespaces  
 Tratar nomes, namespaces e prefixos de namespace é geralmente uma parte complexa da programação de XML. O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] simplifica nomes e namespaces eliminando a necessidade de lidar com prefixos de namespace. Se quiser, você pode controlar prefixos de namespace. Mas se você optar por não controlar explicitamente os prefixos de namespace, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] atribuirá prefixos de namespace durante a serialização se forem necessários ou os serializará usando namespaces padrão se não forem. Se namespaces padrão forem usados, não haverá prefixos de namespace no documento resultante. Para obter mais informações, consulte [Visão geral de namespaces (LINQ to XML) (C#)](namespaces-overview-linq-to-xml.md).  
  
 Outro problema com os DOM é que ele não permite que você altere o nome de um nó. Em vez disso, você precisa criar um novo nó e copiar todos os nós filhos para ele, perdendo a identidade do nó original. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] evita esse problema permitindo que você defina a propriedade <xref:System.Xml.Linq.XName> em um nó.  
  
## <a name="static-method-support-for-loading-xml"></a>Suporte de método estático para carregar XML  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite que você carregue XML usando métodos estáticos em vez de métodos de instância. Isso simplifica o carregamento e a análise. Para obter mais informações, consulte [como carregar XML de um arquivo (C#)](./how-to-load-xml-from-a-file.md).  
  
## <a name="removal-of-support-for-dtd-constructs"></a>Remoção de suporte para construções de DTD  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] simplifica ainda mais a programação de XML removendo o suporte para entidades e referências a entidades. O gerenciamento de entidades é complexo e raramente é usado. Remover o suporte aumenta o desempenho e simplifica a interface de programação. Quando uma árvore [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] é populada, todas as entidades de DTD são expandidas.  
  
## <a name="support-for-fragments"></a>Suporte para fragmentos  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] não fornece um equivalente para a classe `XmlDocumentFragment`. Em muitos casos, no entanto, o conceito de `XmlDocumentFragment` pode ser tratado pelo resultado de uma consulta que é digitada como <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XNode> ou <xref:System.Collections.Generic.IEnumerable%601> de <xref:System.Xml.Linq.XElement>.  
  
## <a name="support-for-xpathnavigator"></a>Suporte para XPathNavigator  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dá suporte a <xref:System.Xml.XPath.XPathNavigator> por meio de métodos de extensão no namespace <xref:System.Xml.XPath?displayProperty=nameWithType>. Para obter mais informações, consulte <xref:System.Xml.XPath.Extensions?displayProperty=nameWithType>.  
  
## <a name="support-for-white-space-and-indentation"></a>Suporte para espaço em branco e recuo  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] trata espaços em branco de maneira mais simples que o DOM.  
  
 Um cenário comum é ler o XML recuado, criar uma árvore XML na memória sem nenhum nó de texto de espaço em branco (isto é, não preservar espaço em branco), executar algumas operações no XML e, em seguida, salvar o XML com recuo. Quando você serializa o XML com formatação, somente os espaços em branco significativos na árvore XML são preservados. Este é o comportamento padrão para [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)].  
  
 Outro cenário comum é ler e modificar XML que já foi recuado intencionalmente. Você pode não querer modificar este recuo de nenhuma forma. No [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], você pode fazer isso preservando o espaço em branco ao carregar ou analisar o XML e desabilitando a formatação ao serializar o XML.  
  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] armazena o espaço em branco como um nó de <xref:System.Xml.Linq.XText>, em vez de ter um tipo de nó especializado do <xref:System.Xml.XmlNodeType.Whitespace>, assim como o DOM.  
  
## <a name="support-for-annotations"></a>Suporte para anotações  
 Elementos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dão suporte a um conjunto extensível de anotações. Isso é útil para acompanhar informações variadas sobre um elemento, como informações de esquema, informações sobre se o elemento está associado a uma interface do usuário ou qualquer outro tipo de informações específicas do aplicativo. Para obter mais informações, consulte [Anotações LINQ to XML (C#)](./linq-to-xml-annotations.md).  
  
## <a name="support-for-schema-information"></a>Suporte para informações do esquema  
O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dá suporte para validação de XSD por meio de métodos de extensão no namespace <xref:System.Xml.Schema?displayProperty=nameWithType>. Você pode validar que uma árvore XML está em conformidade com XSD. Você pode preencher a árvore XML com o PSVI (post-schema-validation infoset). Para obter mais informações, consulte [como validar usando xsd](./how-to-validate-using-xsd-linq-to-xml.md) e <xref:System.Xml.Schema.Extensions> .
  
## <a name="see-also"></a>Confira também

- [Guia de introdução (LINQ to XML)](./linq-to-xml-overview.md)
