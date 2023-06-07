---
title: Importação e exportação de esquemas
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, schema import and export
- XsdDataContractExporter class
- XsdDataContractImporter class
ms.assetid: 0da32b50-ccd9-463a-844c-7fe803d3bf44
ms.openlocfilehash: 942ade69d92d8a213f65a3a2e463b6924e2f986e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84590209"
---
# <a name="schema-import-and-export"></a>Importação e exportação de esquemas
Windows Communication Foundation (WCF) inclui um novo mecanismo de serialização, o <xref:System.Runtime.Serialization.DataContractSerializer> . O se `DataContractSerializer` traduz entre objetos de .NET Framework e XML (em ambas as direções). Além do próprio serializador, o WCF inclui mecanismos de importação de esquema e exportação de esquema associados. O *esquema* é uma descrição formal, precisa e legível por computador da forma do XML que o serializador produz ou que o desserializador pode acessar. O WCF usa o XSD (linguagem de definição de esquema XML) World Wide Web Consortium (W3C) como sua representação de esquema, o que é amplamente interoperável com várias plataformas de terceiros.  
  
 O componente de importação de esquema, <xref:System.Runtime.Serialization.XsdDataContractImporter> , usa um documento de esquema XSD e gera .NET Framework classes (normalmente classes de contrato de dados), de modo que os formulários serializados correspondam ao esquema fornecido.  
  
 Por exemplo, o seguinte fragmento de esquema:  
  
 [!code-csharp[c_SchemaImportExport#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#8)]
 [!code-vb[c_SchemaImportExport#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#8)]  
  
 gera o seguinte tipo (um pouco simplificado para melhor legibilidade).  
  
 [!code-csharp[c_SchemaImportExport#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/cs/source.cs#1)]
 [!code-vb[c_SchemaImportExport#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_schemaimportexport/vb/source.vb#1)]  
  
 Observe que o tipo gerado segue várias práticas recomendadas de contrato de dados (encontradas nas [práticas recomendadas: controle de versão de contrato de dados](../best-practices-data-contract-versioning.md)):  
  
- O tipo implementa a <xref:System.Runtime.Serialization.IExtensibleDataObject> interface. Para obter mais informações, consulte [Contratos de dados compatíveis por encaminhamento](forward-compatible-data-contracts.md).  
  
- Os membros de dados são implementados como propriedades públicas que encapsulam campos particulares.  
  
- A classe é uma classe parcial e as adições podem ser feitas sem modificar o código gerado.  
  
 O <xref:System.Runtime.Serialization.XsdDataContractExporter> permite que você faça o inverso — pegue os tipos que são serializáveis com o `DataContractSerializer` e gere um documento de esquema XSD.  
  
## <a name="fidelity-is-not-guaranteed"></a>A fidelidade não é garantida  
 Não há garantia de que o esquema ou os tipos façam uma viagem de ida e volta com fidelidade total. (Uma *viagem* de ida e volta significa importar um esquema para criar um conjunto de classes e exportar o resultado para criar um esquema novamente.) O mesmo esquema não pode ser retornado. A reversão do processo também não é garantida para preservar a fidelidade. (Exporte um tipo para gerar seu esquema e, em seguida, importe o tipo de volta. É improvável que o mesmo tipo seja retornado.)  
  
## <a name="supported-types"></a>Tipos com suporte  
 O modelo de contrato de dados dá suporte a apenas um subconjunto limitado do esquema WC3. Qualquer esquema que não esteja em conformidade com esse subconjunto causará uma exceção durante o processo de importação. Por exemplo, não há como especificar que um membro de dados de um contrato de dados deve ser serializado como um atributo XML. Portanto, os esquemas que exigem o uso de atributos XML não têm suporte e causarão exceções durante a importação, pois é impossível gerar um contrato de dados com a projeção XML correta.  
  
 Por exemplo, o fragmento de esquema a seguir não pode ser importado usando as configurações de importação padrão.  
  
 [!code-xml[c_SchemaImportExport#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_schemaimportexport/common/source.config#9)]  
  
 Para obter mais informações, consulte [referência de esquema de contrato de dados](data-contract-schema-reference.md). Se um esquema não estiver em conformidade com as regras de contrato de dados, use um mecanismo de serialização diferente. Por exemplo, o <xref:System.Xml.Serialization.XmlSerializer> usa seu próprio mecanismo de importação de esquema separado. Além disso, há um modo de importação especial no qual o intervalo de esquema com suporte é expandido. Para obter mais informações, consulte a seção sobre como gerar <xref:System.Xml.Serialization.IXmlSerializable> tipos em [importando o esquema para gerar classes](importing-schema-to-generate-classes.md).  
  
 O `XsdDataContractExporter` oferece suporte a qualquer tipo de .NET Framework que possa ser serializado com o `DataContractSerializer` . Para obter mais informações, consulte [tipos com suporte no serializador de contrato de dados](types-supported-by-the-data-contract-serializer.md). Observe que o esquema gerado usando os `XsdDataContractExporter` dados normalmente são válidos que o `XsdDataContractImporter` pode usar (a menos que <xref:System.Xml.Serialization.XmlSchemaProviderAttribute> seja usado para personalizar o esquema).  
  
 Para obter mais informações sobre como usar o <xref:System.Runtime.Serialization.XsdDataContractImporter> , consulte [importando o esquema para gerar classes](importing-schema-to-generate-classes.md).  
  
 Para obter mais informações sobre como usar o <xref:System.Runtime.Serialization.XsdDataContractExporter> , consulte [exportando esquemas de classes](exporting-schemas-from-classes.md).  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Runtime.Serialization.DataContractSerializer>
- <xref:System.Runtime.Serialization.XsdDataContractImporter>
- <xref:System.Runtime.Serialization.XsdDataContractExporter>
- [Importando o esquema para gerar classes](importing-schema-to-generate-classes.md)
- [Exportando esquemas de Classes](exporting-schemas-from-classes.md)
