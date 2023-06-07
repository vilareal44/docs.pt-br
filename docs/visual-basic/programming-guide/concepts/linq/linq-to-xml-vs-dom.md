---
title: LINQ to XML e DOM
ms.date: 07/20/2015
ms.assetid: 18c36130-d598-40b7-9007-828232252978
ms.openlocfilehash: 5813ca410e3e2b1ec284681451d0c0cec6f5e393
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84389326"
---
# <a name="linq-to-xml-vs-dom-visual-basic"></a>LINQ to XML vs. DOM (Visual Basic)
Esta seção descreve algumas das principais diferenças entre o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] e a atual API de programação de XML predominante, o W3C DOM (Modelo de Objeto do Documento).  
  
## <a name="new-ways-to-construct-xml-trees"></a>Novas maneiras de criar árvores XML  
 No W3C DOM, você cria uma árvore XML de baixo para cima; ou seja, você cria um documento, cria elementos e, em seguida, adiciona elementos ao documento.  
  
 Por exemplo, veja a seguir uma maneira comum de criar uma árvore XML usando a implementação da Microsoft do DOM, <xref:System.Xml.XmlDocument>:  
  
```vb  
Dim doc As XmlDocument = New XmlDocument()  
Dim name As XmlElement = doc.CreateElement("Name")  
name.InnerText = "Patrick Hines"  
Dim phone1 As XmlElement = doc.CreateElement("Phone")  
phone1.SetAttribute("Type", "Home")  
phone1.InnerText = "206-555-0144"  
Dim phone2 As XmlElement = doc.CreateElement("Phone")  
phone2.SetAttribute("Type", "Work")  
phone2.InnerText = "425-555-0145"  
Dim street1 As XmlElement = doc.CreateElement("Street1")  
street1.InnerText = "123 Main St"  
Dim city As XmlElement = doc.CreateElement("City")  
city.InnerText = "Mercer Island"  
Dim state As XmlElement = doc.CreateElement("State")  
state.InnerText = "WA"  
Dim postal As XmlElement = doc.CreateElement("Postal")  
postal.InnerText = "68042"  
Dim address As XmlElement = doc.CreateElement("Address")  
address.AppendChild(street1)  
address.AppendChild(city)  
address.AppendChild(state)  
address.AppendChild(postal)  
Dim contact As XmlElement = doc.CreateElement("Contact")  
contact.AppendChild(name)  
contact.AppendChild(phone1)  
contact.AppendChild(phone2)  
contact.AppendChild(address)  
Dim contacts As XmlElement = doc.CreateElement("Contacts")  
contacts.AppendChild(contact)  
doc.AppendChild(contacts)  
Console.WriteLine(doc.OuterXml)  
```  
  
 Este estilo de codificação não fornece visualmente muitas informações sobre a estrutura da árvore XML. O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dá suporte a esta abordagem para construir uma árvore XML, mas também dá suporte a uma abordagem alternativa, a *construção funcional*. No Visual Basic, a construção funcional usa literais XML para criar uma árvore XML.  
  
 Veja como você construiria a mesma árvore XML usando a construção funcional [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)]:  
  
```vb  
Dim contacts = _  
    <Contacts>  
        <Contact>  
            <Name>Patrick Hines</Name>  
            <Phone Type="Home">206-555-0144</Phone>  
            <Phone Type="Work">425-555-0145</Phone>  
            <Address>  
                <Street1>123 Main St</Street1>  
                <City>Mercer Island</City>  
                <State>WA</State>  
                <Postal>68042</Postal>  
            </Address>  
        </Contact>  
    </Contacts>  
```  
  
 Observe que o recuo do código para construir a árvore XML mostra a estrutura do XML subjacente.  
  
 Para obter mais informações, consulte [CREATING XML Trees (Visual Basic)](creating-xml-trees.md).  
  
