---
title: Inferindo esquemas de documentos XML
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
ms.assetid: f3d97d53-614d-4a04-a174-87965b7405f6
ms.openlocfilehash: 5b0f9bea33346083ce0015fbf3cdfeb0e0ea1181
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287656"
---
# <a name="inferring-schemas-from-xml-documents"></a>Inferindo esquemas de documentos XML
Este tópico descreve como usar a classe de <xref:System.Xml.Schema.XmlSchemaInference> para inferir um esquema de linguagem de definição de esquema XML (XSD) da estrutura de um documento XML.  
  
## <a name="the-schema-inference-process"></a>O processo de inferência de esquema  
 A classe de <xref:System.Xml.Schema.XmlSchemaInference> do espaço de <xref:System.Xml.Schema?displayProperty=nameWithType> é usada para gerar um ou vários esquemas do idioma da definição de esquema XML (XSD) da estrutura de um documento XML. Os esquemas gerados podem ser usados para validar o documento XML original.  
  
 Como um documento XML é processado pela classe de <xref:System.Xml.Schema.XmlSchemaInference> , a classe de <xref:System.Xml.Schema.XmlSchemaInference> fazer suposições sobre os componentes do esquema que descrevem os elementos e atributos no documento XML. A classe de <xref:System.Xml.Schema.XmlSchemaInference> também infere componentes do esquema de uma maneira restrito inferindo o tipo mais restritivo para um elemento ou atributo específico. Como obter mais informações sobre o documento XML são coletadas, essas restrições são afrouxadas inferindo tipos menos restritivos. Tipo menos restritivo que pode ser inferido é `xs:string`.  
  
 Veja, por exemplo, a seguinte parte de um documento XML.  
  
```xml  
<parent attribute1="6">  
    <child>One</child>  
    <child>Two</child>  
</parent>  
<parent attribute1="A" />
```  
  
 No exemplo anterior, quando o atributo de `attribute1` é encontrado com um valor de `6` pelo processo de <xref:System.Xml.Schema.XmlSchemaInference> , é assumido ser do tipo `xs:unsignedByte`. Quando o segundo elemento de `parent` é encontrado pelo processo de <xref:System.Xml.Schema.XmlSchemaInference> , a restrição está afrouxada alterando tipo a `xs:string` como o valor do atributo de `attribute1` agora é `A`. Da mesma forma, o atributo de `minOccurs` para todos os elementos de `child` inferidos no esquema é afrouxado a `minOccurs="0"` porque o segundo elemento pai não tiver nenhum elemento filho.  
  
