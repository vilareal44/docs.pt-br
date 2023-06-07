---
title: A ferramenta de definição de esquema XML e a serialização XML
description: A ferramenta de definição de esquema XML gera arquivos de classe C# ou Visual Basic para um esquema XSD e gera um esquema XML de uma biblioteca ou arquivo executável.
ms.date: 03/30/2017
helpviewer_keywords:
- Xsd.exe
- XML serialization, XML Schema Definition tool
- XML Schema Definition tool
- serialization, XML Schema Definition tool
ms.assetid: 3c03f855-f931-47ff-bbc6-50c0367a16e4
ms.openlocfilehash: c88770403d4c2abfb5ce99f44ea1bec1f8337545
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "84279770"
---
# <a name="the-xml-schema-definition-tool-and-xml-serialization"></a>A ferramenta de definição de esquema XML e a serialização XML

A ferramenta de definição de esquema XML ([XSD. exe)](xml-schema-definition-tool-xsd-exe.md)é instalada junto com as ferramentas de .NET Framework como parte do &reg; SDK (Software Development Kit) do Windows. A ferramenta é criada principalmente para duas finalidades:  
  
- Para gerar arquivos de classe C# ou Visual Basic que estejam em conformidade com um esquema específico de linguagem XSD. A ferramenta usa um esquema XML como um argumento e gera um arquivo que contém um número de classes que, quando serializadas com o <xref:System.Xml.Serialization.XmlSerializer>, estão em conformidade com o esquema. Para obter informações sobre como usar a ferramenta para gerar classes em conformidade com um esquema específico, consulte [Como usar a Ferramenta de Definição de Esquema XML para gerar classes e XML Schema Documents](xml-schema-def-tool-gen.md).  
  
- Para gerar um documento de esquema XML de um arquivo .dll ou .exe. Para ver o esquema de um conjunto de arquivos que você criou ou um que tenha sido alterado com atributos, passe o DLL ou EXE como um argumento para a ferramenta para gerar o esquema XML. Para obter informações sobre como usar a ferramenta para gerar um XML Schema Document com base em um conjunto de classes, consulte [Como usar a Ferramenta de Definição de Esquema XML para gerar classes e XML Schema Documents](xml-schema-def-tool-gen.md).  
  
Para obter mais informações sobre como usar a ferramenta, consulte [ferramenta de definição de esquema XML (XSD. exe)](xml-schema-definition-tool-xsd-exe.md).  
  
## <a name="see-also"></a>Confira também

- <xref:System.Data.DataSet>
- [Apresentando a serialização XML](introducing-xml-serialization.md)
- [Ferramenta de definição de esquema XML (XSD. exe)](xml-schema-definition-tool-xsd-exe.md)
- <xref:System.Xml.Serialization.XmlSerializer>
- [Como serializar um objeto](how-to-serialize-an-object.md)
- [Como desserializar um objeto](how-to-deserialize-an-object.md)
- [Como usar a ferramenta de definição de esquema XML para gerar classes e documentos de esquema XML](xml-schema-def-tool-gen.md)
- [Suporte à associação de esquema XML](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/sh1e66zd(v=vs.100))
