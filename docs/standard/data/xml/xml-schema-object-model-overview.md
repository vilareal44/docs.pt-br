---
title: Visão geral do modelo de objeto de esquema XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 896a1e12-5655-42c6-8cdd-89c12862b34b
ms.openlocfilehash: 0358efdcc2e8b86f589eea312d791610da5238db
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290325"
---
# <a name="xml-schema-object-model-overview"></a>Visão geral do modelo de objeto de esquema XML
O modelo de objeto (SOM) de esquema no Microsoft.NET Framework é ricos API que permite a você criar, editar, e validar esquemas programaticamente. O SOM opera sobre documentos de esquema XML de forma semelhante à forma como Document Object Model (DOM) opera sobre documentos XML. Documentos de esquema XML são arquivos XML válidos, que carregados uma vez no SOM, transmitem significar sobre a estrutura e a validade de outros documentos XML que estão de acordo com o esquema.  
  
 Um esquema é um documento XML que define uma classe de documentos XML especificando a estrutura ou modelo de documentos XML para um esquema específico. Um esquema identifica as restrições no conteúdo de documentos XML, vocabulários e descreve regras (ou gramática) que documentos XML correspondentes devem seguir para ser considerado válidos esquema- com o esquema específico. Validação de um documento XML é o processo que garante que o documento está de acordo com a gramática especificada pelo esquema.  
  
 Os seguintes são maneiras que o SOM API no .NET Framework permite criar, editar, e para validar esquemas.  
  
- Esquemas válidos de carregar e de salvar a e arquivos.  
  
- Criar esquemas de memória usando classes fortemente tipadas.  
  
- Interagir com a classe de <xref:System.Xml.Schema.XmlSchemaSet> para armazenar em cachê, compilar, e recuperar esquemas.  
  
- Interagir com o método de <xref:System.Xml.XmlReader.Create%2A> da classe de <xref:System.Xml.XmlReader> para validar instância de documentos XML com esquemas.  
  
- Criar editores para criar e esquemas de manutenções.  
  
- Editar dinamicamente um esquema que pode ser seguido e salvo para uso na validação de instância de documentos XML.  
  
## <a name="the-schema-object-model"></a>O modelo de objeto de esquema  
 O SOM consiste em um extenso conjunto de classes no namespace de <xref:System.Xml.Schema?displayProperty=nameWithType> que corresponde a elementos em um esquema XML. Por exemplo, os mapeamentos de elemento de `<xsd:schema>...</xsd:schema>` a <xref:System.Xml.Schema.XmlSchema?displayProperty=nameWithType> classe, e qualquer informação que pode ser contido em um elemento de `<xsd:schema/>` pode ser representadas usando a classe de <xref:System.Xml.Schema.XmlSchema> . Da mesma forma, `<xsd:element>...</xsd:element>` e os elementos de `<xsd:attribute>...</xsd:attribute>` a classes de <xref:System.Xml.Schema.XmlSchemaElement?displayProperty=nameWithType> e de <xref:System.Xml.Schema.XmlSchemaAttribute?displayProperty=nameWithType> respectivamente. Esse mapeamento continua para todos os elementos de um esquema XML que cria um modelo de objeto de esquema XML no espaço de <xref:System.Xml.Schema> ilustrada no diagrama a seguir.  
  
 ![Modelo de objeto do System.Xml.Schema](./media/xml-schema-object-model-overview/xml-schema-object-model.gif)  
  
 Para obter mais informações sobre cada classe no namespace de <xref:System.Xml.Schema> , consulte a documentação de referência do <xref:System.Xml.Schema> na biblioteca de classes do .NET Framework.  
  
## <a name="see-also"></a>Veja também

- [Lendo e gravando esquemas XML](reading-and-writing-xml-schemas.md)
- [Compilando esquemas XML](building-xml-schemas.md)
- [Percorrer esquemas XML](traversing-xml-schemas.md)
- [Esquemas XML de edição](editing-xml-schemas.md)
- [Incluindo ou importando um esquema XML](including-or-importing-xml-schemas.md)
- [XmlSchemaSet para compilação de esquema](xmlschemaset-for-schema-compilation.md)
- [Compilação Infoset de pré esquema](post-schema-compilation-infoset.md)
