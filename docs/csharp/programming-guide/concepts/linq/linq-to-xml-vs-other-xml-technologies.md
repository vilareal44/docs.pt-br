---
title: LINQ to XML versus outros Technologies3 XML
description: Este artigo compara LINQ to XML a várias tecnologias XML, incluindo XmlReader, XSLT, MSXML e XmlLite, para ajudá-lo a decidir qual tecnologia usar.
ms.date: 07/20/2015
ms.assetid: 01b8e746-12d3-471d-b811-7539e4547784
ms.openlocfilehash: 0abe6021dae83df0db0d4116eb3c2919d024a62d
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165324"
---
# <a name="linq-to-xml-vs-other-xml-technologies"></a>LINQ to XML e outras tecnologias XML
Este tópico compara o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] às seguintes tecnologias XML: <xref:System.Xml.XmlReader>, XSLT, MSXML e XmlLite. Estas informações podem ajudá-lo a decidir qual tecnologia usar.  
  
 Para obter uma comparação de [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] com o modelo de objeto do documento (dom), consulte [LINQ to XML vs. dom (C#)](./linq-to-xml-vs-dom.md).  
  
## <a name="linq-to-xml-vs-xmlreader"></a>LINQ to XML vs. XmlReader  
 A classe <xref:System.Xml.XmlReader> é um analisador rápido, somente encaminhamento e não armazenado em cache.  
  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] é implementado sobre a classe <xref:System.Xml.XmlReader>; eles são fortemente integrados. No entanto, você também pode usar apenas <xref:System.Xml.XmlReader>.  
  
 Por exemplo, suponha que você esteja criando um serviço Web que analisará centenas de documentos XML por segundo e que os documentos tenham a mesma estrutura, o que significa que você só precisará criar uma única implementação do código para analisar o XML. Nesse caso, convém usar apenas <xref:System.Xml.XmlReader>.  
  
 Por outro lado, se você estiver construindo um sistema que analisa vários documentos XML menores, todos diferentes, convém aproveitar as vantagens das melhorias de produtividade que o [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] proporciona.  
  
## <a name="linq-to-xml-vs-xslt"></a>LINQ to XML vs. XSLT  
 Tanto [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] quanto XSLT contam com funcionalidades abrangentes de transformação de documentos XML. A linguagem XSLT é uma abordagem declarativa baseada em regras. Programadores avançados de XSLT criam XSLT em um estilo funcional de programação que enfatiza uma abordagem sem estado. As transformações podem ser criadas com o uso de funções puras que são implementadas sem efeitos colaterais. Essa abordagem funcional baseada em regras é desconhecida para muitos desenvolvedores e sua compreensão pode ser difícil e demorada.  
  
 A linguagem XSLT pode ser um sistema muito produtivo que gera aplicativos de alto desempenho. Por exemplo, algumas grandes empresas da Web usam XSLT para gerar HTML de XML extraído de uma variedade de repositórios de dados. O mecanismo XSLT gerenciado compila XSLT em código CLR e é executado ainda melhor em alguns cenários do que o mecanismo XSLT nativo.  
  
 No entanto, XSLT não aproveita as vantagens dos conhecimentos de C# e de Visual Basic que muitos desenvolvedores possuem. Ela requer que os desenvolvedores criem código em uma linguagem de programação diferente e complexa. O uso de dois sistemas de desenvolvimento não integrados, como C# (ou Visual Basic) e XSLT, resulta em sistemas de software que são mais difíceis de desenvolver e manter.  
  
 Quando você estiver totalmente familiarizado com as expressões de consulta [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)], verá que as transformações [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] constituem uma tecnologia avançada e fácil de usar. Basicamente, você forma seu documento XML usando construção funcional, extraindo dados de várias fontes, construindo objetos <xref:System.Xml.Linq.XElement> dinamicamente e montando o todo em uma nova árvore XML. A transformação pode gerar um documento completamente novo. Construir transformações no [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] é relativamente fácil e intuitivo e o código resultante é legível. Isso reduz custos de desenvolvimento e de manutenção.  
  
 O [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] não pretende substituir a linguagem XSLT. A linguagem XSLT ainda é a melhor ferramenta para transformações XML complicadas e centradas em documentos, especialmente se a estrutura do documento não estiver bem-definida.  
  
 A linguagem XSLT tem a vantagem de ser um padrão W3C (World Wide Web Consortium). Se você tiver um requisito de usar apenas tecnologias que sejam padrões, a linguagem XSLT pode ser mais apropriada.  
  
 XSLT é XML; portanto, ela pode ser manipulada programaticamente.  
  
## <a name="linq-to-xml-vs-msxml"></a>LINQ to XML vs. MSXML  
 MSXML é a tecnologia baseada em COM para processar XML que vem incluída no Microsoft Windows. A tecnologia MSXML fornece uma implementação nativa do DOM com suporte para XPath e XSLT. Ela também contém o analisador SAX2 baseado em eventos e não armazenado em cache.  
  
 A MSXML tem bom desempenho, é segura por padrão na maioria dos cenários e pode ser acessada no Internet Explorer para executar processamento XML do lado do cliente em aplicativos em estilo AJAX. A MSXML pode ser usada por meio de qualquer linguagem de programação que seja compatível com o COM, incluindo C++, JavaScript e Visual Basic 6.0.  
  
 A MSXML não é recomendada para uso em código gerenciado baseado em CLR (Common Language Runtime).  
  
## <a name="linq-to-xml-vs-xmllite"></a>LINQ to XML vs. XmlLite  
 XmlLite é um analisador de pull, somente encaminhamento e não armazenado em cache. Os desenvolvedores usam XmlLite principalmente com C++. Não é recomendado que os desenvolvedores usem XmlLite com código gerenciado.  
  
 A principal vantagem de XmlLite é ser um analisador XML leve, rápido e seguro na maioria dos cenários. Sua área de superfície de ameaças é muito pequena. Se você precisar analisar documentos não confiáveis e quiser protegê-los contra ataques de negação de serviço ou da exibição de dados, XmlLite pode ser uma boa opção.  
  
 O XmlLite não é integrado à LINQ (consulta integrada à linguagem). Ele não produz os aperfeiçoamentos de produtividade do programador que são a força motivada por trás do LINQ.  
  
## <a name="see-also"></a>Confira também

- [Guia de introdução (LINQ to XML)](./linq-to-xml-overview.md)