## <a name="working-directly-with-xml-elements"></a>Trabalhando diretamente com Elementos XML  
 Quando você programa com XML, o foco principal é geralmente em elementos XML e talvez em atributos. No [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], você pode trabalhar diretamente com elementos e atributos XML. Por exemplo, você pode fazer o seguinte:  
  
- Criar elementos XML sem usar nenhum objeto de documento. Isso simplifica a programação quando você tem que trabalhar com partes de árvores XML.  
  
- Carregue objetos `T:System.Xml.Linq.XElement` diretamente de um arquivo XML.  
  
- Serialize objetos `T:System.Xml.Linq.XElement` para um arquivo ou fluxo.  
  
 Compare isso para o W3C DOM, no qual o documento XML é usado como um contêiner lógico para a árvore XML. No DOM, os nós XML, incluindo elementos e atributos, devem ser criados no contexto de um documento XML. Aqui está um fragmento de código para criar um elemento de nome DOM:  
  
```vb  
Dim doc As XmlDocument = New XmlDocument()  
Dim name As XmlElement = doc.CreateElement("Name")  
name.InnerText = "Patrick Hines"  
doc.AppendChild(name)  
```  
  
 Se você quiser usar um elemento em vários documentos, deverá importar os nós nos documentos. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] evita essa camada de complexidade.  
  
 Ao usar LINQ to XML, você usará a classe <xref:System.Xml.Linq.XDocument> somente se você desejar adicionar um comentário ou uma instrução de processamento no nível raiz do documento.  
  
## <a name="simplified-handling-of-names-and-namespaces"></a>Tratamento simplificado de nomes e namespaces  
 Tratar nomes, namespaces e prefixos de namespace é geralmente uma parte complexa da programação de XML. O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] simplifica nomes e namespaces eliminando a necessidade de lidar com prefixos de namespace. Se quiser, você pode controlar prefixos de namespace. Mas se você optar por não controlar explicitamente os prefixos de namespace, o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] atribuirá prefixos de namespace durante a serialização se forem necessários ou os serializará usando namespaces padrão se não forem. Se namespaces padrão forem usados, não haverá prefixos de namespace no documento resultante. Para obter mais informações, consulte [visão geral de namespaces (LINQ to XML) (Visual Basic)](namespaces-overview-linq-to-xml.md).  
  
 Outro problema com os DOM é que ele não permite que você altere o nome de um nó. Em vez disso, você precisa criar um novo nó e copiar todos os nós filhos para ele, perdendo a identidade do nó original. [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] evita esse problema permitindo que você defina a propriedade <xref:System.Xml.Linq.XName> em um nó.  
  
## <a name="static-method-support-for-loading-xml"></a>Suporte de método estático para carregar XML  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] permite que você carregue XML usando métodos estáticos em vez de métodos de instância. Isso simplifica o carregamento e a análise. Para obter mais informações, consulte [como carregar XML de um arquivo (Visual Basic)](how-to-load-xml-from-a-file.md).  
  
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
 Elementos [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dão suporte a um conjunto extensível de anotações. Isso é útil para acompanhar informações variadas sobre um elemento, como informações de esquema, informações sobre se o elemento está associado a uma interface do usuário ou qualquer outro tipo de informações específicas do aplicativo. Para obter mais informações, consulte [Anotações LINQ to XML (C#)](linq-to-xml-annotations.md).  
  
## <a name="support-for-schema-information"></a>Suporte para informações do esquema  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] dá suporte para validação de XSD por meio de métodos de extensão no namespace <xref:System.Xml.Schema?displayProperty=nameWithType>. Você pode validar que uma árvore XML está em conformidade com XSD. Você pode preencher a árvore XML com o PSVI (post-schema-validation infoset). Para obter mais informações, consulte [Como validar usando XSD](how-to-validate-using-xsd-linq-to-xml.md) e <xref:System.Xml.Schema.Extensions>.  
  
## <a name="see-also"></a>Confira também

- [Guia de introdução (LINQ to XML)](getting-started-linq-to-xml.md)
