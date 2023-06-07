---
title: Convertendo cadeias de caracteres em tipos de dados do .NET Framework
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
ms.assetid: 65455ef3-9120-412c-819b-d0f59f88ac09
ms.openlocfilehash: fda5441c58d14b91a9eca16fff9149c8795f95b9
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289220"
---
# <a name="converting-strings-to-net-framework-data-types"></a>Convertendo cadeias de caracteres em tipos de dados do .NET Framework
Se você quiser converter uma cadeia de caracteres para um tipo de dados do .NET Framework, use o método **XmlConvert** que se adapta aos requisitos do aplicativo. Para obter uma lista de todos os métodos de conversão disponíveis na classe **XmlConvert**, confira <xref:System.Xml.XmlConvert>.  
  
 A cadeia de caracteres retornada do método **ToString** é uma versão da cadeia de caracteres dos dados que são passados. Além disso, há vários tipos do .NET Framework que são convertidos usando a classe **XmlConvert** e, ainda assim, não usam os métodos na classe **System.Convert**. A classe **XmlConvert** segue a especificação do tipo de dados XSD (XML Schema) e tem um tipo de dados para o qual o **XmlConvert** pode ser mapeado.  
  
 A tabela a seguir lista os tipos de dados do .NET Framework e os tipos de cadeia de caracteres que são retornados usando o mapeamento do tipo de dados do esquema XSD. Esses tipos do .NET Framework não podem ser processados usando **System.Convert**.  
  
|Tipo de .NET Framework|Cadeia de caracteres retornada|  
|-------------------------|---------------------|  
|Boolean|"true", "false"|  
|Single.PositiveInfinity|"INF"|  
|Single.NegativeInfinity|"-INF"|  
|Double.PositiveInfinity|"INF"|  
|Double.NegativeInfinity|"-INF"|  
|Datetime|O formato é "yyyy-MM-ddTHH:mm:sszzzzzz" e seus subconjuntos.|  
|Timespan|O formato é PnYnMnTnHnMnS isto é, `P2Y10M15DT10H30M20S` é uma duração de 2 anos, 10 meses, 15 dias, 10 horas, 30 minutos e 20 segundos.|  
  
> [!NOTE]
> Se estiver convertendo algum dos tipos do .NET Framework listados na tabela para uma cadeia de caracteres usando o método **ToString**, a cadeia de caracteres retornada não será o tipo base, mas o tipo de cadeia de caracteres do esquema XSD.  
  
 O tipo de valor **DateTime** e **Timespan** diferem porque **DateTime** representa um momento no tempo, enquanto **TimeSpan** representa um intervalo de tempo. Os formatos **DateTime** e **Timespan** são definidos na especificação dos tipos de dados do esquema XML (XSD). Por exemplo:  
  
```vb  
Dim writer As New XmlTextWriter("myfile.xml", Nothing)  
Dim [date] As New DateTime(2001, 8, 4)  
writer.WriteElementString("Date", XmlConvert.ToString([date]))  
```  
  
```csharp  
XmlTextWriter writer = new XmlTextWriter("myfile.xml", null);  
DateTime date = new DateTime (2001, 08, 04);  
writer.WriteElementString("Date", XmlConvert.ToString(date));  
```  
  
 **Saída**  
  
 `<Date>2001-08-04T00:00:00</Date>`.  
  
 O código a seguir converte um inteiro para uma cadeia de caracteres:  
  
```vb  
Dim writer As New XmlTextWriter("myfile.xml", Nothing)  
Dim value As Int32 = 200  
writer.WriteElementString("Number", XmlConvert.ToString(value))  
```  
  
```csharp  
XmlTextWriter writer = new XmlTextWriter("myfile.xml", null);  
Int32 value = 200;  
writer.WriteElementString("Number", XmlConvert.ToString(value));  
```  
  
 **Saída**  
  
 `<Number>200</Number>`  
  
 Entretanto, se você estiver convertendo uma cadeia de caracteres para **Boolean**, **Single** ou **Double**, o tipo do .NET Framework que é retornado não será igual ao tipo retornado ao usar a classe **System.Convert**.  
  
## <a name="string-to-boolean"></a>String para Boolean  
 A tabela a seguir mostra qual o tipo gerado para cadeias de caracteres de entradas consideradas, ao converter uma cadeia de caracteres para **Boolean** usando o método **ToBoolean**.  
  
|Parâmetro de entrada válida de cadeia de caracteres|Tipo de saída do .NET Framework|  
|----------------------------------|--------------------------------|  
|"true"|Boolean.True|  
|"1"|Boolean.True|  
|"false"|Boolean.False|  
|"0"|Boolean.False|  
  
 Por exemplo, considerando o seguinte XML:  
  
 **Entrada**  
  
```xml  
<Boolean>true</Boolean>  
<Boolean>1</Boolean>
```  
  
 Ambos podem ser compreendidos pelo código a seguir e **bvalue** é **System.Boolean.True**:  
  
```vb  
Dim bvalue As Boolean = _  
   XmlConvert.ToBoolean(reader.ReadElementString())  
Console.WriteLine(bvalue)  
```  
  
```csharp  
Boolean bvalue = XmlConvert.ToBoolean(reader.ReadElementString());  
Console.WriteLine(bvalue);  
```  
  
## <a name="string-to-single"></a>String para Single  
 A tabela a seguir mostra qual o tipo gerado para cadeias de caracteres de entradas consideradas, ao converter uma cadeia de caracteres para **Single** usando o método **ToSingle**.  
  
|Parâmetro de entrada válida de cadeia de caracteres|Tipo de saída do .NET Framework|  
|----------------------------------|--------------------------------|  
|"INF"|Single.PositiveInfinity|  
|"-INF"|Single.NegativeInfinity|  
  
## <a name="string-to-double"></a>String para Double  
 A tabela a seguir mostra qual o tipo gerado para cadeias de caracteres de entradas consideradas, ao converter uma cadeia de caracteres para **Single** usando o método **ToDouble**.  
  
|Parâmetro de entrada válida de cadeia de caracteres|Tipo de saída do .NET Framework|  
|----------------------------------|--------------------------------|  
|"INF"|Double.PositiveInfinity|  
|"-INF"|Double.NegativeInfinity|  
  
 O código a seguir grava `<Infinity>INF</Infinity>`:  
  
```vb  
Dim value As Double = Double.PositiveInfinity  
writer.WriteElementString("Infinity", XmlConvert.ToString(value))  
```  
  
```csharp  
Double value = Double.PositiveInfinity;  
writer.WriteElementString("Infinity", XmlConvert.ToString(value));  
```  
  
## <a name="see-also"></a>Veja também

- [Conversão de tipos de dados XML](conversion-of-xml-data-types.md)
- [Convertendo tipos do .NET Framework para cadeias de caracteres](converting-dotnet-types-to-strings.md)
