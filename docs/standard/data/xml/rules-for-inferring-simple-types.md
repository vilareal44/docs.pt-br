---
title: Regras para inferir tipos simples
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 394624d6-4da0-430a-8a88-46efe40f14de
ms.openlocfilehash: 571019d13433312a5d31f581c3527aae901bbba7
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289064"
---
# <a name="rules-for-inferring-simple-types"></a>Regras para inferir tipos simples
Descreve como a classe de <xref:System.Xml.Schema.XmlSchemaInference> infere o tipo de dados para atributos e elementos.  
  
 A classe de <xref:System.Xml.Schema.XmlSchemaInference> infere o tipo de dados para atributos e elementos como tipos simples. Esta seção descreve os tipos inferidos possivelmente, como os valores de diferentes de vários são reconciliados a um único tipo, e como esquema- definindo atributos de `xsi` são tratados.  
  
## <a name="inferred-types"></a>Tipos inferidos  
 A classe de <xref:System.Xml.Schema.XmlSchemaInference> infere valores de elemento e atributo como tipos simples e inclui um atributo de tipo no esquema resultante. Todos os tipos são inferidos tipos simples. Qualquer tipo base ou aspecto são incluído como parte do esquema resultante.  
  
 Os valores são examinados individualmente que são encontrados no documento XML. O tipo é inferido para um valor então é examinado. Se um tipo é inferido de um atributo ou um elemento, e um valor para o atributo ou o elemento está localizado que não corresponde ao tipo atualmente inferido, a classe de <xref:System.Xml.Schema.XmlSchemaInference> eleva tipo para cada um de um conjunto de regras. Essas regras são discutidas na seção da promoção de tipos, posterior neste tópico.  
  
 A tabela a seguir lista os tipos inferidos possíveis para o esquema resultante.  
  
|Tipo simples|Descrição|  
|-----------------|-----------------|  
|booleano|True, false, 0, 1.|  
|byte|Inteiros no intervalo de – 128 a 127.|  
|unsignedByte|Inteiros no intervalo de 0 a 255.|  
|short|Inteiros no intervalo de – 32768 a 32767.|  
|unsignedShort|Inteiros no intervalo de 0 a 65535.|  
|INT|Inteiros no intervalo de – 2147483648 a 2147483647.|  
|unsignedInt|Inteiros no intervalo de 0 a 4294967295.|  
|long|Inteiros no intervalo de – 9223372036854775808 a 9223372036854775807.|  
|unsignedLong|Inteiros no intervalo de 0 a 18446744073709551615.|  
|inteiro|Um número de dígitos finito prefixados possivelmente com “-”.|  
|decimal|Valores numéricos que contêm 0 a 28 dígitos de precisão.|  
|FLOAT|Os decimais opcionalmente seguido por “E” ou “e” tiver usado por um valor inteiro que representa o expoente. Os valores decimais podem estar no intervalo de -16777216 a 16777216. Os valores do expoente podem estar no intervalo de – 149 a 104.<br /><br /> O flutuante permite valores especiais representar a infinito e não valores numéricos. Os valores especiais para o flutuante são: 0, -0, INF, - INF, NaN.|  
|double|O mesmo como flutuam exceto valores decimais podem estar no intervalo de -9007199254740992 a 9007199254740992, e o expoente valor podem estar no intervalo de – 1075 a 970.<br /><br /> O tipo double permite valores especiais representar a infinito e não valores numéricos. Os valores especiais para o flutuante são: 0, -0, INF, - INF, NaN.|  
|duration|O formato de duração W3C.|  
|dateTime|O formato dateTime W3C.|  
|time|O formato de hora W3C.|  
|date|Os valores do ano são impedidos de 0001 a 9999.|  
|gYearMonth|O formato gregoriano mês e ano de W3C.|  
|string|Um ou mais caracteres Unicode.|  
  
## <a name="type-promotion"></a>Promoção de tipos  
 A classe de <xref:System.Xml.Schema.XmlSchemaInference> examina valores de atributo e de um elemento de cada vez. Como valores são encontrados, o tipo mais restritivo, o mais sem sinal é inferido. Se um tipo é inferido de um atributo ou um elemento, e um novo valor está localizado que não corresponde ao tipo atualmente inferido, o tipo é inferido alto para um novo tipo que se aplica a ambos tipo atualmente inferido e o novo valor. A classe de <xref:System.Xml.Schema.XmlSchemaInference> considera valores anteriores para elevar o tipo inferido.  
  
 Por exemplo, considere os seguintes fragmentos XML de dois documentos XML:  
  
 `<MyElement1 attr1="12" />`  
  
 `<MyElement1 attr1="52344" />`  
  
 Quando o primeiro valor de `attr1` é encontrado, o tipo de `attr1` será inferida como `unsignedByte` com base no valor `12`. Quando segundo `attr1` é encontrado, o tipo é promovido a `unsignedShort` com base no tipo atualmente inferido de `unsignedByte` e o valor atual `52344`.  
  
 Agora, considere o seguinte XML de dois documentos XML:  
  
 `<MyElement2 attr2="0" />`  
  
 `<MyElement2 attr2="true" />`  
  
 Quando o primeiro valor de `attr2` é encontrado, o tipo de `attr2` será inferida como `unsignedByte` com base no valor `0`. Quando segundo `attr2` é encontrado, o tipo é promovido a `string` com base no tipo atualmente inferido de `unsignedByte` e o valor atual `true` porque a classe de <xref:System.Xml.Schema.XmlSchemaInference> considera valores anteriores para elevar o tipo inferido. No entanto, se ambas as instâncias de `attr2` foram encontrados no mesmo documento XML e não em dois documentos XML diferentes que ilustradas anterior, `attr2` seria inferido como `boolean`.  
  
### <a name="ignored-attributes-from-the-httpswwww3org2001xmlschema-instance-namespace"></a>Atributos ignorados do namespace <https://www.w3.org/2001/XMLSchema-instance>

Os seguintes esquema- está definindo os atributos que são ignorados durante a inferência de esquema.  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`xsi:type`|Se um elemento é encontrado com `xsi:type` especificou, `xsi:type` é ignorado.|  
|`xsi:nil`|Se um elemento com um atributo de `xsi:nil` é encontrado, sua declaração de elemento no esquema inferido tem o valor de `nillable="true"`. Um elemento com um atributo de `xsi:nil` definido como `true` não pode ter elementos filho.|  
|`xsi:schemaLocation`|Se `xsi:schemaLocation` é encontrado, será ignorado.|  
|`xsi:noNamespaceSchemaLocation`|Se `xsi:noNamespaceSchemaLocation` é encontrado, será ignorado.|  
  
## <a name="see-also"></a>Veja também

- [SOM (Schema Object Model) XML](xml-schema-object-model-som.md)
- [Inferindo esquemas de documentos XML](inferring-schemas-from-xml-documents.md)
- [Regras para inferir tipos de nó e estrutura de esquema](rules-for-inferring-schema-node-types-and-structure.md)
