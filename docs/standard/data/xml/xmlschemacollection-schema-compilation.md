---
title: Compilação do esquema de XmlSchemaCollection
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 76f28770-7126-428f-9ed5-7b5ae8bad5ee
ms.openlocfilehash: 3d517652665d6d0693e141d623483ff8946bbbf4
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290234"
---
# <a name="xmlschemacollection-schema-compilation"></a>Compilação do esquema de XmlSchemaCollection
**XmlSchemaCollection** é um cache ou uma biblioteca em que esquemas XDR (XML-Data Reduced) e XSD (XML Schema definition language) podem ser armazenados e validados. **XmlSchemaCollection** melhora o desempenho armazenando em cache esquemas na memória em vez de acessá-los de um arquivo ou de uma URL.  
  
> [!NOTE]
> Embora a classe de **XmlSchemaCollection** armazene esquemas XDR e XML, qualquer método e propriedade que receba ou retorne um objeto **XmlSchema** dá suporte somente a esquemas XML.  
  
> [!IMPORTANT]
> A classe <xref:System.Xml.Schema.XmlSchemaCollection> agora está obsoleta e foi substituída pela classe <xref:System.Xml.Schema.XmlSchemaSet>. Para saber mais sobre a classe <xref:System.Xml.Schema.XmlSchemaSet>, veja [XmlSchemaSet para compilação de esquema](xmlschemaset-for-schema-compilation.md).  
  
## <a name="add-schemas-to-the-collection"></a>Adicionar à coleção esquemas  
 Os esquemas são carregados na coleção usando o método **Add** de **XmlSchemaCollection**, quando o esquema é associado ao URI de um namespace. Para esquemas XML, URI do namespace será normalmente o namespace de destino para o esquema. Para esquemas XDR, o URI de namespace é namespace especificado quando o esquema foi adicionado à coleção.  
  
## <a name="check-for-a-schema-in-the-collection"></a>Verificação para um esquema na coleção  
 Você pode verificar se um esquema está na coleção usando o método **Contains**. O método **Contains** utiliza um objeto **XmlSchema** (somente para esquemas XML) ou uma cadeia de caracteres que representa o URI de namespace associado ao esquema (para esquemas XML e XDR).  
  
## <a name="retrieve-a-schema-from-the-collection"></a>Recuperar um esquema de coleção  
 Você pode recuperar um esquema de coleção usando a propriedade **Item**. A propriedade **Item** usa uma cadeia de caracteres que representa o URI de namespace associado ao esquema, normalmente o namespace de destino, e retorna um objeto **XmlSchema**. A propriedade **Item** se aplica somente a esquemas XML. O valor de retorno é sempre uma referência nula para esquemas XDR porque eles não têm um modelo de objeto disponível.  
  
## <a name="validate-xml-documents-using-xmlschemacollection"></a>Validar documentos XML usando XmlSchemaCollection  
 Você pode validar um documento de instância de XML usando **XmlSchemaCollection** criando o objeto **XmlSchemaCollection**, adicionando seus esquemas à coleta e definindo a propriedade **Schemas** em **XmlValidatingReader** para atribuir o **XmlSchemaCollection** criado a **XmlValidatingReader**.  
  
### <a name="improved-performance"></a>Desempenho aprimorado  
 Se você está validando mais de um documento com o mesmo esquema, é recomendável usar **XmlSchemaCollection**, pois fornece melhor desempenho armazenando em cache esquemas na memória.  
  
 O exemplo de código a seguir cria o objeto **XmlSchemaCollection**, adiciona esquemas à coleta e defina a propriedade **Schemas**.  
  
```vb  
Dim tr as XmlTextReader = new XmlTextReader("Books.xml")  
Dim vr as XmlValidatingReader = new XmlValidatingReader(tr)  
Dim xsc as XmlSchemaCollection = new XmlSchemaCollection  
xsc.Add("urn:bookstore-schema", "Books.xsd")  
vr.Schemas.Add(xsc)  
```  
  
```csharp  
XmlTextReader tr = new XmlTextReader("Books.xml");  
XmlValidatingReader vr = new XmlValidatingReader(tr);  
XmlSchemaCollection xsc = new XmlSchemaCollection();  
xsc.Add("urn:bookstore-schema", "Books.xsd");
vr.Schemas.Add(xsc);  
```  
  
## <a name="see-also"></a>Veja também

- [Validação de XDR com XmlSchemaCollection](xdr-validation-with-xmlschemacollection.md)
- [Validação de XSD (esquema XML) com XmlSchemaCollection](xml-schema-xsd-validation-with-xmlschemacollection.md)
