---
title: Transformações XSLT sobre diferentes armazena
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 369850e9-004a-45d2-b5c3-5060d9135adb
ms.openlocfilehash: 0eb98aad00227688df3e097ad674b44a29e5cb1a
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84281707"
---
# <a name="xslt-transformations-over-different-stores"></a>Transformações XSLT sobre diferentes armazena
> [!NOTE]
> A classe <xref:System.Xml.Xsl.XslTransform> está obsoleta no .NET Framework 2.0. Você pode executar a linguagem XSL Transformations (XSLT) usando a classe <xref:System.Xml.Xsl.XslCompiledTransform>. Confira [Usar a classe XslCompiledTransform](using-the-xslcompiledtransform-class.md) e [Migrar da classe XslTransform](migrating-from-the-xsltransform-class.md) para saber mais.  
  
 O ADO.NET e as classes XML em .NET Framework fornecem um modelo de programação unificado de acesso a dados. Os dados são representados como os dados XML, que são texto delimitado por marcas, e os dados relacionais, que são tabelas que consiste em linhas e em colunas. XML no .NET Framework ler dados XML do fluxo de dados em árvores do nó de DOM (Modelo de Objeto do Documento), em que os dados podem ser acessados por meio de programação, quando o ADO.NET fornecer os meios para acessar e manipular os dados relacionais dentro de um objeto <xref:System.Data.DataSet>.  
  
 Os DOM XML fornecem acesso a dados em documentos XML e classes adicionais para ler, gravar, e navega em documentos XML. Essas classes são suportadas no espaço de <xref:System.Xml> , que também unifica os DOM XML com os serviços de acesso a dados fornecidos pelo ADO.NET. <xref:System.Xml.XmlDataDocument> fornece acesso a dados relacional. <xref:System.Xml.XmlDataDocument> mapeia XML para dados relacionais no ADO.NET <xref:System.Data.DataSet>. Qualquer aplicativo baseado em .NET Framework pode usar as classes no namespace de <xref:System.Xml> para acessar e manipular documentos XML e dados relacionais em <xref:System.Xml.XmlDataDocument>. Essa implementação suporta arquiteturas n- múltiplas para coletando dados e de distribuição. Para saber mais, confira [Integração XML com dados relacionais e ADO.NET](xml-integration-with-relational-data-and-adonet.md).  
  
## <a name="see-also"></a>Veja também

- [A classe XslTransform implementa do processador XSLT](xsltransform-class-implements-the-xslt-processor.md)