## <a name="inferring-schemas-from-xml-documents"></a>Inferindo esquemas de documentos XML  
 A classe de <xref:System.Xml.Schema.XmlSchemaInference> usa dois métodos sobrecarregados de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A> para inferir um esquema de um documento XML.  
  
 O primeiro método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> é usado para criar um esquema com base em um documento XML. O segundo método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> é usado para inferir um esquema que descreve vários documentos XML. Por exemplo, você pode alimentar vários documentos XML para o método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> um de cada vez para gerar um esquema que descreve o conjunto de documentos XML.  
  
 O primeiro método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> infere um esquema de um documento XML contido em um objeto de <xref:System.Xml.XmlReader> , e retorna um objeto de <xref:System.Xml.Schema.XmlSchemaSet> que contém o esquema inferido. O segundo método de <xref:System.Xml.Schema.XmlSchemaInference.InferSchema%2A?displayProperty=nameWithType> procura um objeto de <xref:System.Xml.Schema.XmlSchemaSet> por um esquema com a mesma namespace de destino que o documento XML contido no objeto de <xref:System.Xml.XmlReader> , refinar o esquema existente, e retorna um objeto de <xref:System.Xml.Schema.XmlSchemaSet> que contém o esquema inferido.  
  
 As alterações feitas ao esquema mais aguçado são baseadas na nova estrutura encontrada no documento XML. Por exemplo, como um documento XML é transmitido, as suposições são feitas sobre os tipos de dados encontrada, e o esquema é criado com base nessas suposições. No entanto, se os dados estão localizados em uma segunda passada de inferência suposição que seja diferente do esquema original, é mais aguçado. O exemplo a seguir ilustra o processo de refinamento.  
  
 [!code-cpp[XmlSchemaInferenceExamples#4](../../../../samples/snippets/cpp/VS_Snippets_Data/XmlSchemaInferenceExamples/CPP/XmlSchemaInferenceExamples.cpp#4)]
 [!code-csharp[XmlSchemaInferenceExamples#4](../../../../samples/snippets/csharp/VS_Snippets_Data/XmlSchemaInferenceExamples/CS/XmlSchemaInferenceExamples.cs#4)]
 [!code-vb[XmlSchemaInferenceExamples#4](../../../../samples/snippets/visualbasic/VS_Snippets_Data/XmlSchemaInferenceExamples/VB/XmlSchemaInferenceExamples.vb#4)]  
  
 O exemplo a seguir utiliza o arquivo, `item1.xml`, como a primeira entrada.  
  
 [!code-xml[XmlSchemaInferenceExamples#13](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/item1.xml#13)]  
  
 O exemplo usa o arquivo de `item2.xml` como sua entrada segunda:  
  
 [!code-xml[XmlSchemaInferenceExamples#14](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/item2.xml#14)]  
  
 Quando o atributo de `productID` está localizado no primeiro documento XML, o valor de `123456789` é assumido ser um tipo de `xs:unsignedInt` . No entanto, quando o segundo documento XML é ler e o valor de `A53-246` é encontrado, o tipo de `xs:unsignedInt` não pode mais ser adotado. O esquema é mais aguçado e o tipo de `productID` é alterado para `xs:string`. Além disso, o atributo de `minOccurs` para o elemento de `supplierID` é definido como `0`, porque o segundo documento XML não contém elementos de `supplierID` .  
  
 O seguinte é o esquema inferido do primeiro documento XML.  
  
 [!code-xml[XmlSchemaInferenceExamples#15](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/InferSchema1.xml#15)]  
  
 O seguinte é o esquema inferido do primeiro documento XML, mais aguçado pelo segundo documento XML.  
  
 [!code-xml[XmlSchemaInferenceExamples#16](../../../../samples/snippets/xml/VS_Snippets_Data/XmlSchemaInferenceExamples/XML/InferSchema2.xml#16)]  
  
## <a name="inline-schemas"></a>Esquemas in-line  
 Se um esquema de cores do idioma da definição de esquema XML (XSD) é encontrado durante o processo de <xref:System.Xml.Schema.XmlSchemaInference> , <xref:System.Xml.Schema.XmlSchemaInferenceException> é lançada. Por exemplo, o seguinte esquema embutido gerencie <xref:System.Xml.Schema.XmlSchemaInferenceException>.  
  
```xml  
<root xmlns:ex="http://www.contoso.com" xmlns="http://www.tempuri.org">  
    <xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" targetNamespace="http://www.contoso.com">  
        <xs:element name="Contoso" type="xs:normalizedString" />  
    </xs:schema>  
    <ex:Contoso>Test</ex:Contoso>  
</root>  
```  
  
## <a name="schemas-that-cannot-be-refined"></a>Esquemas que não podem ser refinados  
 Há construções de Esquema XML do W3C que o processo de <xref:System.Xml.Schema.XmlSchemaInference> do idioma da definição de esquema XML (XSD) não pode manipular se um determinado tipo para refinar e causar uma exceção seja lançada. Como um tipo complexo cujo compositor de nível superior é algo diferente de uma sequência. No modelo de objeto (SOM) do esquema, isso corresponde a <xref:System.Xml.Schema.XmlSchemaComplexType> cuja propriedade de <xref:System.Xml.Schema.XmlSchemaComplexType.Particle%2A> não é uma instância de <xref:System.Xml.Schema.XmlSchemaSequence>.  
  
## <a name="see-also"></a>Veja também

- <xref:System.Xml.Schema.XmlSchemaInference>
- [SOM (Schema Object Model) XML](xml-schema-object-model-som.md)
- [Inferindo um esquema XML](inferring-an-xml-schema.md)
- [Regras para inferir tipos de nó e estrutura de esquema](rules-for-inferring-schema-node-types-and-structure.md)
- [Regras para inferir tipos simples](rules-for-inferring-simple-types.md)
